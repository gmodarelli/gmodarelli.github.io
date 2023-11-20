---
title: Day Night Cycle
subtitle: The Wagadu Chronicles
summary: A Day Night Cycle system for the Afrofantasy MMORP The Wagadu Chronicles by Twin Drums.
tags:
  - Tech Art
  - Unity
  - The Wagadu Chronicles

# Date published
date: '2023-11-15T14:00:00Z'

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

This is one of the first systems I've developed at [Twin Drums](https://www.twindrums.com) for the Afrofantasy MMORPG [The Wagadu Chronicles](https://store.steampowered.com/app/2464400/The_Wagadu_Chronicles/).

The Day Night Cycle has gone through many iterations, but what I present here is the final version that's shipped in the Early Access of the game on Steam.

A day in Wagadu lasts 24 hours like here on Earth, but in-game those 24 hours pass by in 20 minutes. This is important for gameplay reasons, as there are things you can only do at night in the game and we wanted our players to experience the full set of features in a normal play session.

Here's a video of 24 hours passing by in Wagadu in just 30 seconds.

{{< youtube 7GaWbxUoD7Q >}}

The system allows for many customizations. Here's a few of them:

- duration in minutes of a 24-hours Wagadian day
- daytime vs nighttime speed curve
- light color gradient and light intensity curve
- ambient light type and gradients
- fog color gradient and density curve
- post-process override curves for specific effects (like Bloom)

## Night Bioluminescence

One of my favorite feature of the Day Night Cycle is Night Bioluminesence: at night plants start to glow, and we can control the intensity of the glow by a setting on the plants' materials and the Bloom Threshold and Intensity curves, as I demostrate in the video below (the exaggerated bloom intensity is for demonstration purpose alone, don't do this at home!).

{{< youtube oFq7FXuKNLg >}}