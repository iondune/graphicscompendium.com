---
layout: page
active: lectures
title: "Lecture 5: Vectors"
auto-title: true
---

## Basics

A 2D point:

$$ p = \begin{bmatrix}x\\y\end{bmatrix} $$

represents a location w/ respect to some coordinate system.

A 2D vector:

$$ \vec v = \begin{bmatrix}x\\y\end{bmatrix} $$

represents a **displacement** from a position.

Vectors have **length** and direction - but no inherent location.
Can represent a displacement or motion.

Consider that one step north while standing in NYC is the same as one step north while standing in SLO - in terms of displacement/motion.


### Notation

We write vectors as lists of components: $$ { x_0, x_1 } $$

We write vectors as a column matrix:

$$ \vec v = \begin{bmatrix}x_0\\x_1\end{bmatrix} $$

$$ \vec w = \begin{bmatrix}x_0\\x_1\\x_2\end{bmatrix} $$



## Operations

### Scalar

Multiplication by a scalar:

$$ \vec a = \begin{bmatrix}2\\5\end{bmatrix} $$

$$ \vec b = \begin{bmatrix}3\\-2\end{bmatrix} $$

$$ 4 * \vec a = \begin{bmatrix}8\\20\end{bmatrix} $$

Multiplying a vector by a scalar changes its length, but not its direction.

### Addition

$$ \vec a + \vec b = \begin{bmatrix}2 + 3\\5 - 2\end{bmatrix} = \begin{bmatrix}5\\3\end{bmatrix} $$

(this is component-wise)

Vector additional can be visualized by placing the vectors head-to-tail.

### Subtraction

Geometric of $$ - \vec v $$ is a vector in the opposite direction.

So, subtraction is not suprising:

$$ \vec a - \vec b = \begin{bmatrix}a_0 - b_0\\a_1 - b_1\end{bmatrix} $$



## Basis Vectors


### Linear Combination

A **linear combination** of $$ m $$ vectors $$ v_1, v_2, ..., v_m $$ is a *vector* of the form:

$$ \vec w = a_1 * \vec v_1 + a_2 * \vec v_2 + ... + a_m * \vec v_m $$

where $$ a_1, a_2, ..., a_m $$ are scalars.

Barycentric coordinates are an example of a linear combination (of $$ 3 $$ vectors).


### Linearly Independant

If two vectors are non-zero and not parallel, they are called **linearly independant**.

For two given linearly independant vectors $$ \vec v_1 $$ and $$ \vec v_2 $$, we can write any vector $$ \vec v_3 $$ in terms of the other two.

$$ \vec v_3 = 2 * \vec v_2 + \frac{1}{2} * \vec v_1 $$

The two vectors $$ \vec v_1 $$ and $$ \vec v_2 $$ are then called **basis vectors**.

*Any* vector can be expressed as a linear combination of the basis vectors:

$$ \vec c = a_c * \vec a + b_c * \vec b $$

Note that the cooeficients $$ a_c $$ and $$ b_c $$ are unique -
that is, a different $$ a_c $$ and $$ b_c $$ will give a different vector $$ c $$.



## Cartesian Coordinates

If two vectors are at right angles to each other (e.g. they are **orthogonal**), they form an **orthogonal basis**.

If the vectors are also both of length $$ 1 $$, we call this an **orthonormal basis**.

In 2D, we can use 2 such special vectors $$ \vec x $$ and $$ \vec y $$ to define the **Cartesian basis vectors**:

$$ \vec x = \begin{bmatrix}1\\0\end{bmatrix} $$

$$ \vec y = \begin{bmatrix}0\\1\end{bmatrix} $$

Not surprisingly, the Cartesian basis vectors in 3D are:

$$ \vec x = \begin{bmatrix}1\\0\\0\end{bmatrix} $$

$$ \vec y = \begin{bmatrix}0\\1\\0\end{bmatrix} $$

$$ \vec z = \begin{bmatrix}0\\0\\1\end{bmatrix} $$

(Note that for a 3D coordinate system, we need three basis vectors.)


### Handedness

Depending on where the $$ z $$ axis points, we call a basis **left** or **right** handed.
This will come up again when we talk about other operations on vectors.

The handedness refers to how the third basis is related to the first two.
If the first basis vector is along your thumb and the second basis vector is along your index finger, your palm is pointing in the direction of the third basis vector.
Note that this is a different direction for your left or right hand.

In OpenGL, we use a right-handed basis.



## Coordinate Frame

A **Coordinate Frame** (or coordinate system) consists of an origin point $$ o $$ $$ {0, 0, 0} $$ and a basis of linearly independant vectors $$ \vec a, \vec b, \vec c $$.

The standard coordinate system in 3D is:

$$ \vec x = \begin{bmatrix}1\\0\\0\end{bmatrix} $$

$$ \vec y = \begin{bmatrix}0\\1\\0\end{bmatrix} $$

$$ \vec z = \begin{bmatrix}0\\0\\1\end{bmatrix} $$

$$ o = {0, 0, 0} $$

Any 3D point $$ {q, r, s} $$ can be written as:

$$ \begin{bmatrix}q\\r\\s\end{bmatrix} = o + q * \vec x + r * \vec y + s * \vec z $$

