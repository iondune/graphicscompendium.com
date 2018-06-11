---
layout: page
active: lectures
title: "Lecture 18: Lookat Matrix"
auto-title: true
---


`glm::lookat(vec3 eye, vec3 center, vec3 up)`

- with only `eye` and `center`, infinite possibilities
- `up` tells us which one

How do we construct the matrix?



## Camera Basis Vectors

**U:** "side", "right", $$ camera_x $$

**V:** $$ camera_y $$

**W:** "direction", "forward", $$ camera_z $$

Actually, the **W** vector is the opposite of the "forward" vector of the camera.
This is because we are working in a *right-handed* coordinate system.

$$ forward = \frac{center - eye}{|| center - eye ||} $$

$$ U = forward \times up $$

$$ V = U \times forward $$

$$ W = -forward $$

### Camera Matrix

$$ C =
\begin{bmatrix}1 & 0 & 0 & P_x\\0 & 1 & 0 & p_y\\0 & 0 & 1 & p_z\\0 & 0 & 0 & 1\end{bmatrix} *
\begin{bmatrix}U_x & V_x & W_x & 0\\U_y & V_y & W_y & 0\\U_z & V_z & W_z & 0\\0 & 0 & 0 & 1\end{bmatrix}
=
\begin{bmatrix}U_x & V_x & W_x & P_x\\U_y & V_y & W_y & P_y\\U_z & V_z & W_z & P_z\\0 & 0 & 0 & 1\end{bmatrix}
$$

### View Matrix

What we want is the view matrix, inverse of the camera matrix.

$$ V = C^{-1} $$

Because $$ U $$, $$ V $$, and $$ W $$ are orthonormal basis vectors (and other matrix theorems etc.) the inverse of the above matrix is:

$$ V = C^{-1} =
\begin{bmatrix}U_x & U_y & U_z & -\vec P \cdot \vec U\\V_x & V_y & V_z & -\vec P \cdot \vec V\\W_x & W_y & W_z & -\vec P \cdot \vec W\\0 & 0 & 0 & 1\end{bmatrix}
$$

