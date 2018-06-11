---
layout: page
active: lectures
title: "Lecture 20: Final Project Grab-Bag"
auto-title: true
---


## Global Illumination

[Slide Deck](https://docs.google.com/presentation/d/1MWH-IcK2I0JK39sADBSA0qj9zyM2W9JWPYme-MKkUNQ/edit?usp=sharing){:class="btn btn-info"}

### Ambient Occlusion

**Ambient occlusion** accounts for the shadowing of indirect light caused by nearby geometry.
It is very similar to Monte Carlo indirect lighting except instead of adding light reflected by nearby light, you shadow a fragment based on the number of nearby occluding samples.


### Radiosity

**Radiosity** is a global illumination technique in which the scene is supdivided into small geometry samples and then light is iteratively bounced throughout the scene.
All geometry is assumed to be perfectly diffuse.
Rendering finishes when the simulation converges - e.g. further bouncing does not change the illumination.


### Photon Mapping

**Photon mapping** is a global illumination technique that is able to render advanced lighting effects such as caustics.
First, photo particles are cast into the scene by the light sources and recursively bounced, storing hit information throughout the scene.
Then, a gather step illuminates parts of the scene based on how many photos hit that area.


### Path Tracing

**Path tracing** is a very slow but very accurate technique for rendering images with global illumination.
It supports caustics just like photon mapping.
In some ways photon mapping can be considered an optimization/approximation of path tracing.

Path tracing involves generating many, many paths between the camera and the scene light sources.
You can generate these paths either by starting at the camera and tracing outwards towards lights
or by starting at the light and tracing towards the camera.
You can also start paths at both the light and camera and connect them in the middle - this is called bidirectional path tracing.
A large number of rays is necessary to generate accurate images without too much noise.

- [Reference for bidirectional path tracing](https://graphics.stanford.edu/courses/cs348b-03/papers/veach-chapter10.pdf)


### Soft/Area Lights

**Soft lights** can be implemented by casting multiple shadow feelers and summing illumination for multiple light vectors



## Distribution Ray Tracing

[Slide Deck](https://docs.google.com/presentation/d/1Ne_-WUN0DvDOIJFrXX1wHDQFSItCCIGum3LsMjc5FC4/edit?usp=sharing){:class="btn btn-info"}

**Distribution ray tracing** is a general methodology that includes using a distribution of random samples to simulate a number of effects.
It is dicussed on slides 40-49 of [this slide deck](https://docs.google.com/presentation/d/1FaixfnT0TACwkWryQnqlD7PtdMg8eaSW7YKDKegKAW0/edit?usp=sharing).

### Soft Shadows

**Soft shadows**, just like soft lights and area lights, can be simulated by sampling random light/shadow rays.

### Depth of Field

**Depth of field** can be simulated by sampling a number of random camera rays from a given pixel square and through a particular focus plane.

### Motion Blur

**Motion blur** can be simulated by sampling a number of random time samples where an object is at different positions for each time.

### Glossy Reflections (and Refractions)

**Glossy reflections** and also refractions can be simulated by sampling a number of randomly perturbed reflection or refraction vectors.



## Geometry

### Constructive Solid Geometry

**Constructive Solid Geometry**, or CSG, describes geometry as a tree of set operations (e.g. union, interscetion, difference) applied to geometric primitives.
It can be rendered by finding the intersection range for each object (e.g. the "enter" and "exit" intersection) and applying the set operations to those ranges!

- [Reference for constructive solid geometry](http://web.cse.ohio-state.edu/~parent.1/classes/681/Lectures/19.RayTracingCSG.pdf)
