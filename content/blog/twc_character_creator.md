+++
title = "Character Creator"
description = "In-game Character Creator for The Wagadu Chronicles."
date = "2023-11-17"

[taxonomies]
tags = ["unity", "blender", "tech art", "The Wagadu Chronicles"]

[extra]
local_image = "img/character_creator.jpg"
+++

## Introduction

At Twin Drums I was responsible for the asset pipeline of our characters, from the assets organization in Blender, to the in-game character creator written in C#.

Together with the character artists, we came up with a way to split up our characters so that we could reuse as much of the existing assets as possible, without having to create new ones for every body feature combinations.

As for the various body parts, we also wanted to have a single rig to reuse the same animations for all of our characters. To that hand I've created a main rig for the initial body shape of our characters and then duplicated it so that it would adapt to the second body shape we introduced to the game.
The two rigs shared the exact same bones hierarchy, which made it possible to retarget animations from one to the other, with minor manual tweaks.

{{ full_width_image(src="img/cc/rigs.png", alt="Riggify Rigs") }}

{{ full_width_image(src="img/cc/rig_1.png", alt="Rig 1") }}

## Blender Character Assembler

In order to author animations, clothes and accessories, I've created a Blender AddOn that our artists and animators would use to assemble a character in Blender to use as base for their work.

This tool served many purposes:

- Skin clothes and accessories to different body shapes
- Author animations on the same assets that would then be assembled by the game
- Automatic FBX export to Unity to ensure right settings and naming conventions
- Bulk export of all character art assets - body parts, hairstyles, clothes, accessories, rigs and animations - whenever a change to the rigs was made

Here's a quick video showing the Character Assembler in Blender.

{{ youtube(id="kgKIwx2TQSQ")}}

## In-Engine Character Assembler

As counterpart to the Blender Character Assembler, I've built a Unity Character Assembler to validate all the assets produced for characters, not only bodies, hairstyles and clothes, but also weapons, tools and animations.

This tool then also became the runtime client of the game's Character Creator. The same C# class used in the editor is also used in the shipped version of the game.

Here's a quick video showing the Character Assembler in Unity.

{{ youtube(id="qnX7ROH5IzM")}}

## In-Game Character Creator

You can see the Character Creator in action in this Early Access Trailer video

{{ youtube(id="hUWx3kuRMAM")}}