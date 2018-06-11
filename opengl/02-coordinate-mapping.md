---
layout: page
active: lectures
title: "Lecture 2: Coordinate Mapping"
auto-title: true
---


## Coordinate Mapping

To draw something, we need to transform from world to pixel!

Before transform, we must constrain the view into the world.


### General Form

In general, transforms can look like this:

$$ x_p = c * x_w + d $$

$$ y_p = e * y_w + f $$

Where $$ c $$ and $$ e $$ are scales and $$ d $$ and $$ f $$ are shifts.

(it's going to seem like we use these equations for this assignment and never again, but this is the basis for most of the transforms we will do this quarter)


We must constrain our world into a particular view.
We will determine a mapping from four world space values to pixel/screen space values.
On the left is world coordinates, on the right is pixel coordinates:

$$ L => 0 $$

$$ R => w $$

$$ B => 0 $$

$$ T => h $$

Where $$ w $$ and $$ h $$ are the width and height of the screen in pixels (e.g. `1024 x 768`).

$$ L $$, $$ R $$, $$ B $$, and $$ T $$ are the left, right, bottom, and top (respectively) of the viewport in world space.
We will use $$ -1 $$ for Left and Bottom, and $$ 1 $$ for Right and Top.


### Solve for $$ c $$ and $$ d $$

$$ 0 = c L + d $$

$$ d = - c L $$

So what about $$ c $$?

$$ w = c R + d $$

$$ w = c R - c L $$

$$ w = c ( R - L ) $$

$$ c = \frac{w}{R - L} $$

Plug back in to our $$ w $$ equation.

$$ d = \frac{-w}{R - L} * L $$

$$ d = \frac{-w * -1}{w} = \frac{w}{2} $$

Let's say our window is `320x240` and using the L/R/B/T values from above.

$$ d = \frac{w}{2} = \frac{240}{2} = 120 $$

And now plugging in to our $$ c $$ equation:

$$ c = \frac{w}{R - L} = \frac{240}{1 - (-1)} = \frac{240}{2} = 120 $$

So our general transformation from world coordinates into pixel coordinates is:

$$ x_p = c * x_w + d $$

$$ x_p = 120 * x_w + 120 $$

The formulation for y will be quite similar.


### Aspect Ratio

To account for aspect ratio, we need to pick different L and R values.

$$ L = -w / h $$

$$ R = w / h $$

$$ B = -1 $$

$$ T = 1 $$

