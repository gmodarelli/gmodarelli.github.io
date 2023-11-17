---
title: Character Creator
subtitle: The Wagadu Chronicles
summary: In-game Character Creator for The Wagadu Chronicles.
tags:
  - Tech Art
  - Unity
  - Blender

# Date published
date: '2023-11-17T14:00:00Z'

external_link: ''

image:
  caption: The Wagadu Chronicles, Twin Drums
  focal_point: Smart

links:
  - icon: steam
    icon_pack: fab
    name: Wishlist The Wagadu Chronicles
    url: https://store.steampowered.com/app/2464400/The_Wagadu_Chronicles/
---

## Introduction

At Twin Drums I was responsible for the asset pipeline of our characters, from the assets organization in Blender, to the in-game character creator written in C#.

Together with the character artists, we came up with a way to split up our characters so that we could reuse as much of the existing assets as possible, without having to create new ones for every body feature combinations.

As for the various body parts, we also wanted to have a single rig to reuse the same animations for all of our characters. To that hand I've created a main rig for the initial body shape of our characters and then duplicated it so that it would adapt to the second body shape we introduced to the game.
The two rigs shared the exact same bones hierarchy, which made it possible to retarget animations from one to the other, with minor manual tweaks.

![Riggify Rigs](/images/cc/rigs.png)

![Rig 1](/images/cc/rig_1.png)

## Blender Character Assembler

In order to author animations, clothes and accessories, I've created a Blender AddOn that our artists and animators would use to assemble a character in Blender to use as base for their work.

This tool served many purposes:

- Skin clothes and accessories to different body shapes
- Author animations on the same assets that would then be assembled by the game
- Automatic FBX export to Unity to ensure right settings and naming conventions
- Bulk export of all character art assets - body parts, hairstyles, clothes, accessories, rigs and animations - whenever a change to the rigs was made

Here's a quick video showing the Character Assembler in Blender

{{< youtube kgKIwx2TQSQ >}}

## In-Engine Character Assembler

WIP

## In-Game Character Creator

WIP