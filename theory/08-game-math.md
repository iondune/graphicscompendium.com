---
layout: page
active: lectures
title: "Lecture 8: Game Math"
auto-title: true
---

## Scalars and Vectors

Numerical quantities used in mathematics and physics fall into two broad categories:
**scalars** and **vectors**.

Scalars are quantities that can be described using a single numerical value
which represents its size/magnitude:

- Distance
- Mass
- Time

Vectors are quantities that carry enough information to represent a direction in space,
in addition to a magnitude:
- Difference between two points (distance between, and what direction you need to go)
- Velocity of a projectile (speed and the direction it's going)
- A force acting on an object (strength of the force, and the direction applied)


### Vectors

Vectors are often visualized as an arrow.
However, a vector does not have a specific "start point" - it's just a direction and magnitude.
So two vectors visualized in different places are the same vector.

Vectors are n-dimensional, described by n coordinates or components.

$$ v = (1, 2, 3) $$

$$ u = (4, 5) $$

- 2D: x, y
- 3D: x, y, z
- 4D: x, y, z, w

In mathematics we commonly use the following notation for vectors:

$$ \vec v = \begin{bmatrix}x\\y\end{bmatrix} $$

But what can we do with vectors?

Sometimes we use $$x$$, $$y$$, $$z$$ to describe the individual components of a vector.

Sometimes we'll use a subscript:

$$ \vec v = \begin{bmatrix}v_1\\v_2\end{bmatrix} $$

Subscripts will sometimes index from 1 (generally in pure mathematics),
though some authors index from 0 to match with common programming conventions.


## Vector Operations

### Magnitude

We can compute the **magnitude**, or length, of a vector:

$$ \lVert v\rVert = \sqrt{x^2 + y^2} $$

This equation applies similarly in larger dimensions:

$$ \begin{align}
\lVert w\rVert & = \sqrt{x^2 + y^2 + z^2}\\
\lVert q\rVert & = \sqrt{x^2 + y^2 + z^2 + w^2}
\end{align} $$



### Scalar Multiplication

Multiplying a vector by a scalar changes its length, but not its direction.

$$ \vec a = \begin{bmatrix}2\\5\end{bmatrix} $$

$$ \vec b = \begin{bmatrix}3\\-2\end{bmatrix} $$

$$ 4 * \vec a = \begin{bmatrix}8\\20\end{bmatrix} $$


### Addition

Vector additional can be visualized by placing the vectors head-to-tail.
The result of vector addition is what you would get if you first travelled along one vector, then the other.

$$ \vec a + \vec b = \begin{bmatrix}2 + 3\\5 - 2\end{bmatrix} = \begin{bmatrix}5\\3\end{bmatrix} $$

This is a component-wise operation - each component in the result vector is the sum of the related component in each operand vector.


### Subtraction

The geometric interpretation of $$ - \vec v $$ is a vector in the opposite direction.

So, subtraction is not suprising:

$$ \vec a - \vec b = \vec a + (-\vec b) = \begin{bmatrix}a_0 - b_0\\a_1 - b_1\end{bmatrix} $$


### Dot Product

There are three ways to multiply vectors together.

There is the obvious way, component-wise multiplication, that looks a lot like vector addition.
However, this type of multipication doesn't have much geometric meaning, and it is not common in mathematics.
It is commonly used in games when working with colors, but we're going to talk about that later.

The other two types of multiplcation are the **dot product** and the **cross product**.
The cross product is a little complex and only works in 3D, so we're just going to talk about the dot product today.

The dot product is interesting because unlike our other operations, it does not product a vector -
it produces a scalar.

$$ \text{vector} \cdot \text{vector} = \text{scalar} $$

Specifically, for two 2D vectors it produces:

$$ \vec a \cdot \vec b = a_1 * b_1 + a_2 * b_2 $$

Similarly, in 3D:

$$ \vec a \cdot \vec b = a_1 * b_1 + a_2 * b_2 + a_3 * b_3 $$

The geometric meaning of this product is that it describes
*the degree to which the two vectors are facing the same direction*.

It's easiest to reason about if we take the dimensions of the vectors out of the question -
let's say we have two vectors of **unit length** (that is, their magnitude is 1).

If the dot product of these vectors is 1, they are the same vector (pointing the exact same direction).
If it's, say, 0.5, these vectors are generally pointing in the same direction but not quite.

If the dot product is 0, the vectors are orthogonal - they point at right angles to each other.
If the dot product is -1, the vectors point in exact opposite directions.

This operation is very useful in the context of games.
For example, you can use the dot product to easily check whether one character is looking at another.



## Coordinate Systems

Sometimes we want to talk about a specific place in our spaces.
We might call this a point, a coordinate, or a position.

If we pick some point in space called the **origin** that represents $$(0, 0, 0)$$
in our coordinate system (or however many 0s are necessary to match the number of dimensions),
then we can think of the point

$$p = {3, 4}$$

as being $$ 3 $$ units away from the origin on the horizontal axis and $$ 4 $$ units away from the origin on the vertical axis.

We could also use the vector

$$ \vec p = \begin{bmatrix}3\\4\end{bmatrix} $$

to represent the offset from the origin that we travel along to reach $$ p $$.

As such, even though mathematically and physically vectors and points are two distinctly different things,
in common practice we'll commonly use vectors to represent points as well -
especially because a lot of constructs related to vectors work well with points as well.

But back to coordinate systems - we said that $$p$$ is 3 units away from the horizontal axis.
What is this axis?
Commonly we'll call it the $$\hat x$$ axis (and the vertical axis is the $$\hat y$$),
and the choice of these axis is another fundamental piece of the coordinate system.

If we have an **origin** point along with an axis (sometimes called **basis vector**) for each dimension,
those pieces together form a coordinate system (also sometimes called a coordinate frame).



## Transforms

There's a mathematical bridge between what we just talked about and what we're going to talk about now,
but it involves some linear algebra so we're going to ignore it in this class (in the interest of time).

With both 2D and 3D objects, there are a number of operations we can perform to move those objects around in space.

The most commonly used are **translation**, **rotation**, and **scale**.
You can apply these operations to any object in Unity, and there is a separate "mode" for each one.
A fourth, less commonly used operation is **shear**.

All of these operations are called **transforms**.
Specifically, they are what we call affine transforms, because while they can change a lot about an object,
they preserve any parallel lines.

[You can see some 2D examples of these transforms here.](https://iondune.github.io/csc471/lectures/08-transformations)

An interesting thing about these affine transforms is that they can be represented by something called a matrix.
In fact, a matrix can represent both these transforms as well as composite transforms -
for example, a translation followed by a rotation.
Matrices, like vectors, have dimensions, but it takes a 4x4 matrix to represent all possible affine transforms in 3D.
This is really only important to know because often in game engines you will see a `mat4` type
that is used to represent various transformations.

These `mat4`s are just 4x4 matrices.
If you want to learn more, I suggest taking CSC 471.

### Translations and Scales

If we want to talk about translations or scales, we usually just refer to a set of three numbers
that describe the *amount* of translation/scale in each dimension.

A translation of $${0, 0, 0}$$ is an identity transform, or a transform that does nothing.
Similarly, a scale of $${0, 0, 0}$$ is an identity transform.

A translation of $${3, 4, 5}$$ will move an object 3 units on the $$\hat x$$ axis, 4 units on $$\vec y$$, and 5 units on $$\vec z$$.
A scale of $${2, 1, 0.5}$$ will make an object twice as long, the same height, and half as deep.

### Rotations

Rotations are not as simple to talk about, however.

If we look at Unity, we can see that we have the option to rotate an object around any of the three axis.
Generally speaking, the dimension that doesn't change when you perform a particular rotation
is the axis around which you are rotating.

However, despite the fact that we can think about rotations in three axis, it's not great to store them this way.
A group of three numbers that describe rotations applied around three axis in sequence is called **Euler angles**.
But they have some limitations that can be difficult to describe - so we'll have an in-class demo.

How else can we represent a rotation?

#### Complex Numbers

Let's take a look at complex numbers:

$$ (a, b) = a + b i $$

$$ a $$ is a real number.

$$ i $$ is an imaginary number.

It has some interesting rules: $$ i^2 = -1 $$

So we can multiply two complex numbers following the rules of algebra:

$$ (a + b i) * (c + d i) = $$

$$ a * c + a * d i + b i * c + b i * d i = $$

$$ ac + ad i + bc i + bd i^2 = $$

$$ (ac - bd) + (ad + bc) i $$

We can also write this:

$$ (a, b) * (c, d) = (ac - bd, ad + bc) $$

Alright, what else can we do with complex numbers?
We can plot them in 2D.

![complex-number](figures/complex-number.png){:class="img-thumbnail"}

Doing so shows us a few interesting aspects of complex numbers:

* If we multiply by $$ i $$, the number rotates around 90 degrees.
* If we multiply by $$ -1 = i^2 $$, the number rotates around 180 degrees.

What if we multiply by any other unit-length complex number?

$$ (a, b) * (cos\theta, sin\theta) = (a cos\theta - b sin\theta, a sin\theta + b cos\theta) $$

This gives us the result of rotating the vector $$\theta$$ degrees around the origin.
It makes sense because:

$$ i = (0, 1) = (cos(\frac{\pi}{2}), sin(\frac{\pi}{2})) $$, or a rotation of 90 degrees.

$$ -1 = i^2 = (-1, 0) = (cos(\pi), sin(\pi)) $$, or a rotation of 180 degrees.

$$ 1 = (1, 0) = (cos(0), sin(0)) $$, or a rotation of 0 degrees.

#### What about 3D?

This is rotation around a **single axis**, and we need **two** numbers to do it - a real and imaginary part.

We also use *unit-length* complex numbers, not just all of them.

To represent a 3D rotation, with is around **three axis**, we need **four** numbers to do it - a real and three imaginary parts.

[Euler's rotation theorem](https://en.wikipedia.org/wiki/Euler%27s_rotation_theorem) tells us that we can represent any arbitrary rotation in 3D
as a rotation around a single axis $$ \vec e $$ by a given rotation $$ \theta $$.

So we need exactly **four numbers** to describe an arbitrary rotation.

The number system with one real axis and one imaginary axis is the complex numbers.

The number system with one real axis and three imaginary axis is called **quaternions**.
Yes, this is a four dimensional coordinate system and thus complicated to think about and attempt to visualize.
What's important to know is that quaternions are a mathematically convenient way to store rotations,
so most game engines use them to represent rotations.

Unity uses a quaternion to store the rotation of every object,
but also has a way to access that same rotation in Euler angle form.

