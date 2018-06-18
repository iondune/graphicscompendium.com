---
layout: page
active: lectures
title: "Lecture 15: Anti-Aliasing"
auto-title: true
---

[Slide Deck](https://docs.google.com/presentation/d/1QQX93-7boqMMRny6YIr4PC0r3gurClO07pdA262SoMU/edit?usp=sharing){:class="btn btn-info"}

## Super Sampling

### Pixel Centers

When we originally talked about camera rays, we set up our coordinate system such that we had one ray generated for each pixel.
This ray traveled through the center of the pixel, and we used the color found by this ray as the color we wrote out to our image file.

To implement super-sampling, we need to generate multiple rays per pixel.
Then, we average up the colors of each of these rays in order to determine the color of the pixel.
For example, this means that if we cast four rays for a given pixel, and two of the rays hit a purple sphere while two rays hit the black background,
the resulting color would be 50% black and 50% purple (i.e. dark purple).

Recall that originally our determination of camera space coordinates from pixel coordinates looked like this:

$$ U_s = -{1 \over 2} + \frac{i + {1 \over 2}}{w} $$

$$ V_s = -{1 \over 2} + \frac{j + {1 \over 2}}{h} $$

Where $$ U_s, V_s $$ are the camera space coordinates for a given pixel $$ i, j $$ of an image with size $$ w, h $$.


### Super Samples

But this gives us a single coordinate in the center of the pixel.
We instead want to have an $$ n \times n $$ grid of rays within that pixel.

A $$ 2 \times 2 $$ grid of super samples (sometimes called `2x super sampling`) will produce a significantly improved image.
However, it will take up to four times as long to render.

A $$ 3 \times 3 $$ grid of super samples (sometimes called `3x super sampling`) will produce an image similar to a `2x` but perhaps slightly smoother.
However, it will take up to nine times as long to render.

In general, increasing the super sample parameter significantly increases the render time but will produce diminishing returns.


### Equations

Note that if we consider the bounds of this particular pixel, trying to find the "super sample locations" is quite similar to trying to find the "pixel center locations" within the image as a whole.
In particular, it is exactly the same:

$$ Pixel_x = -{1 \over 2} + \frac{m + {1 \over 2}}{s} $$

$$ Pixel_y = -{1 \over 2} + \frac{n + {1 \over 2}}{s} $$

Where $$ Pixel_x, Pixel_y $$ is the coordinate within the tiny pixel region for a given super sample $$ m, n $$ when there are $$ s \times s $$ super samples.

Recall that $$ i, j $$ in our original camera ray equation indicated the pixel of concern.
We can simply add our new offset to this pixel location to produce an equation for super sample locations.

$$ U_s = -{1 \over 2} + \frac{i + {1 \over 2} + \left(  -{1 \over 2} + \frac{m + {1 \over 2}}{s}  \right) }{w} $$

$$ V_s = -{1 \over 2} + \frac{j + {1 \over 2} + \left(  -{1 \over 2} + \frac{n + {1 \over 2}}{s}  \right) }{h} $$

Which can be simplified to:

$$ U_s = -{1 \over 2} + \frac{i + \frac{m + {1 \over 2}}{s} }{w} $$

$$ V_s = -{1 \over 2} + \frac{j + \frac{n + {1 \over 2}}{s} }{h} $$

Where $$ i, j$$ is the pixel, $$ m, n $$ is the sub-pixel, $$ w, h $$ is the image size and $$ s $$ is the number of sub-pixel super-samples.

By multiplying by $$s$$, we can see that this is equivalent to treating the image as $$s$$ times as large:

$$
\begin{align}
U_s & = -{1 \over 2} + \frac{i + \frac{m + {1 \over 2}}{s} }{w} \\
& = -{1 \over 2} + \frac{i + \frac{m + {1 \over 2}}{s} }{w} * \frac{s}{s} \\
& = -{1 \over 2} + \frac{i * s + m + {1 \over 2} }{w * s}
\end{align}
$$

This is what we get in our original equation for $$ U_s $$ (without supersampling)
if we replace $$ i $$ with $$ i * s + m $$ and replace $$ w $$ with $$ w * s $$.
