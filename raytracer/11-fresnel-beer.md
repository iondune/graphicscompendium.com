---
layout: page
active: lectures
title: "Lecture 11: Fresnel and Beer's Law"
auto-title: true
---


## Schlick's Approximation

**Schlick's Approximation** is a commonly used approximation for the Fresnel equations.

$$ F = F_0 + (1 - F_0)*(1 - (\vec n \cdot \vec v))^5 $$

Where $$ F_0 $$ is the reflectance at normal incidence, given by:

$$ F_0 = \frac{(n - 1)^2}{(n + 1)^2} $$

Where $$ n $$ is the material's index of refraction.

This is similar to the Fresnel contribution we had for Cook-Torrance, instead here we use the normal vector instead of halfway vector.
Remember that in Cook-Torrance, we summing contributions from microfacets with microsurface normal to the halfway vector.
In this case we are only concerned with the macrosurface normal.



## Beer's Law


Given distance travelled through a medium $$ d $$, for object with color $$ c $$:

$$ absorbance = (1 - c) * \alpha * -d $$

Where $$ \alpha $$ is a constant that we will arbitrarily define in this class:

$$ \alpha = 0.15 $$

We then multiply the color gathered by the refracted/transmitted ray by attenuation:

$$ attenuation = e^{absorbance} $$



## Shading

We have a handful of new `.pov` material constants as well as secondary rays which must now contribute to our final rendered color.

For this iteration of the class we will use the following (somewhat arbitrarily decided) system for determining the final color.

First we define the three computed colors:

- Local color: this is the result of the local shading computation, e.g. Blinn-Phong or Cook-Torrance.
  It includes the ambient, diffuse, and specular terms.
- Reflection color: this is the color found by recursively tracing along the reflection vector.
- Refraction color: this is the color found by recursively tracing along the transmitted (refraction) vector.

In addition we have the following material properties:

- `filter`: How transparent the material is
- `reflection`: How much light the material reflects

As `relection` increases, we need to show more reflected light and as a result, less local light.
Similarly, as `filter` increases, we need to show more refracted light and as a result, less local and reflected light.

Things get a little complicated when we throw Schlick's Approximation into the mix, because now our "refracted light" can also include a reflected component.

This is the model we will use to scale the three light contributions:

![ReflectionRefractionLight]({{ site.baseurl }}/images/ReflectionRefractionLight.png)

We will first split our total light into the "filter" and "non-filter" sides.
On the "filter" side, we have our transmitted light and some reflected light (as described by the Schlick's Approximation).
On the "non-filter" side, we will have our local shading computation as well as the reflected light as specified by the `.pov` material.


```pascal
r // reflection vector
pt // intersection point

local_color := blinn_phong(n, l, v) // or cook_torrance
reflection_color := raytrace(pt + r * epsilon, r)
transmission_color := raytrace(pt + t * epsilon, t)

fresnel_reflectance := schlicks_approximation(ior, n, v)
local_contribution = (1 - finish.filter) * (1 - finish.reflection)
reflection_contribution = (1 - finish.filter) * (finish.reflection) + (finish.filter) * (fresnel_reflectance)
transmission_contribution = (finish.filter) * (1 - fresnel_reflectance)

total_color :=
    local_contribution * local_color +
    reflection_contribution * reflection_color +
    transmission_contribution * transmission_color
```
