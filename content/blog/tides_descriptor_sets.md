+++
title = "Descriptor Sets Management"
description = "How we manage descriptor sets for Tides of Revival"
date = "2025-06-02"

[taxonomies]
tags = ["DirectX 12", "graphics programming", "Tides of Revival"]

[extra]
local_image = "img/tidesrpg.jpg"
+++

I'm writing the renderer for the Immersive open world RPG [Tides of Revival](https://github.com/Srekel/tides-of-revival). Here is a screenshot from the current version of the game.

{{ full_width_image(src="img/tidesrpg.jpg", alt="Tides of Revival") }}

In today's post I want to share my approach to GPU resource bindings.

The DirectX 12 renderer for the game relies on bindless and HLSL 6.6 Dynamic Resources for almost all GPU resources.
Still, there are some resources that need to be bound to shaders. For those, we have to use descriptors.

Tides' renderer is written on top of a custom fork of [The-Forge](https://github.com/gmodarelli/The-Forge/tree/ze-forge), so we didn't have to write our own management code for heaps and descriptor handles (among many other things), and could instead focus on building a GPU abstraction layer tailored to the needs of the game.

_I'll talk about why and how we use The-Forge in a future post, so stay tuned!_

I wanted to build a thin GPU-resource binding abstraction layer that would satisfy two main goals:

- Make it easy to bind resources (textures, buffers, samplers, etc.) without having to deal with lower-level descriptors and descriptor sets data structures
- Make the `HLSL` code the place where we declare resources, without having to duplicate their declarations in `Zig`

To achieve both goals I extended The-Forge's shader compilation pipeline.
HLSL shaders are first compiled offline via `DXC` and their blobs are loaded during application startup.

After a shader blob is succesfully loaded, a list of `ShaderReflectionDescriptor`s is generated from each `D3D12_SHADER_DESC.BoundResources`. You can find the full source code on [Github](https://github.com/gmodarelli/The-Forge/blob/ze-forge/Common_3/Graphics/Direct3D12/Direct3D12_cxx.cpp#L133), but here is what the struct looks like:

```C
typedef struct ShaderReflectionDescriptor
{
    const char Name[32];            // Resource identifier used as binding key from higher-level APIs
    D3D_SHADER_INPUT_TYPE Type;     // The fields from the `D3D12_SHADER_INPUT_BIND_DESC` struct.
    D3D_SRV_DIMENSION Dimension;
    uint32_t BindPoint;
    uint32_t BindCount;
    uint32_t Space;
} ShaderReflectionDescriptor;
```

The list of `ShaderReflectionDescriptor`s is then processed to generate `Descriptor`s that get associated to one of 4 `DescriptorSet`s. The full source code is available [here](https://github.com/gmodarelli/The-Forge/blob/ze-forge/Common_3/Graphics/Direct3D12/Direct3D12.c#L4448), but here's the gist of what's going on with a real example from a gaussian blur shader we're using in the game.

Any given shader can declare resources that belong to one of 4 "spaces", where each space has a different update frequency (per-draw, per-pass, per-frame and persistent).

```HLSL

struct BlurData { ... };

cbuffer g_CBO : register(b0, SPACE_PerFrame)
{
    BlurData g_blur_data;
};

Texture2D<float4> g_input : register(t0, SPACE_Persistent);
RWTexture2D<float4> g_output : register(u1, SPACE_Persistent);

// Rest of the shader
```

This HLSL code will generate 1 per-frame and 1 persistent `DescriptorSet`s. The first set will contain a single `Descriptor` for a constant buffer named `g_CBO`, the second will contain two `Descriptor`s, one for a read-only 2D texture named `g_input` and one for a read-write 2D texture named `g_output`.

From `Zig` we can then associate GPU resources handles to descriptors in descriptor sets.

**NOTE**: _This code is part of the graphics abstraction layer and there are data types and APIs that are still work in progress. I might write about in the future._

```ZIG
// Per-frame descriptors
for (0..zf.frames_in_flight_count) |frame_index| {
    const resource_binding_descs = [_]zf.ResourceBindingDesc{
        .{
            .name = "g_CBO",
            .binding_type = .buffer,
            .buffer_handle = gfx.gauss_blur_constant_buffers[frame_index],
        },
    };

    zf.updateDescriptorSet(
        &resource_binding_descs,
        .per_frame,
        @intCast(frame_index),
        gfx.gauss_blur_vertical_shader,
        gfx.gauss_blur_vertical_material.passes[0].per_frame_descriptor_set
    );
}

// Persistent descriptors
{
    const resource_binding_descs = [_]zf.ResourceBindingDesc{
        .{
            .name = "g_input",
            .binding_type = .render_target,
            .render_target_handle = gfx.gbuffer0,
        },
        .{
            .name = "g_output",
            .binding_type = .render_texture,
            .render_texture_handle = gfx.gauss_blur_a,
        },
    };

    zf.updateDescriptorSet(
        &resource_binding_descs,
        .persistent,
        0,
        gfx.gauss_blur_horizontal_shader,
        gfx.gauss_blur_horizontal_material.passes[0].persistent_descriptor_set
    );
}
```

There is still a bit of duplication since if you rename a resource in a shader, you need to update the binding code on `Zig` as well. But my plan is to use as few shaders as possibile for the game, so the need for more complex solutions (like code-gen or a more data-driven approach) is not needed just yet.

If you're interested in graphics programming and what to chat about the renderer of Tides of Revival, you can find me on our [Discord Server](https://discord.gg/t2SUa4ng).