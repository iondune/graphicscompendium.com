---
layout: page
active: lectures
title: "Lecture 9: Triangles"
auto-title: true
---

For spheres and planes, we plugged the ray equation $$ p(t) = p_o + t * \vec d $$
into the equation of the shape in question to solve for intersection.
But what is the equation for a triangle?
We have the plane equation, and we have line equations for each edge.
We could find the plane intersection point, then check against each line.
In fact, this is how you can support arbitrary polygons,
but there are more convenient representations for triangles.


## Review: Barycentric Coordinates

Barycentric coordinates of point $$ p $$ in terms of $$ a $$, $$ b $$, and $$ c $$
are the numbers $$ \alpha $$, $$ \beta $$, and $$ \gamma $$ such that:

$$ p = \alpha * a + \beta * b + \gamma * c $$

With the constraint:

$$ \alpha + \beta + \gamma = 1 $$

This **must** be true if the three values are barycentric coordinates.

However, the values don't have to be positive.

A point $$ p $$ is inside the the triangle if and only if:

$$ 0 \leq \alpha \leq 1 $$

$$ 0 \leq \beta \leq 1 $$

$$ 0 \leq \gamma \leq 1 $$

What does $$ \alpha = 0 $$ mean? Any point opposite the vertex $$ a $$, or rather along the line from $$ b $$ to $$ c $$.

What about $$ \alpha, \beta = 0 $$? In this case we know $$ \gamma $$ must be $$ 1 $$, this is the $$ c $$ vertex.

The center of the triangle is $$ \alpha = \beta = \gamma = \frac{1}{3} $$

One interesting thing to note is that we're really defining a coordinate system with, e.g.,
origin $$ a $$ and basis vectors $$ b - a $$ and $$ c - a $$

$$ p = a + \beta * (b - a) + \gamma * (c - a) $$

## Intersection

Let's plug in our standard ray equation:

$$ P_0 + t * \vec d = a + \beta * (b - a) + \gamma * (c - a) $$

Since this is a vector form it is really three different equations:

$$ P_0x + t * x_d = x_a + \beta * (x_b - x_a) + \gamma * (x_c - x_a) $$

$$ P_0y + t * y_d = y_a + \beta * (y_b - y_a) + \gamma * (y_c - y_a) $$

$$ P_0z + t * z_d = z_a + \beta * (z_b - z_a) + \gamma * (z_c - z_a) $$

Here we have three equations and three unknowns - $$ t $$, $$ \beta $$, and $$ \gamma $$.
It's a linear system we can solve using matrix algebra.
First let's move all the unknowns to one side:

$$ (x_a - x_b) * \beta + (x_a - x_c) * \gamma + x_d * t = x_a - P_0x $$

$$ (y_a - y_b) * \beta + (y_a - y_c) * \gamma + y_d * t = y_a - P_0y $$

$$ (z_a - z_b) * \beta + (z_a - z_c) * \gamma + z_d * t = z_a - P_0z $$


### Linear System

Let's rewrite in matrix form:

$$
\begin{bmatrix}
x_a - x_b       &   x_a - x_c      &   x_d \\
y_a - y_b       &   y_a - y_c      &   y_d \\
z_a - z_b       &   z_a - z_c      &   z_d \\
\end{bmatrix}

\begin{bmatrix}
\beta \\
\gamma \\
t \\
\end{bmatrix}

=

\begin{bmatrix}
x_a - P_0x \\
y_a - P_0y \\
z_a - P_0z \\
\end{bmatrix}
$$

This is a linear system of the form:

$$ A x = b $$

This 3x3 linear system can be solved by finding the inverse of the matrix.

$$ A x = b $$

$$ A^{-1} A x = A^{-1} b $$

$$ x = A^{-1} b $$

However, we don't need to compute the inverse explicitly - there is a faster option.


### Cramer's Rule

Given a linear system of the form

$$ A x = b $$

with

$$ x =
\begin{bmatrix}
x_1 \\
x_2 \\
x_3 \\
\end{bmatrix}
$$

$$ x_i = \frac{\text{det}(A_i)}{\text{det}(A)} $$

where $$ A_i $$ is the matrix formed by replacing the $$i$$th column of $$ A $$ by the column vector $$ b $$.

The denominator $$ \text{det}(A) $$ gives us an obvious way to test for intersection - if this is 0, there is no intersection.
Cramer's rule is also convenient because it permits solving for individual elements of the solution vector.
So, we can solve for $$ t $$ and check if it's positive.
Them, we can solve for $$ a $$ and check $$ 0 \leq \alpha \leq 1 $$, etc.


### Determinants

Recall, to compute the 2x2 determinant:

$$ \text{det}
\begin{vmatrix}
a & b \\
c & d \\
\end{vmatrix}
= ad - bc
$$

And for a 3x3 determinant:

$$ \text{det}
\begin{vmatrix}
a & b & c\\
d & e & f\\
g & h & i\\
\end{vmatrix}
$$

$$
= a * \text{det}
\begin{vmatrix}
e & f \\
h & i \\
\end{vmatrix}
- b * \text{det}
\begin{vmatrix}
d & f \\
g & i \\
\end{vmatrix}
+ c * \text{det}
\begin{vmatrix}
d & e \\
g & h \\
\end{vmatrix}
$$

$$
= aei - afh - bdi + bfg + cdh - ceg
$$

Note the pattern of (+) (-) (+) for the three 2x2 determinants.
We can compute this "expansion" of the determinant using any row or column, not just the first row (as in this example).
However, if you use the middle row or middle column, you flip the signs to (-) (+) (-)


### Solve

Below are the results of applying Cramer's rule.
Note the vertical bars around a matrix mean to compute the determinant.

$$
\beta = \frac{
    \begin{vmatrix}
    x_a - P_0x      &   x_a - x_c       &   x_d \\
    y_a - P_0y      &   y_a - y_c       &   y_d \\
    z_a - P_0z      &   z_a - z_c       &   z_d \\
    \end{vmatrix}
}{
    \begin{vmatrix}
    A
    \end{vmatrix}
}
$$

$$
\gamma = \frac{
    \begin{vmatrix}
    x_a - x_b       &   x_a - P_0x      &   x_d \\
    y_a - y_b       &   y_a - P_0y      &   y_d \\
    z_a - z_b       &   z_a - P_0z      &   z_d \\
    \end{vmatrix}
}{
    \begin{vmatrix}
    A
    \end{vmatrix}
}
$$

$$
t = \frac{
    \begin{vmatrix}
    x_a - x_b       &   x_a - x_c       &    x_a - P_0x \\
    y_a - y_b       &   y_a - y_c       &    y_a - P_0y \\
    z_a - z_b       &   z_a - z_c       &    z_a - P_0z \\
    \end{vmatrix}
}{
    \begin{vmatrix}
    A
    \end{vmatrix}
}
$$


## Implementation

### Common Subterms

3x3 matrices have common subterms that can be exploited by carefully naming variables.
Lets look at generic form with dummy variables.

$$
\begin{bmatrix}
a & d & g\\
b & e & h\\
c & f & i\\
\end{bmatrix}

\begin{bmatrix}
\beta \\
\gamma \\
t \\
\end{bmatrix}

=

\begin{bmatrix}
j \\
k \\
l \\
\end{bmatrix}
$$

Solving for determinants expands to (here we expand the determinant on the third column purposefully):

$$
\beta = \frac{
    \begin{vmatrix}
    j & d & g \\
    k & e & h \\
    l & f & i \\
    \end{vmatrix}
}{
    \begin{vmatrix}
    a & d & g \\
    b & e & h \\
    c & f & i \\
    \end{vmatrix}
}
$$

$$
\beta = \frac
{j (ei - hf) + k (gf - di) + l (dh - eg)}
{a (ei - hf) + b (gf - di) + c (dh - eg)}
$$

The expression $$ (ei - hf) $$ is found in two places - instead of computing this twice,
you can create a variable for it, compute once, and use it twice.
Of course, also save the denominator value - it is the same for all three variables.

Similarly, we can purposefully expand both $$ \gamma $$ and $$ t $$ computations to share expressions.

$$
\gamma = \frac{
    \begin{vmatrix}
    a & j & g \\
    b & k & h \\
    c & l & i \\
    \end{vmatrix}
}{
\text{det}(A)
}
$$

$$
\gamma = \frac
{i (ak - jb) + h (jc - al) + g (bl - kc)}
{\text{det}(A)}
$$

$$
t = \frac{
    \begin{vmatrix}
    a & d & j \\
    b & e & k \\
    c & f & l \\
    \end{vmatrix}
}{
\text{det}(A)
}
$$

$$
t = \frac
{- d (bl - kc) + e (al - jc) - f (ak - jb)}
{\text{det}(A)}
$$

Which we can then manipulate to create common terms with the $$ \gamma $$ solution.

$$
t = \frac
{-f (ak - jb) - e (jc - al) - d (bl - kc)}
{\text{det}(A)}
$$

Arguably, this also makes cleaner and easier to read/verify code.

### Pseudocode

```pascal
hit_tri(ray r, vert a, vert b, vert c, t0, t1)

    compute t
    if (t < t0) || (t > t1) then
        return false

    compute gamma
    if (gamma < 0) || (gamma > 1) then
        return false

    compute beta
    if (beta < 0) || (beta > 1 - gamma) then
        return false

    return true
```
