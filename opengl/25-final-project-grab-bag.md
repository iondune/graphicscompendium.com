---
layout: page
active: lectures
title: "Lecture 25: Final Project Grab-Bag"
auto-title: true
---

<a href="https://drive.google.com/open?id=1Qkcfy328nOsn2LFyM0ba2NaI1Ri0lCPSIAymPs29xL4" class="btn btn-info">Skinned Meshes, Terrain, Skyboxes</a>

<a href="https://drive.google.com/open?id=1A2HqB7SmpfqNt_z6va2FjTpPHodlzSrGMo21UrimnRo" class="btn btn-info">Shadow Mapping</a>

<a href="https://drive.google.com/open?id=1xurgjYOEMrvzFKmP3JzL_4j5J__AFfr3pk4elCdepD8" class="btn btn-info">Deferred Shading</a>

<a href="https://drive.google.com/open?id=1iC4yEGBnj856JGb50ccxVOQS7b_D9q2BAVPmaEEK0i0" class="btn btn-info">Ambient Occlusion</a>

<a href="https://drive.google.com/open?id=12ufwW0S-mLxtesVS_TgDHZrDsKuWzzvaxTBZ97C14SE" class="btn btn-info">Physically-Based Rendering</a>

<a href="https://drive.google.com/open?id=1uZ9InL7XoBo0FJo2aqRqDC1T-nS69lgxaFU3FX4ZRMM" class="btn btn-info">Particle Systems</a>

<a href="https://drive.google.com/open?id=161wuu9Nu_dp425D9f3v-QfF6Eqcv6cBlKwg3dQQs7Mk" class="btn btn-info">Collision Detection</a>












Skyboxes
--------


With a skybox there are two goals:

1. Make the skybox appear behind everything (if the sky is "in front" of something then there is a problem...)
2. Make the skybox appear to not move (you can't get "closer" to the sky)

In practice both of these objectives can be accomplished by simply making the skybox very large, but there are a couple of problems with that.

The "proper" way to make a skybox is by:

1. Using a trick in the vertex shader to make the skybox always behind everything
2. Make the skybox move along with the camera so it is always around the camera

With both of these accomplished, the skybox can be quite small, e.g. a single unit cube.

How do we do these things?

First, make this the last line in your vertex shader:

```glsl
gl_Position.z = gl_Position.w;
```

or

```glsl
gl_Position = gl_Position.xyww;
```

This makes it so that the ndc position of the vertex is `1` in **Z**, meaning it is as far away from the camera as possible.
This is because of the perspective/homogenous divide done by OpenGL to get coordinates from clip space into ndc.

Next, either set the skybox position to the camera position before rendering, or (sneaky) extract only the rotational portion (discarding the translation portion) of the view matrix and using that instead of the usual view matrix when rendering the skybox.

This line can be used to create a view matrix without translation components, in a vertex shader:

```glsl
mat4 ViewRotation = mat4(mat3(View));
```

