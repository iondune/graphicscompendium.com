---
layout: page
active: lectures
title: "Lecture 2: Camera"
auto-title: true
---

## Overview

Our ray tracing task requires us to:

1. Compute a "viewing ray" for each pixel in the output image,
2. Find the closest intersection point in the scene
3. Set the pixel value based on material (and later, on lighting)

**So what is a ray?**

A starting point $$p_0$$ and a direction (normalized) $$\hat d$$

The ray is represented parametrically using $$t$$ for time:

$$p(t) = p_0 + t * \hat d$$

In general, we test for intersections against geometric objects by solving for valid values of $$t$$



## Image Plane

Given some camera specification, we need to be able to compute view rays.
Let's assume that our camera is sitting somewhere along the positive Z axis and is looking towards -Z.
We want to define some image plane in 3D space.
Then, we can map a given pixel in our output image to a point in this image plane, and use that to find the ray for that particular pixel.

![Camera Image Plane]({{ site.baseurl }}/images/povraycam.png)

For a given pixel $$i, j$$, let's define a set of coordinates coordinates $$U_s, V_s$$.
We'll call these the "view space coordinates".
Where $$w$$ and $$h$$ are the output image width, height:

$$U_s = \frac{i}{w}$$

$$V_s = \frac{j}{h}$$

This will give us an image plane that stretches from $$(0, 0)$$ to $$(1, 1)$$ in view space coordinates.
But, we want the image plane to stretch from $$(-1/2, -1/2)$$ to $$(1/2, 1/2)$$.
We can accomplish this quite simply by shifting our coordinates:

$$ U_s = \frac{i}{w} - \frac{1}{2} $$

$$ V_s = \frac{j}{h} - \frac{1}{2} $$

Another problem with these equations is that we are getting the *lower-left corner of each pixel box* instead of the centers.
Let's say that we want to get the pixel centers (there are a few reasons why we want to, though the resulting image will be nearly the same):

$$ U_s = -\frac{1}{2} + \frac{i + 0.5}{w} $$

$$ V_s = -\frac{1}{2} + \frac{j + 0.5}{h} $$

There's one other problem here.
The images that we are rendering will typically be rectangular, not a perfect square.
Most of the time we will render `640x480` images in this class, which has an aspect ratio of `4:3`.

If we use this square image plane we will get squashed images that distort the relationship between horizontal and vertical scale.
There are two equivalent ways to solve this problem.
We could stretch the image plane along the horizontal axis by $$ 4/3 $$.

However, we will eventually be multiplying these view space coordinates by the basis vectors of our camera.
We can use a "right" basis vector with length $$ 4/3 $$ to account for the aspect ratio at that time.
If you take a look at any of the `.pov` files for this class, you will notice that the "right" vectors are scaled in this way.



## View Space

### Definitions

It is common to define a coordinate system using a set of orthogonal basis vectors.
Our world space coordinate system is defined by the orthogonal basis vectors $$\hat x$$, $$\hat y$$, and $$\hat z$$.
We will define our view space basis vectors $$\vec u$$, $$\hat v$$, and $$\hat w$$.
These are often called the "camera basis vectors".

We can find our basis vectors using the camera specification in our `.pov` files:

```POV-Ray
camera {
    location <0, 0, 14>
    up <0, 1, 0>
    right <1.333, 0, 0>
    look_at <0, 0, 1>
}
```

$$\vec u$$ is our `right` vector (note that it is _not_ normalzied).

$$\hat v$$ is our `up` vector.

Here we run into a problem, however.

`location` and `look_at` describe positions, not vectors, but there is another problem.

We have a right-handed coordinate system, which means that $$ \hat w $$ must be the right-handed cross product of $$\vec u$$ and $$\hat v$$.

However, that gives us a $$\hat w$$ that points in the **_opposite_** direction of where the camera is looking.

We will account for this by defining $$\hat w$$ as the _opposite_ of the "look" vector $$\hat l$$, which is the direction the camera is actually looking.

$$ \hat w = - \hat l $$

The look vector can be found by simply normalizing the difference between `look_at` and `location`:

$$ \hat l = \frac{look\_at - location}{|| look\_at - location ||} $$


### Transformations

Given some coordinate in view space, $$ P_v = U_s, V_s, W_s $$, we can find that position transformed into world space, $$ P_w $$, using the following equation:

$$ P_w = C_0 + U_s * \vec u + V_s * \hat v + W_s * \hat w $$

Where $$C_0$$ is the origin of the view space system expressed in world space coordinates, otherwise known as the "camera position".

We have our $$U_s$$ and $$V_s$$ and defined above, but what is $$ W_s $$?

Geometrically, this is the distance the image plane is pushed along the viewing axis.
This distance is usually referred to as the **focal length**.
In this class we will use a focal length of 1 since that results in a reasonable field of view and is also simple.
Remember that our $$\hat w$$ basis vector points in the opposite direction, so we define:

$$ W_s = - focal\_length = -1 $$

Don't forget that we leave our $$\vec u$$ right vector un-normalized, despite apparent convention, to account for the aspect ratio down the line.
You can just think of it as a normalized basis vector which has been scaled up by the aspect ratio.


## Camera Rays

With all of these components we are ready to define the rays (in world space coordinates) for each pixel.
For each ray we need an origin point and a direction.
The origin point is easy - all rays originate from the camera location.

$$ P_0 = camera.location $$

For direction, we should normalize the difference between the pixel's position on the image plane (transformed to world space) and the camera origin.

$$ \vec d = P_w - C_0 $$

$$ \vec d = ( C_0 +  U_s * \vec u + V_s * \hat v + W_s * \hat w ) - C_0  $$

$$ \vec d = U_s * \vec u + V_s * \hat v + W_s * \hat w + C_0 - C_0  $$

$$ \vec d = U_s * \vec u + V_s * \hat v + W_s * \hat w $$

$$ \hat d = \frac{\vec d}{|| \vec d ||} $$

Plug these in to get the final ray equation.

$$p(t) = p_0 + t * \hat d$$
