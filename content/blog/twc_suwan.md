+++
title = "Suwan"
description = "Procedural toolset to generate islands for the Afrofantasy MMORP The Wagadu Chronicles by Twin Drums."
date = "2023-11-16"

[taxonomies]
tags = ["unity", "procgen", "tech art", "The Wagadu Chronicles"]

[extra]
local_image = "img/suwan.png"
+++

## Introduction

Suwan is a set of procedural tools I have developed together with our level designer, Jennika, at [Twin Drums](https://www.twindrums.com) for the Afrofantasy MMORPG [The Wagadu Chronicles](https://store.steampowered.com/app/2464400/The_Wagadu_Chronicles/).
The world of the game is made up of tens of islands that appear once in the game and then disappear when a new cycle (in the game's lore) begins, living space for new islands.

Since we are an indie company with a small budget and a small team, we knew from day one we needed some procedural tools to help us produce the islands for the game.

Here is a video overview of how Suwan works.

{{ youtube(id="MxajoV5Tu9w") }}

## Features

The biggest strength of Suwan is that it enables Jennika to never leave the editor and to be in a constant flow while working on the islands.
Here is a list of some of the features offered by Suwan:

- Map generation and editing
- Unity Terrain generation and painting
- Ocean and Lakes generation, with SDF-driven shores detection
- Cliffs and ramps generation to connect different terrain elevations
- Biomes and sub-biomes vegetation scattering
- Gameplay resources scattering
- NPC encounters scattering
- Minimap generation

## Technical Details

The main goal of Suwan has always been to improve productivity. Together with Jennika we have identified the most time consuming tasks in her manual process and constantly replaced those with procedural or automated tools.
Once we had a set of tools to replace the most time-consuming and manual tasks, I focused on performance: if the tools are slow and unresponsive they break your flow and make you less productive.

At the heart of Suwan is a set of Compute Shaders that take as input all the parameters Jennika needs to define an island, and generate a set of 2D Textures that encode variours landscape features: elevation, terrain/water type, sub-biomes, scattered vegation and so on.

These 2D textures are then fed to other C# systems to generate their 3D, in-engine counterparts: painted terrain tiles with lakes and oceans, scattered vegetation, interactable resources and NPCs and more.