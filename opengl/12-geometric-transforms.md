---
layout: page
active: lectures
title: "Lecture 12: Geometric Transforms"
auto-title: true
---

## Composite Transformations

Often we want to apply more than one transform.
If we first wanted to scale an object, we could write:

$$ v_2 = S v_1 $$

Where $$ v_1 $$ is the vector to be scaled, $$ S $$ is the scale matrix, and $$ v_2 $$ is the scaled vector.

Then, if we wanted to rotate the object, we would write:

$$ v_3 = R v_2 $$

Where $$ v_3 $$ is the final vector and $$ R $$ is the rotation matrix.

In full, we could write:

$$ v_3 = R (S v_1) $$

However, we can also compute the composite matrix $$ R * S $$, then apply this to $$ v_1 $$ and get the same result.

$$ v_3 = (R * S) v_1 $$

This concept of a **composite** matrix is essential in graphics applications as it means
we can represent the work of two matrices in a single composite matrix.

Thus we can send a single matrix as a uniform for each draw call which can apply
multiple transformations to an object.

Note that these transforms are applied right side first!

Thus $$ R S v_1 $$ is different from $$ S R v_2 $$



## Translation

So far we have covered rotation, scale, and shear transformations.
There is one type of transformation conspiciously missing from this list: **translation** (or movement).

This is because we have been looking at matrices $$ M $$ that transform a point in the following form:

$$ x\prime = m_{1,1} * x + m_{1,2} * y $$

$$ y\prime = m_{2,1} * x + m_{2,2} * y $$

But this kind of transform cannot allow us to *move* locations.
As an example, you can't multiply the point $$ {0, 0} $$ with any number to move it.
To translate something, we need a transform of the form:

$$ x\prime = x + x_t $$

$$ y\prime = y + y_t $$

There is no way to do this with a 2x2 matrix!

What's the solution?
We could keep track of a separate translation...
but the goal here is to represent everything with a matrix.
Then we can easily composite transformations.

The solution is to move to a higher dimension.

Recall that a 2D shear looked like:

$$ A = \begin{bmatrix}1 & shear\\0 & 1\end{bmatrix} $$

A 3D shear looks like:

$$ A = \begin{bmatrix}1 & 0 & x_s\\0 & 1 & y_s\\0 & 0 & 1\end{bmatrix} $$

Applied to some 3D vector:

$$ \begin{bmatrix}1 & 0 & x_s\\0 & 1 & y_s\\0 & 0 & 1\end{bmatrix} \begin{bmatrix}x\\y\\z\end{bmatrix} = \begin{bmatrix}x+x_s*z\\y+y_s*z\\z\end{bmatrix} $$

So we set the third or $$ z $$ component of our vector to $$ 1 $$:

$$ \begin{bmatrix}1 & 0 & x_t\\0 & 1 & y_t\\0 & 0 & 1\end{bmatrix} \begin{bmatrix}x\\y\\1\end{bmatrix} = \begin{bmatrix}x+x_t\\y+y_t\\1\end{bmatrix} $$

Thus by using a 3D transformation matrix and a 3D vector with $$ 1 $$ in the $$ z $$ component, we can use a matrix to represent a 2D translation.

This video shows visually how a shear in 3D space works out to be a translate in 2D space:

![Affine transformations](https://upload.wikimedia.org/wikipedia/commons/c/c4/Affine_transformations.gif)

<div class="well well-sm">
  By LucasVB (Own work) [Public domain], <a href="https://commons.wikimedia.org/wiki/File%3AAffine_transformations.gif">via Wikimedia Commons</a>
</div>



## Composite with Translation

For example, we can translate a coordinate by $$ {t_x, t_y} $$ and then rotate by an angle of $$ \theta $$ by using the following composite matrices:

$$ \begin{bmatrix}cos\theta & -sin\theta & 0\\sin\theta & cos\theta & 0\\0 & 0 & 1\end{bmatrix}
   \begin{bmatrix}1 & 0 & x_t\\0 & 1 & y_t\\0 & 0 & 1\end{bmatrix}
   \begin{bmatrix}x\\y\\1\end{bmatrix} $$

Notice that we've added a column of $$ 0, 0, 1 $$ and a row of $$ 0, 0, 1 $$ to the rotation matrix from before.
This holds true for other 2D transforms.
For example, scale:

$$ \begin{bmatrix}s_x & 0 & 0\\0 & s_y & 0\\0 & 0 & 1\end{bmatrix} $$

and shear-x:

$$ \begin{bmatrix}1 & shear_x & 0\\0 & 1 & 0\\0 & 0 & 1\end{bmatrix} $$


### Positions vs. Vectors

This also gives us a tool for distinguishing between positions and vectors.

We want positions to me affected by translation matrices, of course.
But if we have something which is geometrically a vector (e.g. a surface normal), we want that to be scaled and rotated by not translated.
We can accomplish this by putting a $$ 0 $$ in the $$ z $$ component instead of a $$ 1 $$.

Thus, for vectors:

$$ \begin{bmatrix}1 & 0 & x_t\\0 & 1 & y_t\\0 & 0 & 1\end{bmatrix} \begin{bmatrix}x\\y\\0\end{bmatrix} = \begin{bmatrix}x\\y\\0\end{bmatrix} $$

This extra component is called the **homogeneous** coordinate.
This is because it is typically either $$ 0 $$ or $$ 1 $$.


## What about 3D?

Since we used a third component to represent translations in our 2D coordinate system,
it should come as no surprise that we used a fourth component to represent translations in a 3D coordinate system:

3D scale:

$$ \begin{bmatrix}s_x & 0 & 0 & 0\\0 & s_y & 0 & 0\\0 & 0 & s_z & 0\\0 & 0 & 0 & 1\end{bmatrix} $$

3D rotate (around Z axis):

$$ \begin{bmatrix}cos\theta & -sin\theta & 0 & 0\\sin\theta & cos\theta & 0 & 0\\0 & 0 & 1 & 0\\0 & 0 & 0 & 1\end{bmatrix} $$

