---
layout: page
active: lectures
title: "Lecture 13: Normal Matrix"
auto-title: true
---


## Transforms Example

Consider a simple transform matrix $$ M $$

$$
\begin{bmatrix}
-1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1 \\
\end{bmatrix}
$$

If we take the cross product between two vectors, we get one result:

$$ (1, 0, 0) \times (0, 1, 0) = (0, 0, 1) $$

If we transform each by M, then take the cross product, we get something different (as expected):

$$ M(1, 0, 0) \times M(0, 1, 0) = $$

$$ (-1, 0, 0) \times (0, 1, 0) = (0, 0, -1) $$

What might not be expected is that if we apply the $$ M $$ transform to the result of the first cross product,
we get a different result:

$$ M((1, 0, 0) \times (0, 1, 0)) = M(0, 0, 1) = (0, 0, 1) $$

This shows us that the result of a cross product transforms differently from a cross product.
That's because that result is not a vector - it's a bivector.

## Derivation

Let's take a closer look.
What do we get if we transform some vector $$ a $$ by matrix $$ M $$?

$$ M \vec a = M (a_1 \hat x + a_2 \hat y + a_3 \hat z) = a_1 M_1 + a_2 M_2 + a_3 M_3 $$

Here $$ M_i $$ is the $$i$$th column of $$ M $$.

So what is the cross product of two vectors $$ a $$ and $$ b $$ after they are transformed?

$$ Ma \times Mb = $$

$$ (a_1 M_1 + a_2 M_2 + a_3 M_3) \times (b_1 M_1 + b_2 M_2 + b_3 M_3) = $$

The cross product is distributive over addition, so we can write:

$$ a_1 M_1 \times b_1 M_1 + a_1 M_1 \times b_2 M_2 + a_1 M_1 \times b_3 M_3 + $$

$$ a_2 M_2 \times b_1 M_1 + a_2 M_2 \times b_2 M_2 + a_2 M_2 \times b_3 M_3 + $$

$$ a_3 M_3 \times b_1 M_1 + a_3 M_3 \times b_2 M_2 + a_3 M_3 \times b_3 M_3 = $$

Any vector crossed with itself is 0:

$$ a_1 M_1 \times b_2 M_2 + a_1 M_1 \times b_3 M_3 + $$

$$ a_2 M_2 \times b_1 M_1 + a_2 M_2 \times b_3 M_3 + $$

$$ a_3 M_3 \times b_1 M_1 + a_3 M_3 \times b_2 M_2 = $$

And cross products are anti-commutative ($$ a \times b = - b \times a $$), so:

$$ a_1 M_1 \times b_2 M_2 + -  b_3 M_3 \times a_1 M_1 + $$

$$ -b_1 M_1 \times a_2 M_2 + a_2 M_2 \times b_3 M_3 + $$

$$ a_3 M_3 \times b_1 M_1 + - b_2 M_2 \times a_3 M_3 = $$

$$ (a_2 b_3 - a_3 b_2) (M_2 \times M_3) + $$

$$ (a_3 b_1 - a_1 b_3) (M_3 \times M_1) + $$

$$ (a_1 b_2 - a_2 b_1) (M_1 \times M_2) $$


Okay, there's the formula for the cross product components on the left, but what about those cross products on the right?

Well we can write this in vector form like so:


$$
\begin{bmatrix}
(M_2 \times M_3)_x * (a_2 b_3 - a_3 b_2) + (M_3 \times M_1)_x * (a_3 b_1 - a_1 b_3) + (M_2 \times M_3)_x * (a_1 b_2 - a_2 b_1) \\
(M_2 \times M_3)_y * (a_2 b_3 - a_3 b_2) + (M_3 \times M_1)_y * (a_3 b_1 - a_1 b_3) + (M_2 \times M_3)_y * (a_1 b_2 - a_2 b_1) \\
(M_2 \times M_3)_z * (a_2 b_3 - a_3 b_2) + (M_3 \times M_1)_z * (a_3 b_1 - a_1 b_3) + (M_2 \times M_3)_z * (a_1 b_2 - a_2 b_1) \\
\end{bmatrix}
$$

Which shows us that we can rewrite this as matrix-vector multiplcation.

$$
\begin{bmatrix}
M_2 \times M_3 \\
M_3 \times M_1 \\
M_1 \times M_2 \\
\end{bmatrix}^T
\begin{bmatrix}
a_2 b_3 - a_3 b_2 \\
a_3 b_1 - a_1 b_3 \\
a_1 b_2 - a_2 b_1 \\
\end{bmatrix}
$$

On the right is the result of the cross product between $$a$$ and $$b$$.
On the left is a matrix we can multiply the result by to get what we have gotten by transforming both vectors before crossing.

So what is this matrix on the left?

$$
\begin{bmatrix}
M_2 \times M_3 \\
M_3 \times M_1 \\
M_1 \times M_2 \\
\end{bmatrix}^T
$$

Well, first let's just point something out.

$$ (M_2 \times M_3) \cdot M_1 = \text{det}M $$

$$ (M_3 \times M_1) \cdot M_2 = \text{det}M $$

$$ (M_1 \times M_2) \cdot M_3 = \text{det}M $$

This follows from the formula for determinant - if you multiply everything out, it's the same quantity.

However, all other dot products are $$ 0 $$, e.g.:

$$ (M_2 \times M_3) \cdot M_2 = 0 $$

$$ (M_2 \times M_3) \cdot M_3 = 0 $$

This also follows from the multiplication:

$$ (M_2 \times M_3) \cdot M_2 = $$

$$
\begin{bmatrix}
M_2y M_3z - M_3y M_2z \\
M_2z M_3x - M_3z M_2x \\
M_2x M_3y - M_3x M_2y \\
\end{bmatrix}
\cdot
\begin{bmatrix}
M_2x \\
M_2y \\
M_2z \\
\end{bmatrix} =
$$

$$
M_2y M_3z M_2x - M_3y M_2z M_2x +
M_2z M_3x M_2y - M_3z M_2x M_2y +
M_2x M_3y M_2z - M_3x M_2y M_2z = 0
$$

This is interesting because it gives us:

$$
\begin{bmatrix}
M_2 \times M_3 \\
M_3 \times M_1 \\
M_1 \times M_2 \\
\end{bmatrix}
M
=
\begin{bmatrix}
\text{det}M & 0 & 0 \\
0 & \text{det}M & 0 \\
0 & 0 & \text{det}M \\
\end{bmatrix}
$$

$$
\frac{1}{\text{det}M}
\begin{bmatrix}
M_2 \times M_3 \\
M_3 \times M_1 \\
M_1 \times M_2 \\
\end{bmatrix}
M
=
\begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1 \\
\end{bmatrix}
$$

Which, by the definition of matrix inverse, means that:

$$
\frac{1}{\text{det}M}
\begin{bmatrix}
M_2 \times M_3 \\
M_3 \times M_1 \\
M_1 \times M_2 \\
\end{bmatrix}
=
M^{-1}
$$

$$
\begin{bmatrix}
M_2 \times M_3 \\
M_3 \times M_1 \\
M_1 \times M_2 \\
\end{bmatrix}
=
(\text{det}M) M^{-1}
$$

$$
\begin{bmatrix}
M_2 \times M_3 \\
M_3 \times M_1 \\
M_1 \times M_2 \\
\end{bmatrix}^T
=
(\text{det}M) (M^{-1})^T
$$

There we go - inverse transpose is how to transform the result of a cross product.
We need to divide by the determinant as well, but since we always normalize,
this only really matters for flipping normals.

$$
(\text{det}M) (M^{-1})^T
\begin{bmatrix}
a_2 b_3 - a_3 b_2 \\
a_3 b_1 - a_1 b_3 \\
a_1 b_2 - a_2 b_1 \\
\end{bmatrix}
$$

Next, we will cover why exactly it is that the result of a cross product is this different entity that obeys different rules.


