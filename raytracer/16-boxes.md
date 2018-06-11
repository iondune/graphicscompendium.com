---
layout: page
active: lectures
title: "Lecture 16: Boxes"
auto-title: true
---


## Overview

We can represent a bounding box as a bounded region between $$ x_{min} $$ and $$ x_{max} $$ on the **x** axis, and between $$ y_{min} $$ and $$ y_{max} $$ on the **y** axis.

{% include scaled-image.html name="BoxIntersection1" %}

For a 3D box we simply need to add a similar constraint $$ z_{min} $$ and $$ z_{max} $$.
We'll live in the 2D world for now.

For a given ray we can compute the intersections with all four of our parallel lines.
We'll call these intersections $$ t_{x min} $$, $$ t_{x max} $$, etc.
These are the **t** values for the ray, intersecting with each of the four respective lines.

{% include scaled-image.html name="BoxIntersection2" %}

If the largest of the **t ? min** values is greater than the smallest of the **t ? max** values, then the ray does not intersect the box.

For example, in the above **ray 1** we can see that $$ t_{x min} > t_{y max} $$, therefore there is no intersection.

{% include scaled-image.html name="BoxIntersection3" %}

For **ray 2** we can see that the largest min value, $$ t_{x min} $$ is less than the smallest max value, $$ t_{y max} $$, therefore there is intersection.

{% include scaled-image.html name="BoxIntersection4" %}

For **ray 3** we can see that $$ t_{y min} > t_{x max} $$, therefore there is no intersection.



## Intersections

So how do we find the intersection **t** values?

Recall our equation of a ray:

$$ P(t) = P_0 + t * \vec d $$

We can solve this for each dimension individually, thus we want to solve for t such that:

$$ x_{min} = P_{0x} + t * d_x $$

or

$$ t = \frac{x_{min} - P_{0x}}{d_x} $$

Thus we can compute the four **t** values:

$$ t_{x min} = \frac{x_{min} - P_{0x}}{d_x} $$

$$ t_{x max} = \frac{x_{max} - P_{0x}}{d_x} $$

$$ t_{y min} = \frac{y_{min} - P_{0y}}{d_y} $$

$$ t_{y max} = \frac{y_{max} - P_{0y}}{d_y} $$

And we can apply the test using the following pseudo-code:

```perl
if max(t_x_min, t_y_min) > min(t_x_max, t_y_max) then
    return false # no hit
else
    return true
```

Note that if the ray is traveling the opposite direction we need to swap the mins and maxes.
In general we are concerned with the order in which the rays reach the two lines.

Thus we can generalize our approach (which also makes it easier to add a 3rd dimension later)

```perl

tgmin = -infinity
tgmax = infinity

# for x

t1 = (x_min - P_0_x) / dx
t2 = (x_max - P_0_x) / dx

if t1 > t2 then
    swap(t1, t2)
    # Because we want the closest t1 from the perspective of the ray

if t1 > tgmin then tgmin = t1 # not a typo - remember we are finding the largest of the mins
if t2 < tgmax then tgmax = t2 # smallest of the maxes

# same for y, z

if tgmin > tgmax then return false
if tgmax < 0 then return false # box behind

# If we're still here, it's an intersect, and tgmin is the intersction point
if (tgmin > 0)
    return (true, tgmin)
else
    return (true, tgmax) # Though we could be inside the box, in which case tgmax is the "exit" intersection
```

If the ray is parallel to any of the faces of our box, e.g. if `dx == 0`, we'll get a divide-by-zero when we try to do this.
However, if that's the case we just need to check the start point of the ray.


```perl

if dx == 0 then
    if P_0_x < x_min or P_0_x > x_max then
        return false
else
    # Do the check we did before

```



## Pov-Ray

In our `.pov` files, we are given the min and max of a box:

```
box {<-2, -3, -4>, <4, 3, 2>
  pigment { color rgb <1.0, 0.2, 1.0>}
  finish {ambient 0.2 diffuse 0.8}
}
```

This describes a box with min `<-2, -3, -4>` and max `<4, 3, 2>`.



## Normal Calculation

To calculate the normal of a box at a given intersection point, we simply need to figure out which side of the box we hit.
The normal is then implicitly understood.

```perl
if pt.x == min.x then
    return vec3(-1, 0, 0)
else if pt.x == max.x then
    return vec3(1, 0, 0)
else if pt.y == min.y then
    return vec3(0, -1, 0)

# etc...
```

Note however that it is insufficient to simply use the `==` operator for float comparisons of this nature.
Due to the inherent imprecision of floating point values we need to check if `pt.x` is simply near to `min.x`.

If you're using `glm`, there is a built in `glm::epsilonEqual` function that you can use in `<glm/gtc/epsilon.hpp>`.

It's relatively simple to write our own espilon comparison method, though:

```cpp
bool Equals(float const a, float const b, float const epsilon = 0.0001f)
{
    return std::abs(a - b) < epislon;
}
```

Note that there is no one-size-fits-all approach to comparing float values.
The correct technique is going to depend on the relative size and scale of values.
For the units in our program, something like the above will work just fine.

If you're interested in a slightly more in-depth look at floating point comparisons,
[this blog post](http://realtimecollisiondetection.net/blog/?p=89) is a good place to start.


## Interactive Example

<div id="example1">
  <iframe id="exampleFrame1" src="examples/box-ray-intersect.html" width="100%" height="840px"></iframe>
</div>

Example Code:

```js
function FindIntersectTime(d, p0) {
  tymin = (bbox_min.y - p0.y) / d.y;
  tymax = (bbox_max.y - p0.y) / d.y;
  txmin = (bbox_min.x - p0.x) / d.x;
  txmax = (bbox_max.x - p0.x) / d.x;

  if (tymin > tymax) {
    var temp = tymin;
    tymin = tymax;
    tymax = temp;
  }
  if (txmin > txmax) {
    var temp = txmin;
    txmin = txmax;
    txmax = temp;
  }

  smallestMax = min(txmax, tymax); // Smallest of maxs
  largestMin = max(txmin, tymin); // Largest of mins

  // If max of mins is < min of maxes, then miss
  if (smallestMax < largestMin || smallestMax < 0) {
    return -1;
  }

  if (largestMin > 0) {
    return largestMin;
  }
  else {
    return smallestMax;
  }
}
```
