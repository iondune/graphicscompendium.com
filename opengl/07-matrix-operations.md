---
layout: page
active: lectures
title: "Lecture 7: Matrix Operations"
auto-title: true
---


## Simple Operations

### Scalar Multiplication

Simply multiply each element by the scalar.

$$ 3 * \begin{bmatrix}1 & 2\\2 & 1\end{bmatrix} = \begin{bmatrix}3 & 6\\6 & 3\end{bmatrix} $$


### Linear Combination

Not surprisingly, a linear combination of matrices is a matrix of the form:

$$ M = a_1 * M_1 + a_2 * M_2 + ... + a_m * M_m $$


### Transpose

To find the transpose of a matrix $$ A $$, with notation $$ A^T $$,
interchange rows and columns of matrix $$ A $$.

$$ A = \begin{bmatrix}1 & 6 & 3\\-1 & 2 & 4\end{bmatrix} $$

$$ A^T = \begin{bmatrix}1 & -1\\6 & 2\\3 & 4\end{bmatrix} $$

A **symmetric** matrix is identical to its tranpose.
Only a square matrix can be symmetric.



## Vector Operations

### Dot Product

The **dot product** of two vectors $$ v $$ and $$ w $$ is the sum of each of their corresponding elements.

$$ \vec v = \begin{bmatrix}3\\7\\-1\end{bmatrix} $$

$$ \vec w = \begin{bmatrix}4\\2\\6\end{bmatrix} $$

$$ \vec v \cdot \vec w = 3 * 4 + 7 * 2 + -1 * 6 = 12 + 14 - 6 = 20 $$

Note that this operation takes two vectors and returns a **scalar**.


### Normalization

The dot product of a vector with itself is the sum of the squares of its elements.

We can use this to compute a vector's length.

$$ \| \vec v \| = \sqrt{\vec v \cdot \vec v} = \sqrt{v_x^2 + v_y^2 + v_z^2} $$


### Unit Vectors

A **unit vector** is one with length = `1`

Unit vectors are useful and we sometimes need to take an arbitrary vector $$ \vec v $$ and **normalize** is to make it have unit length.
This often occurs when we care about the direction of a vector, but not its magnitude.

$$ \hat v = \frac{\vec v}{\| \vec v \|} = \frac{\vec v}{\sqrt{v \cdot v}} $$

Note the different symbol used here to denote a unit vector $$ \hat v $$, as opposed to $$ \vec v $$.


### Angles

We've seen how the dot product of a vector with itself relates to the length of the vector.

The dot product of a vector with another vector relates to the length of both vectors, as well as the angle between them.

Specifically:

$$ \vec a \cdot \vec b = \| a \| * \| b \| * cos\theta $$

Where $$ \theta $$ is the angle between the two vectors.

When using normalized vectors:

$$ \hat a \cdot \hat b = cos \theta $$

Thus the convenience of normalized vectors is discovered.

This is one of the most useful formulas used in graphics.
It is commonly used to compute the angle between two vectors, i.e.

$$ acos( \hat a \cdot \hat b) = \theta $$

We can use this to determine if two vectors are pointing away from one another or not.


### Projection

The projection of a vector $$ \vec a $$ onto vector $$ \vec b $$ is:

$$ (\vec a \cdot \vec b) * \vec b $$



## Matrix Multiplication

### By Vectors

A common use of matrix multiplication in computer graphics is to multiply a matrix times a vector.

For a given matrix $$ A $$ and vector $$ v $$, we say for $$ A * v $$ that the column vector $$ v $$ is pre-multiplied by $$ A $$.

Note that the number of rows of $$ A $$ must be the number of elements in $$ v $$.

The result of $$ A * v $$ is a column vector $$ c $$ whose $$ i^{th} $$ element is the dot product of the $$ i^{th} $$ row of $$A$$ with $$v$$.

$$ A * v = \begin{bmatrix}a_{1,1} & a_{1,2}\\a_{2,1} & a_{2,2}\end{bmatrix} * \begin{bmatrix}v_1\\v_2\end{bmatrix} $$

$$ = \begin{bmatrix}row_1 \cdot v\\row_2 \cdot v\end{bmatrix} $$

$$ = \begin{bmatrix}a_{1,1} * v_1 + a_{1,2} * v_2\\a_{2,1} * v_1 + a_{2,2} * v_2\end{bmatrix} $$


### By Matrices

Matrix by matrix multiplication $$ A * B $$ is only defined when the number of columns in $$ A $$ is the number of rows in $$ B $$.

Can we multiply $$ 3 \times 2 $$ with $$ 2 \times 4 $$? **Yes**

Can we multiply $$ 2 \times 3 $$ with $$ 2 \times 4 $$? **No**

Note that multiplying a matrix by the identity matrix produces the original matrix.

Note that matrix multiplication is not commutative.

$$ A * B \ne B * A $4
