---
layout: page
active: lectures
title: "Lecture 21: Texture Filtering"
auto-title: true
---



## Texels

For most of these discussions, it's a good idea to drop the notion that a picture is an array of square colored pixels.
Instead, think of a texture as an array of sample values with some distance between samples, e.g. a **frequency**.

In additionl, each element of a texture is called a **texel** instead of a **pixel**.



## Texture Parameters

When creating textures we have to apply some parameters.
These parameters affect how the texture is accessed by `texture()` calls in shaders.

### Wrap

The wrap mode specifies what happens if we access the texture outside of the $$ 0, 0 $$ to $$ 1, 1 $$ range.

![21-figure-wrap](21-figure-wrap.png)


## Filtering

Filtering affects how the texture appears if it is larger than or smaller than the screen area where the texture is visible.
In a 3D application, one or the other is pretty much always the case.

Magnification filtering is the easiest to think about and display.
If we zoom in on the texture, does it look blurry or do we start to see the pixels as large squares?

![21-figure-mag-filter](21-figure-mag-filter.png)

Minification has to do with happens when the texture is very far away from the camera.
What do we do if a given pixel on the screen covers multiple texels?


We can either do a linear filter of the covered texels, or we can pick the nearest one.
Picking the nearest texel can make textures look noisy when they are moving slightly at a distance.


## Mip Maps

If a texture becomes very far away, there are many reasons that we might want to do something special.
If the texture object fits within a single pixel on the screen, should we do a linear blend of all colors in the texture?
If not, the results will be noisy.

And in general, it is inefficient to use a high resolution texture for very far away objects.

Enter mip maps.
Mip maps are an array of textures representing an object at half, quarter, eight, etc. resolution all the way down to a single texel.
These mip map layers can be selected at draw time (automatically by OpenGL) to pick the appropriate size for the distance of the object.
This is faster, since we are using smaller textures for distance objects, and looks better,
since we are sampling a texture that is the appropriate size for the object in question.

![21-figure-mipmap](21-figure-mipmap.png)
