---
layout: page
active: lectures
title: "Lecture 8: Cook-Torrance GGX"
auto-title: true
---


## Power exponent

In traditional Blinn-Phong we had a simple power parameter (usually called `shininess`) that we used to control the size and intensity of the specular highlight.

In Cook-Torrance we want to have a more general parameter that can be used in multiple places.
Cook-Torrance defines a constant $$ \alpha $$ that indicates the roughness of the material, with `0` indicating ideal smooth surfaces and `1` indicating maximum roughness.
In practice you never want to use absolute `0` or `1` since these edge cases tend to produce divide-by-zero and other issues.

We can then relate our new $$ \alpha $$ parameter to our old `power` variable as such:

$$ \text{power} = \left( \frac{2}{\alpha^2} - 2 \right) $$

`roughness` is one of the material properties defined by our `.pov` files.
We'll use UE4's convention for determining $$\alpha$$ from roughness:

$$ \alpha = \text{roughness}^2 $$

Note that for traditional Blinn-Phong (specifically, not just $$D_{blinn}$$ but if we aren't doing Cook-Torrance at all), you should still use the above power equation for `shininess`,
but you don't need to square the roughness constant:

$$ \text{power} = \left( \frac{2}{\text{roughness}^2} - 2 \right) $$

This is an arbitrary convention but I have found it works reasonably well!



## GGX Equations

### Normal Distribution Function


![Cook_Torrance_D_GGX]({{ site.baseurl }}/images/Cook_Torrance_D_GGX.png)

$$ \large
D_{GGX} =
\chi^+ (\vec n \cdot \vec h)
\frac
{ \alpha^2 }
{ \pi \left((\vec n \cdot \vec h)^2 *(\alpha^2 - 1) + 1\right)^2}
$$

$$\chi^+$$ is the positive characteristic function:

$$ \large
\chi^+(a) =
\begin{cases}
1 & \text{if $a > 0$} \\
0 & \text{if $a \leq 0$}
\end{cases}
$$


### Geometric Shadowing Function

![Cook_Torrance_G_GGX]({{ site.baseurl }}/images/Cook_Torrance_G_GGX.png)

$$ \large
G_{GGX} =
G_1(\vec v) * G_1(\vec l)
$$

$$ \large
G_1(x) =
\chi^+(\frac{\vec x \cdot \vec h}{\vec x \cdot \vec n})
\frac{2}{1+\sqrt {1 + \alpha^2 tan^2(\theta_x))}}
$$

$$\theta_x$$ is the angle between $$\hat x$$ and $$\hat n$$.

$$ tan^2(\theta) = sec^2(\theta) - 1 $$
$$ = \frac{1}{cos^2(\theta)} - 1 $$
$$ = \frac{1 - cos^2(\theta)}{cos^2(\theta)} $$

$$
tan^2(\theta_x) = \frac{1 - (\vec x \cdot \vec n)^2}{(\vec x \cdot \vec n)^2}
$$


## Implementation Details

*Every* dot product in these equations should be "saturated", i.e. clamped between 0 and 1.

