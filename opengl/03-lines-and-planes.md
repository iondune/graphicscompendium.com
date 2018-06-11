---
layout: page
active: lectures
title: "Lecture 3: Lines and Planes"
auto-title: true
---


## Lines

Explicit equation:

$$ y = m * x + b $$


Implicit equation:

Consider two known points of a triangle $$(x_0, y_0)$$ and $$(x_1, y_1)$$.

We can use these points to determine the slope and plug into the explicit equation:

$$ y = \frac{(y_1 - y_0)}{(x_1 - x_0)} x + b $$

$$ 0 = (y_1 - y_0) x - (x_1 - x_0) y + c $$

$$ f(x, y) = (y_1 - y_0) x - (x_1 - x_0) y + c $$

Here we have replaced the unknown y-intercept $$ b $$ with another constant, $$ b * (x_1 - x_0) = c $$.
We don't really care what $$ b $$ was so we'll just move on with $$ c $$.

We know that $$(x_0, y_0)$$ is on the point, so let's plug in an solve for $$ c $$.

$$ f(x_0, y_0) = (y_1 - y_0) x_0 - (x_1 - x_0) y_0 + c = 0 $$

$$ c = (x_1 - x_0) y_0 - (y_1 - y_0) x_0 $$

$$ c = x_1 y_0 - x_0 y_0 - y_1 x_0 + y_0 x_0 $$

$$ c = x_1 y_0 - y_1 x_0 $$

Plug back in to our original equation with $$ c $$

$$ f(x, y) = (y_1 - y_0) * x - (x_1 - x_0) * y + x_1 * y_0 - y_1 * x_0 = 0 $$



## Planes

A plane is defined by a point $$p_1$$ and a normal $$\hat n$$.
The plane consists of all points for which $$(p-p_1)$$ is perpendicular to $$\hat n$$.

Or, using the dot product:

$$ f(p) = (p - p_1) \cdot \hat n = 0 $$

And to get a more commonly seen equation:

$$ f(x, y, z) = (x - p_x) * n_x + (y - p_y) * n_y + (z - p_z) * n_z = 0 $$

$$ = n_x * x + n_y * y + n_z * z - (n_x * p_x + n_y * p_y + n_z * p_z) $$

$$ = A * x + B * y + C * z + D $$

where

$$ A = n_x $$

$$ B = n_y $$

$$ C = n_z $$

$$ D = - (n_x * p_x + n_y * p_y + n_z * p_z) $$
