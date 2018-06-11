---
layout: page
active: lectures
title: "Lecture 10: Reflection and Refraction"
auto-title: true
---

Now that we have shading & shadows in our scene, what else is there other than geometric complexity?

We started with a purely local approximation of shading.
By adding shadows, we begin the process of adding global constraints to our system.



## Reflections

What else does light do?
Well, for mirror-like surfaces, very shiny surfaces, other part's of the scene's geometry is reflected onto those shiny surfaces.

We can model this type of surface reflection for reflective surfaces (e.g. any object with a `reflective` finish property greater than 0 in our `.pov` files)
by __adding__ color from any geometry intersected by reflected rays.

For some incident ray $$ \vec d $$ and normal $$ \vec n $$, the reflection vector is:

$$ \vec r = \vec d - 2 * (\vec d \cdot \vec n) * \vec n $$


### Implementation

You will need to do a recursive ray cast in order to gather reflected color.

Multiply the returned color by the `reflection` material property.

```pascal
r // reflection vector
pt // intersection point

reflection_color := raytrace(pt + r * epsilon, r)
total_color := local_color + finish.reflection * reflection_color
```


## Refractions

When light rays travel between materials they are sometimes bent.
This occurs because light travels at different velocities in different media.
The velocity of light in some medium is often represented by the **index of refraction** which is the ratio between the speed of light in that media over the speed of light in a vacuum, `c`.

Thus if a material has an index of refraction (commonly, "ior") of 2.0, then light travels at half of `c` in that media.

The border between two different media with different ior is called an interface.
When a light ray passes through an interface between media with different ior, it is bent.
This effect is called **refraction**.


### Snell's Law

Snell's Law tells us how much light will be bent, given the ior of each media at the interface and the incoming ray angle.

$$ n_1 sin(\theta) = n_2 sin(\phi) $$

Where $$ n_1 $$ is the ior of the first media (where the ray starts), $$ n_2 $$ is the ior of the media the ray is entering, and $$ \theta $$ and $$ \phi $$ are the angles of incidence and refraction, respectively.
Those angles are between the given vector and the normal of the interface.

In our ray tracers we are going to know $$ \theta $$, $$n_1$$, and $$n_2$$.
We can find $$ \theta $$ since we know the incident vector (it's $$ d $$, the direction of our ray).
We'll know $$n_1$$ and $$n_2$$ because we know where our ray is coming from and $$ n_2 $$ is a material property of the object we are shading.

That leaves $$ \phi $$ as the unknown.
Let's solve for it.


### Derivation

$$ n_1 sin(\theta) = n_2 sin(\phi) $$

$$ sin(\phi) = \frac{n_1}{n_2} sin(\theta) $$

`sin` is nice, but `cos` is better - we can write a cosine as a dot product.
So let's use trigonometric identities to change this to something-something cosine!

$$ sin^2(x) + cos^2(x) = 1 $$

We have $$ sin $$ but want $$ sin^2 $$ so let's just square our equation and apply the identity.

$$ sin^2(\phi) = \left(\frac{n_1}{n_2}\right)^2 sin^2(\theta) $$

$$ 1 - cos^2(\phi) = \left(\frac{n_1}{n_2}\right)^2 (1 - cos^2(\theta)) $$

$$ cos^2(\phi) = 1 - \left(\frac{n_1}{n_2}\right)^2 (1 - cos^2(\theta)) $$

$$ cos(\phi) = \sqrt{1 - \left(\frac{n_1}{n_2}\right)^2 (1 - cos^2(\theta))} $$

Great, but we actually want to be able to solve for the transmitted vector itself, not just the angle $$ \phi $$.
The angle would be annoying to use.

Let's solve for the vector $$ \vec t $$ which is the **transmitted**, or refracted, vector.

Where do we start?
Well, since we have an equation for $$ cos(\phi) $$ let's try to right something in terms of that.
Since $$ \vec t $$ is a normalized vector we can write it simply in terms of two orthonormal basis vectors.

$$ \vec t = cos(\sigma) * \vec a + sin(\sigma) * \vec b $$

This is a very general form that works for any angle $$ \sigma $$ and related basis vectors.
We want to use $$ \phi $$, but then we need to figure out what $$ \vec a $$ and $$ \vec b $$ are.
Well, $$ \vec a $$ is the opposite of the normal $$ \vec n $$ but we will need to solve for $$ \vec b $$.

$$ \vec t = cos(\phi) * (-\vec n) + sin(\phi) * \vec b $$

Where can we get more information about $$ \vec b $$?
Well, we still know $$ \vec d $$, so let's write it in terms of the same basis vectors!

$$ -\vec d = cos(\theta) * \vec n + sin(\theta) * (-\vec b) $$

$$ \vec d = -cos(\theta) * \vec n + sin(\theta) * \vec b $$

Now solve for $$ \vec b $$

$$ \vec b = \frac{\vec d + cos(\theta) * \vec n}{sin(\theta)} $$

And plug that into our equation for $$ \vec t $$

$$ \vec t = cos(\phi) * (-\vec n) + sin(\phi) * \frac{\vec d + cos(\theta) * \vec n}{sin(\theta)}  $$

Let's get rid of that $$ sin(\phi) $$ using Snell's Law:

$$ sin(\phi) = \left(\frac{n_1}{n_2}\right) sin(\theta) $$

$$ \vec t = cos(\phi) * (-\vec n) + \left(\frac{n_1}{n_2}\right) sin(\theta) * \frac{\vec d + cos(\theta) * \vec n}{sin(\theta)} $$

We can also cancel out the $$ \sin(\theta) $$s!

$$ \vec t = cos(\phi) * (-\vec n) + \left(\frac{n_1}{n_2}\right) \left(\vec d + cos(\theta) * \vec n \right) $$

And now plug in the equation for $$ \cos(\phi) $$ that we found before:

$$ cos(\phi) = \sqrt{1 - \left(\frac{n_1}{n_2}\right)^2 (1 - cos^2(\theta))} $$

$$ \vec t = (-\vec n) * \sqrt{1 - \left(\frac{n_1}{n_2}\right)^2 (1 - cos^2(\theta))} + \left(\frac{n_1}{n_2}\right) \left(\vec d + cos(\theta) * \vec n \right) $$

This is commonly written the other way around so let's just switch things up, it'll make it easier to read:

$$ \vec t = \left(\frac{n_1}{n_2}\right) \left(\vec d + cos(\theta) * \vec n \right) - \vec n * \sqrt{1 - \left(\frac{n_1}{n_2}\right)^2 (1 - cos^2(\theta))}$$

Finally, an equation in terms of $$ cos $$s, and we know the angle $$ \theta $$!
It's the angle of incidence, so some simple geometry tells us that:

$$ cos(\theta) = (-\vec d \cdot \vec n) $$

$$ \vec t = \left(\frac{n_1}{n_2}\right) \left(\vec d + (-\vec d \cdot \vec n) * \vec n \right) - \vec n * \sqrt{1 - \left(\frac{n_1}{n_2}\right)^2 (1 - (-\vec d \cdot \vec n)^2)}$$

Swap out minus signs or remove them where they don't matter:

$$ \vec t = \left(\frac{n_1}{n_2}\right) \left(\vec d - (\vec d \cdot \vec n) * \vec n \right) - \vec n * \sqrt{1 - \left(\frac{n_1}{n_2}\right)^2 (1 - (\vec d \cdot \vec n)^2)}$$


### Implementation

With transparent objects you will intersect the surface both when entering and exiting.
For exit refractions, make sure to:

- Flip the $$ \frac{n_1}{n_2} $$ to $$ \frac{n_2}{n_1} $$ since we are now *leaving* the media.
- Also flip the normal to $$ - \vec n $$

There are two material values that relate to refraction: `refraction` and `filter`.
`filter` tell us how transparent the material is (e.g. a value of `1.0` means the object is totally transparent, `0.0` means opaque).
`refraction` is redundant (any transparent material with an index of refraction is refractive) so we will ignore it.

