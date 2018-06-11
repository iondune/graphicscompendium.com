---
layout: page
active: lectures
title: "Lecture 4: Planes"
auto-title: true
---

## Overview

A plane is defined by a point $$p_1$$ and a normal $$\hat n$$.
The plane consists of all points for which $$(p-p_1)$$ is perpendicular to $$\hat n$$.

Or, using the dot product:

$$ f(p) = (p - p_1) \cdot \hat n = 0 $$

Recall our equation for a ray:

$$ p(t) = p_0 + t * \hat d $$

Plugging in for $$p$$ gives us:

$$ f(p) = (p_0 + t * \hat d - p_1) \cdot \hat n = 0 $$



## Derivation

Solve for $$t$$:

$$ ((p_0 - p_1) + \hat d * t) \cdot \hat n = 0 $$

$$ (p_0 - p_1) \cdot \hat n + d * t \cdot \hat n = 0 $$

$$ (p_0 -p_1) \cdot \hat n = -t * (\hat d \cdot \hat n) $$

$$ t = \frac{(p_1 - p_0) \cdot \hat n}{\hat d \cdot \hat n} $$

But now... $$p_0$$ is the origin of our ray but we need a $$p1$$

Povray gives us planes in this format:

```POV-Ray
plane {
    <A, B, C>, D
}
```

Where `<A, B, C>` is the normal of the plane and `D` is the "distance" of the plane, or how far from the origin the plane is along the normal.
Therefore, $$\hat n * D$$ is a point on the plane.

$$ t = \frac{(\hat n * D - p_0) \cdot \hat n}{\hat d \cdot \hat n} $$

The dot product of a normalized vector with itself is 1, so we really have:

$$ t = \frac{D * (\hat n \cdot \hat n) - (p_0 \cdot \hat n)}{\hat d \cdot \hat n} $$

$$ t = \frac{D - (p_0 \cdot \hat n)}{\hat d \cdot \hat n} $$

This gives us $$t$$ in terms of known quantities.

## Other Plane Equation

It's important to note that there is another commonly used equation for a plane.

$$ A*x + B*y + C*z + D = 0 $$

It looks like we can use this equation with the four parameters given by the Povray plane specification.
However, for "simplicity", Povray actually gives us -D for this equation.
i.e. in Povray, the intended equation is:

$$ A*x + B*y + C*z - D = 0 $$

$$ A*x + B*y + C*z = D $$

You can also use this equation to find a $$p_1$$ for the plane.
