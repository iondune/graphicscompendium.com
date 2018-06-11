---
layout: page
active: lectures
title: "Lecture 6: Matrices"
auto-title: true
---

A **matrix** is an array of elements that follow certain rules:

$$ A = \begin{bmatrix}1 & 6 & 3\\-1 & 2 & 4\end{bmatrix} $$

We use square brackets to enclose the array.

A matrix has $$ n $$ rows and $$ m $$ columns, and is said to be of order $$ n \times m $$.

Always mention the *rows* first.
Thus, the above example $$ A $$ is $$ 2 \times 3 $$

## Examples

$$ A = \begin{bmatrix}1 & 0 & 0\\0 & 1 & 0\\0 & 0 & 1\end{bmatrix} $$

is $$ 3 \times 3 $$


$$ B = \begin{bmatrix}1 & 1 & 0\end{bmatrix} $$

is $$ 1 \times 3 $$


$$ C = \begin{bmatrix}1\\-2\\9\\0\end{bmatrix} $$

is $$ 4 \times 1 $$


$$ D = \begin{bmatrix}1 & 0 & 0 & 2\\0 & 1 & 0 & 4\\0 & 0 & 1 & 5\\0 & 0 & 0 & 1\end{bmatrix} $$

is $$ 4 \times 4 $$

A matrix is **square** if the `# of rows` == `# of columns`.

Some common square matrices:

- **zero matrix** is all `0`s
- **identity matrix** is all `0`s except along the main diagonal (top left to bottom right) is `1`s

In computer graphics, we most commonly use square $$ 2 \times 2 $$, $$ 3 \times 3 $$, and $$ 4 \times 4 $$ matrices.



## Operations

Two matrices are equal if

- they have the same `# of rows and columns`
- their corresponding elements in each position are equal

$$ A = \begin{bmatrix}1 & 2\\3 & 4\end{bmatrix} $$

$$ B = \begin{bmatrix}1 & x\\y & 4\end{bmatrix} $$

If $$ A = B $$, then $$ x = 2 $$ and $$ y = 3 $$.


### Addition and Subtraction

Addition and subtraction of matrices is defined by addition and subtraction of corresponding elements.

*We can only add and subtract matrices of the same order.*

$$ \begin{bmatrix}2 & 1 & 3\\4 & 1 & -1\end{bmatrix} + \begin{bmatrix}3 & 2 & 0\\0 & 1 & 0\end{bmatrix} $$

$$ = \begin{bmatrix}2 + 3 & 1 + 2 & 3 + 0\\4 + 0 & 1 + 1 & -1 + 0\end{bmatrix} = \begin{bmatrix}5 & 3 & 3\\4 & 2 & -1\end{bmatrix} $$
