---
layout: page
active: references
title: "Cook-Torrance Components"
auto-title: true
---

General form of the Cook-Torrance shading model:

$$ r = k_a + \sum_{i=0}^n l_c * (\vec n \cdot \vec l) * ( d * r_d + s * r_s ) $$

$$ s + d = 1 $$

$$ s $$ in this equation is the `specular` material property, which we can use directly from our `.pov` files.

Diffuse reflection is often modelled using Lambertian:

$$ r_d = k_d $$

There are other options, however, including Oren-Neyer.

Cook-Torrance specular reflectance (also sometimes simply called microfacet reflectance or microfacet BRDF) has the form:

$$ r_s = \frac{D * G * F}{4 * (\vec n \cdot \vec l) * (\vec n \cdot \vec v)} $$


$$ D $$ is the Normal Distribution Function, which describes the concentration of microfacets which are oriented such that they *could* reflect light. [(1)](#ref-naty)

$$ G $$ is the Geometric Shadowing-Masking Function, which describes the percentage of microfacets which are neither shadowed or masked by neighboring microfacets. [(1)](#ref-naty)

$$ F $$ is the Fresnel Function, which describes the amount of light reflected by the material (as opposed to being absorbed or transmitted by the material).

## Symbols

$$ \vec n $$ is the macrosurface normal.

$$ \vec l $$ is the light vector.

$$ \vec v $$ is the view vector.

$$ \vec m $$ is the microsurface normal - in these computations (i.e. for implementation purposes) the half-vector is used.

$$ \theta_m $$ is the angle between $$ \vec m $$ and $$ \vec n $$

$$ \alpha $$ is the roughness constant.
$$ \alpha $$ is defined as such using the `roughness` material property from our `.pov` files: [(4)](#ref-rants)

$$ \alpha = roughness^2 $$

## Normal Distribution Functions

The $$ D $$ function is always positive. [(2)](#ref-ggx)
It is also normalized to satisfy certain geometric properties. [(3)](#ref-ndf)

This normalization is the source of the $$ \frac{1}{\pi \alpha^2} $$ factors you see in the Blinn-Phong and Beckmann variations.

$$ D(m) $$ is the concentration of microfacets oriented with subsurface normal $$ \vec m $$.
For our microfacet model we plug in $$ \vec h $$ since we want $$ D(h) $$, the concentration of microfacets which can reflect light from $$ \hat l $$ to $$ \hat v $$. [(1)](#ref-naty)

### Blinn-Phong

![Cook_Torrance_D_BlinnPhong]({{ site.baseurl }}/images/Cook_Torrance_D_BlinnPhong.png)

$$
\Large D_{blinn} = \frac{1}{\pi \alpha^2} (\vec n \cdot \vec m)^{power}
$$

Here, $$ power $$ is the "shininess" or "specular power".
There doesn't seem to be a standard way to compute this from $$ \alpha $$ or `roughness`.
Past iterations of CSC 473 have used the reciprical of POV-Ray roughness:

$$
power = \frac{1}{roughness}
$$

I recommend (and use) the equation provided by Karis': [(4)](#ref-rants)

$$
power = \left( \frac{2}{\alpha^2} - 2 \right)
$$


### Beckmann

![Cook_Torrance_D_Beckmann]({{ site.baseurl }}/images/Cook_Torrance_D_Beckmann.png)

This is the NDF which Cook and Torrance originally advocated for in [(5)](#ref-ct).

$$
\Large D_{beckmann} = \frac{1}{\pi \alpha^2}
\frac
{e^{\frac{-tan^2(\theta_m)}{\alpha ^2}}}
{(\vec n \cdot \vec m)^4}
$$

Note that for normalized vectors, the following are equivalent:

$$
cos^4(\theta_m) = (\vec n \cdot \vec m)^4
$$

And by trigonometric identity, the following are equivalent:

$$
-tan^2(\theta_m) = \left(\frac{(\vec n \cdot \vec m)^2 - 1}{(\vec n \cdot \vec m)^2}\right)
$$

With those substitutions, you find the common form of the Beckmann NDF.

### GGX

![Cook_Torrance_D_GGX]({{ site.baseurl }}/images/Cook_Torrance_D_GGX.png)

$$
\Large D_{GGX} =
\frac
{\alpha^2 \chi^+ (\vec n \cdot \vec m)}
{\pi \left((\vec n \cdot \vec m)^2 *(\alpha^2 + tan^2(\theta_m))\right)^2}
$$



## Geometric Functions

### Cook-Torrance

![Cook_Torrance_G_Cook_Torrance]({{ site.baseurl }}/images/Cook_Torrance_G_Cook_Torrance.png)

$$ \Large
G_{cook-torrance} =
min \left( 1, \frac{2 (\vec n \cdot \vec m) (\vec n \cdot \vec v)}{(\vec v \cdot \vec h)}, \frac{2 (\vec n \cdot \vec m) (\vec n \cdot \vec l)}{(\vec v \cdot \vec h)} \right)
$$


### GGX

![Cook_Torrance_G_GGX]({{ site.baseurl }}/images/Cook_Torrance_G_GGX.png)

$$ \Large
G_{GGX} =
G_1(\vec v) * G_1(\vec l)
$$

$$ \Large
G_1(x) =
\chi^+(\frac{\vec x \cdot \vec m}{\vec x \cdot \vec n})
\frac{2}{1+\sqrt {1 + \alpha^2 tan^2(\theta_x))}}
$$

$$\chi^+$$ is the positive characteristic function:

$$ \large
\chi^+(a) =
\begin{cases}
1 & \text{if $a > 0$} \\
0 & \text{if $a \leq 0$}
\end{cases}
$$

## Fresnel Functions

### Schlick's Approximation

$$ F_0 = \frac{(n - 1)^2}{(n + 1)^2} $$

$$ F = F_0 + (1 - F_0)*(1 - (\vec v \cdot \vec h))^5 $$



## References

<a name="ref-naty"></a>
1: [Naty Hoffman, **Background: Physics and Math of Shading**, 2012](http://blog.selfshadow.com/publications/s2012-shading-course/hoffman/s2012_pbs_physics_math_notes.pdf).
From the [SIGGRAPH 2012 Practical Physically Based Shading in Film and Game Production](http://blog.selfshadow.com/publications/s2012-shading-course)

<a name="ref-ggx"></a>
2: [Walter et al., **Microfacet Models for Refraction through Rough Surfaces**, 2007](http://www.cs.cornell.edu/~srm/publications/EGSR07-btdf.pdf).
Introduced the GGX form of D and G functions.

<a name="ref-ndf"></a>
3: [Nathan Reed, **How Is The NDF Really Defined?**, 2013](http://www.reedbeta.com/blog/hows-the-ndf-really-defined/).
Describes where the normalization factor of D function comes from.

<a name="ref-rants"></a>
4: [Brian Karis, **Specular BRDF Reference**, 2013](http://graphicrants.blogspot.com/2013/08/specular-brdf-reference.html).
Reference of D, F, and G functions.
Written by a UE4 graphics programmer.

<a name="ref-ct"></a>
5: [Robert Cook & Kenneth Torrance, **A Reflectance Model for Computer Graphics**, 1982](http://inst.cs.berkeley.edu/~cs294-13/fa09/lectures/cookpaper.pdf).
Original Cook-Torrance paper.
Many of the equations in this paper have slightly different forms in modern usage, so beware.
