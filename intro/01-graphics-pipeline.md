---
layout: page
active: lectures
title: "Chapter 1: The Graphics Pipeline"
auto-title: true
---


## Overview

The purpose of this chapter is discuss a way to draw shapes/geometry to 2D screen.
The 2D screen is the monitor that is attached to your computer.
The shapes and geometry are what we want to draw.
We are going to focus on triangles in this part,
though points and lines are other shapes that you could draw.
Triangles are the main type of shape, or "primitive", that is rasterized by most rasterization pipelines.
Triangles are 3D shapes with some useful properties, and most other shapes can be created using multiple triangles.

The Graphics Pipeline is the process by which we convert a description of a 3D scene into a 2D image that can be viewed.
This involves both **transformations between spaces** and **conversions from one type of data to another**.

## Spaces

What is a space?
Consider if you and I are standing in a room, 6 feet apart and facing the same direction.
Exactly half way inbetween us is a table, and I am on the left of the table.
If I were to answer "where is the table", I would likely say the table is 3 feet to my right.
But you would say the table is 3 feet to your left.
Neither of us is wrong, we're just describing relationships in space using a difference reference point.
We might say that I am describing the position of the table in "my coordinates"
while you are describing it in "your coordinates".

In computer graphics, we sometimes call these two different coordinate systems spaces.
A lot (seriously, a lot) of computer graphics has to do with transforming geometry between spaces.
Usually, the spaces are different in more complicated ways
(consider if you turned yourself 45 degrees, and I flipped over and did a headstand -
our descriptions of the table's position would now be more complicated and different).

It's nice to have a common frame of reference for these things.
Usually, we describe the positions of most things in our scene in terms of "world coordinates".
This "world space" has a common reference point, or **origin**,
from which the positions of all other objects are described.
The world coordinate system also has a sense of directionality, e.g. which way is up,
but we'll discuss this later.

In our rasterizers, our task will be to take triangles with coordinates stored in world space,
and transform them into pixels that we can color on the screen.

<img src="01-figure-coordinate-frame.png" alt="coordinate frame" class="img-thumbnail" />

## Types of Geometry

As discussed, the elements of our scenes will usually be primitive geometry,
specifically either points, lines, or triangles.
The geometry can be expressed in 2D coordinates,
but usually we'll be talking about 3D geometry.

The output of our rasterizer will be an image that can be viewed.
This image will be a two-dimensional array of color values.
These color values are called pixels, or "picture elements".

We'll usually talk about these pixels as a x and y coordinate on the screen.
These coordinates will be integers, and will range from 0 to the width or height of the screen.

$$ 0 \le x_p \le w - 1 $$

$$ 0 \le y_p \le h - 1 $$

<img src="01-figure-pixel-coordinates.png" alt="pixel coordinates" class="img-thumbnail" />

We need a way to transform from world to pixel.



### General Graphics Pipeline

<img src="01-figure-graphics-pipeline.png" alt="graphics pipeline" class="img-thumbnail" />

**vertex array:** what are the vertices

- input specification

**vertex shader:** move to camera's perspective

object space `-->` screen coordinates

**rasterization:** which pixels are inside triangle?

**fragment shader:** what color is each pixel?

**testing and blending:** which pixels are visible?



## Software Rasterizer

1. Read in triangles
2. Convert triangls to windows coordinates
3. Rasterize each triangle
  - (use barycentric coordinates to test in-triangle AND interpolat colors)
4. Write interpolated color values per pixel
  (using a z-buffer test to resolve depth)

### Labs

#### Lab 1
- Compute bounding box
- Draw box and vertices

#### Lab 2
- Compute barycentric
- Check is-interior?
- Interpolate colors
