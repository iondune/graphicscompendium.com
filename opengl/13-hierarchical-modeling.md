---
layout: page
active: lectures
title: "Lecture 13: Hierarchical Modeling"
auto-title: true
p5: true
---


## Transformation Order

The first thing to understand is that the order of transformations makes a big difference.

A good example of this comes from applying a rotation and translation to an object.

Our rotation transformations rotate an object around the origin.
But sometimes, we may want to rotate an object around a different point.

For a given vertex $$ v $$, rotation matrix $$ R $$ and translation matrix $$ T $$.

$$ v\prime = T * R * v $$

This describes first a rotation about the origin, then we translate to a position described by $$ T $$.
So the object will be rotated, but it will be at the position specified by $$ T $$.

$$ v\prime = R * T * v $$

This describes a translation followed by a rotation around the *new* origin.
Now it's like the object is off in space and orbiting the origin.

See the visual example below!

<div id="example1">
  <iframe id="exampleFrame1" src="13-example-xform-order.html" width="820px" height="420px"></iframe>
</div>



## Hierarchical Models

Let's say we want to build a robot arm like below:

<div id="example2">
  <iframe id="exampleFrame2" src="13-example-arm.html" width="820px" height="420px"></iframe>
</div>

We have a red upper arm, green lower arm, and blue hand.

For the red upper arm, the transform is simple.
We defined a **Rotation 1** and rotate by this:

$$ red = R_1 * v $$

For the green lower arm, we want the block to follow the rotation of the upper arm, but also be able to rotate around the elbow.
We define a **Rotation 2** which is the elbow rotation.
We also need to account for the fact that the green block is moved along the x-axis
(it doesn't occupy the same space as the red block, it's at the end of the red block).

So we want to first rotate around the elbow, then move out to the end of the upper arm.
Finally we need to rotate along with the upper arm rotation.

$$ green = R_1 * T_{green} * R_2 * v $$

We'll do a similar process for the blue hand.
We add a **Rotation 3** which is the rotation of the wrist.
We also need to translate out to the end of the green lower arm.

Note that how we chain these transformations together is necessary to make all the rotations work.
We also don't have to account for the length of the red upper arm when translating the blue hand.
That is already accounted for in our chain.

$$ blue = R_1 * T_{green} * R_2 * T_{blue} * R_3 * v $$



## Hierarchic Transforms Slides

[For the rest of the lecture we will follow these slides.](https://docs.google.com/presentation/d/1-UWKLmk1xsU49JKv2JGv8UsIVJCHnpaLhkLa-ejYIU0/edit?usp=sharing)



## Rectangle Transform: "A" Example

<div id="example3">
  <iframe id="exampleFrame3" src="13-example-a.html" width="420px" height="420px"></iframe>
</div>


### Source Code

```javascript
rotate(-rot);


push();
  translate(-75, 0);
  rotate(radians(60));
  scale(3, 0.25);

  // red rectangle
  fill(204, 101, 101, 127);
  stroke(127, 63, 63);
  rect(-50, -50, 100, 100);
pop();

push();
  translate(75, 0);
  rotate(radians(-60));
  scale(3, 0.25);

  // green rectangle
  fill(101, 204, 101, 127);
  stroke(63, 127, 63);
  rect(-50, -50, 100, 100);
pop();

push();
  translate(0, -20);
  scale(1.25, 0.25);

  // blue rectangle
  fill(101, 101, 204, 127);
  stroke(63, 63, 127);
  rect(-50, -50, 100, 100);
pop();
```


## Transformation Order

Note that the standard order to apply the usual three transforms (translation, rotation, and scale) to an object is:


$$ T * R * S * v $$

You definitely want to apply scale before translation - otherwise, the scale will increase or decrease the translation amount.

If the scale is uniform (same value for x, y, and z) then it and rotation can be in either order.
But, applying a non-uniform scale after rotation will cause a weird effect (it will turn rectangles into trapezoids).
