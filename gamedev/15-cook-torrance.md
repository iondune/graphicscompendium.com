---
layout: page
active: lectures
title: "Lecture 15: Cook-Torrance"
---

Cook-Torrance Reflectance Model
===============================

In 1982, Robert Cook and Kenneth Torrance published a reflectance model that more accurately
represented the physical reality of light reflectance than the Phong and Blinn-Phong models.

The proposed method is general purpose - it has three "pluggable" components that can be replaced with equations of your choice.
It is also effective at representing a variety of materials, whereas a BRDF like Blinn-Phong is
really only good at representing plastics and some metals (though that point is debatable).
As such, it is still in common usage today.
Though that is perhaps a moot point, since the Phong reflection model is a weak approximation from 1975 that is still in common usage.
Regardless...



## Equations

With previous shading models, we had something of the form

$$ r = k_a + \sum_{i=0}^n l_c * ( r_d + r_s ) $$

With

$$ r_d = k_d * (\vec n \cdot \vec l) $$

$$ r_s = k_s * (\vec h \cdot \vec n) ^ p $$

To compute total reflectance $$ r $$ with the Blinn-Phong model.
Here we have defined $$ r_d $$ for the diffuse reflectance and $$ r_s $$ for the specular reflectance.
Note the "power" term of the specular highlight is now $$ p $$ to avoid confusion with forcoming equations.

This is the same equation we have been using and have already implemented.

Cook and Torrance propose a variation of this equation:

$$ r = k_a + \sum_{i=0}^n l_c * (\vec n \cdot \vec l) * ( d * r_d + s * r_s ) $$

With

$$ r_d = k_d $$

This equation introduces two concepts:

1. The diffuse and specular reflectance components are now scaled by some variables $$ s $$ and $$ d $$.
2. We pull the $$ \vec n $$ and $$ \vec l $$ dot product out of the diffuse reflectance and make it a part of the sum.
  This makes our $$ r_d $$ a constant, though we can modify this definition later if we choose to.
  This also implies that our $$ r_s $$ specular reflectance is also multiplied by the same dot product (unlike before in Blinn-Phong).

As we will soon learn, however, $$ r_s $$ now includes a $$ (\vec n \cdot \vec l) $$ divisor, which will simply cancel out with the $$ (\vec n \cdot \vec l) $$ in the numerator.
This convention is adopted for two reasons:

1. It comes from the derivation of the forthcoming $$ r_s $$ equation (specifically, since it is a microfacet model).
2. To be confusing.

Those I suspect that the second reason is only from my perspective.

Nevertheless, note that many texts and explanations adopt this convention.


### What about $$ s $$ and $$ d $$ ?

$$ s $$ and $$ d $$ are a constant ratio that balances out the diffuse and specular contributions to our reflectance.

$$ s + d = 1 $$

Sometimes we ignore the $$ s $$ altogether and simply write:

$$ r = k_a + \sum_{i=0}^n l_c * (\vec n \cdot \vec l) * ( s * r_d + (1 - s) * r_s ) $$

This ratio is based on the observation that (except for emissive materials), conservation of energy implies that a material can only
reflect as much energy (light) as it takes in.

Therefore if a material reflects a lot of light specularly, then it must reflect less light diffusely.


### What about $$ r_s $$ ?

I'm getting there.

$$ r_s = \frac{D * G * F}{4 * (\vec n \cdot \vec l) * (\vec n \cdot \vec v)} $$

Note the n-dot-l divisor that I warned would be there!

Note also that in the literature and other places you will sometimes find $$ \pi $$ instead of $$ 4 $$ in the denominator.
*This is an error that was made in the original Cook-Torrance paper and has thus been repeated in many places.*
The correct derivation of this equation has the $$ 4 $$.
It's a pretty small difference in a constant factor, though, so it's not incredibly important to get it right.


### What about $$ D * G * F $$ ?

$$ D $$, $$ G $$, and $$ F $$ are the three "pluggable" functions that make up the Cook-Torrance specular reflectance.
You can choose from a wide variety of options for these equations.
To talk about what they mean, however, we need to discuss the core theory behind the Cook-Torrance model.



## Microfacets

Cook-Torrance is a microfacet model, which means that it approximates surfaces as a collection of small individual faces called microfacets.
You can think of these microfacets as being polygonal approximations of the surface at a sub-pixel level.

![Microfacets](15-figure-microfacet.png)

The important concept here is that the normal of the surface and the normals of each microfacet may not be the same.
For exceptionally smooth surfaces, like a perfect mirror, all the microfacets face the same direction which is $$ \vec n $$, the normal of the surface.
However, for matte or rough surfaces, such as matte paint or stone, the microfacets face random directions.

On rough surfaces, these microfacets can occlude other facets, cast shadows on facets, and be pointing away from the normal.

The Cook-Torrance model attempts to account for these three phenomena.


### So... what about $$ D * G * F $$ ?

$$ D $$ is the Normali Distribution Function, which accounts for the fraction of facets which reflect light at the viewer.

$$ G $$ is the Geometric Attenuation Function, which accounts for the shadowing and masking of facets by one another.

$$ F $$ is the Fresnel Function, which accounts for the *Fresnel* Effect, which causes rays with high angle of incidence to reflect with more specularity.

$$ G $$ and $$ F $$ have a less significant contribution to the final shaded result than $$ D $$, so we will cover $$ D $$ first.



## Normal Distribution

$$ D $$ is the part of the Cook-Torrance model which defines the shape of the specular highlight.
It describes the concentration of facets oriented to reflect specularily, e.g. facets for which $$ m == h $$
There are a lot of choices here.
It may not be surprising that one of the choices is **Blinn-Phong**, that is, we can choose to use the same specular highlight shape as we previously used in the Blinn-Phong reflectance model.

$$ D_{blinn} = \frac{1}{\pi \alpha^2} (\vec h \cdot \vec n)^{\left( \frac{2}{\alpha^2} - 2 \right)} $$

Now this looks sort of familiar!
Except, we are dividing by something now... and our clean "power" exponent has been replaced by something more nasty.

Let's explain these two changes - starting with the divisor.


### Normalization

Our $$ D $$ functions for Cook-Torrance need to be normalized such that:

$$ \int_\Omega D(h)(\vec h \cdot \vec n) d\omega_i = 1 $$

The result of this normalization in our Blinn-Phong case (and indeed, for many different cases) is a factor of $$ \frac{1}{\pi \alpha^2} $$

But what is $$ \alpha $$ ?


### Power exponent

In traditional Blinn-Phong we had a simple power parameter (usually called `shininess`) that we used to control the size and intensity of the specular highlight.

In Cook-Torrance we want to have a more general parameter that can be used in multiple places.
Cook-Torrance defines a constant $$ \alpha $$ that indicates the roughness of the material, with `0` indicating ideal smooth surfaces and `1` indicating maximum roughness.
In practice you never want to use absolute `0` or `1` since these edge cases tend to produce divide-by-zero and other issues.

We can then relate our new $$ \alpha $$ parameter to our old `power` variable as such:

$$ power = \left( \frac{2}{\alpha^2} - 2 \right) $$

`roughness` is one of the material properties defined by our `.pov` files.

$$ \alpha = roughness^2 $$

Note that for traditional Blinn-Phong (specifically, not just $$D_{blinn}$$ but if we aren't doing Cook-Torrance at all), you should still use the above power equation for `shininess`,
but you don't need to square the roughness constant:

$$ power = \left( \frac{2}{roughness^2} - 2 \right) $$

This is an arbitrary convention but I have found it works reasonably well!


### Other Choices

As I have mentioned, there are other choices for Normal Distribution function.
Instead of Blinn-Phong, the original authors Cook and Torrance recommend the *Beckmann* distribution.
Modern authors usually recommend the more recent *GGX* distribution.

However, these is enough to process without adding these equations in as well.
For now we'll just stick with the Blinn-Phong distribution function, as it is good enough.
Indeed, the Blinn-Phong and Beckmann functions are quite similar for many values of $$ \alpha $$.

For additional function choices for $$ D $$, as well as choices for $$ G $$ and $$ F $$, can be found in the [reference]({{ site.baseurl }}/references/cook-torrance).

Now we will cover $$ G $$ and $$ F $$.



## Geometric Attenuation

$$ G $$ is the part of the Cook-Torrance model which accounts for attenuation caused by neighboring microfacets.
It describes the fraction of microfacets which are neither occluded or shadowed.
Microfacets can block the light from reaching other microfacets, and they can also obscure the microfacets from the viewer's perspective.
This effect is more prominent on rougher materials.

$$ G $$ is therefore a number from 0 to 1 which indicates the proportion of light that is **not** blocked by either of these effects.
As such it is defined as the minimum of two different factors which calculate either type of attenuation.

$$ G = min \left( 1, \frac{2 (\vec h \cdot \vec n) (\vec n \cdot \vec v)}{(\vec v \cdot \vec h)}, \frac{2 (\vec h \cdot \vec n) (\vec n \cdot \vec l)}{(\vec v \cdot \vec h)} \right) $$



## Fresnel

$$ F $$ is the part of the Cook-Torrance model which accounts for the *fresnel* effect.

In computer graphics the most commonly used approximation for the fresnel effect is **Schlick's Approximation**:

$$ F = F_0 + (1 - F_0)*(1 - (\vec v \cdot \vec h))^5 $$

Where $$ F_0 $$ is the reflectance at normal incidence, given by:

$$ F_0 = \frac{(n - 1)^2}{(n + 1)^2} $$

Where $$ n $$ is the material's index of refraction.

And if you're wondering, yes! Opaque materials can and do still have indices of refraction.
See [here](https://physics.stackexchange.com/questions/223048/how-do-opaque-materials-have-an-index-of-refraction).



## References

[Coding Labs](http://www.codinglabs.net/article_physically_based_rendering_cook_torrance.aspx)
Covers Cook-Torrance with diagrams, in the context of PBR renderer using Monte-Carlo IBL

[RenderMan](https://renderman.pixar.com/view/cook-torrance-shader)
Pixar's explanation of Cook-Torrance in the context of RenderMan, their offline rendering system and associated shader language.
Not completely useful to us since it skips some aspects that are important to us (and either implicit or ignored in their case).

[Walter et. al.](https://www.cs.cornell.edu/~srm/publications/EGSR07-btdf.pdf)
*Microfacet Models for Refraction through Rough Surfaces*.
Introduces the GGX functions.

[Graphic Rants](http://graphicrants.blogspot.com/2013/08/specular-brdf-reference.html)
Very informative blog post from a UE4 dev describing different function choices for D, F, G.

[SIGGRAPH 2012](http://blog.selfshadow.com/publications/s2012-shading-course/#course_content)
Several presentations including general PBR exploration.

[SIGGRAPH 2015](http://blog.selfshadow.com/publications/s2015-shading-course/hoffman/s2015_pbs_physics_math_slides.pdf#57)
More recent slides from the previous lesson.

[SIGGRAPH 2010](http://renderwonk.com/publications/s2010-shading-course/hoffman/s2010_physically_based_shading_hoffman_b_notes.pdf)
Some generally useful coursenotes about correct lighting.

[ruh.li](http://ruh.li/GraphicsCookTorrance.html)
Pretty straight-forward explanation of Cook-Torrance with sample code.

[stackgl](https://github.com/stackgl/glsl-specular-cook-torrance/blob/master/index.glsl) and [stackgl](https://github.com/stackgl/glsl-specular-beckmann/blob/master/distribution.glsl)
Sample glsl code implementing Cook-Torrance reflection model.

[Correct Specular Term?](https://computergraphics.stackexchange.com/questions/3946/correct-specular-term-of-the-cook-torrance-torrance-sparrow-model)
StackExchange question about variations in the Cook-Torrance equation.

[Importance Sampling?](https://computergraphics.stackexchange.com/questions/4979/what-is-importance-sampling)
StackExchange question about importance sampling, with some useful explanation of Cook-Torrance components.

[Shininess to roughness?](https://computergraphics.stackexchange.com/questions/1515/what-is-the-accepted-method-of-converting-shininess-to-roughness-and-vice-versa)
StackExchange question about how to convert from Blinn-Phong shininess to Cook-Torrance roughness.

[Frostbite](http://www.frostbite.com/wp-content/uploads/2014/11/course_notes_moving_frostbite_to_pbr.pdf)
Paper on how Frostbite converted their engine to PBR (Cook-Torrance).

[Disney BRDF](https://disney-animation.s3.amazonaws.com/library/s2012_pbs_disney_brdf_notes_v2.pdf)
Disney paper with lots of BRDF discussion.

