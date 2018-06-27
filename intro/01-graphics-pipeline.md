---
layout: page
active: lectures
title: "Lecture 1: The Graphics Pipeline"
auto-title: true
---


## Overview

**Motivation:** Draw shapes/geometry to 2D screen.

The Graphics Pipeline is the process by which we convert a description of a 3D scene into a 2D image that can be viewed.

This involves both **transformations between spaces** and **conversions from one type of data to another**.

- Spaces
  - What is a space? Left/right perspective example
  - Where do we store geometry of scene?
    - "World" coordinates

<img src="01-figure-coordinate-frame.png" alt="coordinate frame" class="img-thumbnail" />

- Types of Data
  - "Primitives"
    - Simple kinds of figures/geometry
  - Triangle meshes
  - What is a picture?
    - pixels, "picture elements"

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
