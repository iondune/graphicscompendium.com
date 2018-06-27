---
layout: page
active: lectures
title: "Lecture 8: Coordinate Transformations"
auto-title: true
p5: true
---

## Basics

Multiplying a matrix by a vector allows us to produce a new, changed vectors.

$$ A * v = \begin{bmatrix}a_{1,1} & a_{1,2}\\a_{2,1} & a_{2,2}\end{bmatrix} * \begin{bmatrix}x\\y\end{bmatrix} $$

$$ = \begin{bmatrix}a_{1,1} * x + a_{1,2} * y\\a_{2,1} * x + a_{2,2} * y\end{bmatrix} $$

Such a transformation can be used to **scale**, **shear**, and **rotate** a vector or Cartesian coordinates.


## Scale

<div id="example1">
  <iframe id="exampleFrame1" src="08-example-scale.html" width="420px" height="420px"></iframe>
</div>

### Matrix formulation:

$$ A = \begin{bmatrix}s_x & 0\\s_y & 0\end{bmatrix} $$



## Shear

<div id="example2">
  <iframe id="exampleFrame2" src="08-example-shear.html" width="420px" height="420px"></iframe>
</div>

### Matrix formulation:

$$ A = \begin{bmatrix}1 & tan \theta\\0 & 1\end{bmatrix} $$



## Rotation

<div id="example3">
  <iframe id="exampleFrame3" src="08-example-rotate.html" width="420px" height="420px"></iframe>
</div>

### Matrix formulation:

$$ A = \begin{bmatrix}cos \theta & -sin \theta\\sin \theta & cos \theta\end{bmatrix} $$

### Matrix derivation:

There is a requisite diagram for understanding this which is coming soon...

$$ \vec a = (x_a, y_a) $$

$$ \vec b = (x_b, y_b) $$


$$ \vec b $$ is $$ \vec a $$ rotated by $$ \theta $$

$$ \vec a $$ makes angle $$ \alpha $$ with the x-axis and its length is $$ r = x_a^2 + y_a^2 $$

Therefore:

$$ x_a = r * cos \alpha $$

$$ y_a = r * sin \alpha $$

And:

$$ x_b = r * cos(\alpha + \theta) $$

$$ y_b = r * sin(\alpha + \theta) $$

Using trigonometric relationships:

$$ sin(A + B) = sin(A) * cos(B) + sin(B) * cos(A) $$

$$ cos(A + B) = cos(A) * cos(B) - sin(B) * sin(A) $$

Thus:

$$ x_b = r * cos(\alpha) * cos(\theta) - r * sin(\alpha) * sin(\theta) $$

$$ y_b = r * sin(\alpha) * cos(\theta) + r * cos(\alpha) * sin(\theta) $$

Substituting the equations for $$ x_a $$ and $$ y_a $$:

$$ x_b = x_a * cos(\theta) - y_a * sin(\theta) $$

$$ y_b = y_a * cos(\theta) + x_a * sin(\theta) $$

And the transformation matrix is revealed.

