---
layout: page
active: lectures
title: "Lecture 19: Character Animation"
auto-title: true
---


<a href="https://docs.google.com/presentation/d/1arICH6hOCWwWcBlD0bK0rmZBTIB62g3IL1aUN1bqedY/edit?usp=sharing" class="btn btn-info">Slide Deck</a>


Dual Quaternions
----------------

Dual numbers form a number system similar to complex numbers.
Whereas complex numbers involve a real and imaginary part, dual numbers involve a real and a dual part.
Just like the imaginary part of a complex number is multiplied by $$ i $$,
the dual part of a dual number is multiplied by a new operator $$ \epsilon $$ such that

$$ z = r + d \epsilon $$

$$ \epsilon^2 = 0 $$

$$ \epsilon \ne 0 $$

Where $$ z $$ is the dual number, $$ r $$ is the real part, and $$ d $$ is the dual part.

A dual number is a number formed with two quaternions - a real quaternion and a dual quaternion, such that:

$$ q = q_r + q_d \epsilon $$

Where $$ q $$ is the dual quaterion, $$ q_r $$ is a quaternion representing the real part and $$ q_d $$ is a quaternion representing the dual part.

It turns out that a dual quaternion can be used to represent a composite rotation and translation.
The real part of the dual quaternion represents the rotation in standard quaternion form.
The imaginary part represents the translation multipled by the rotation:

$$ q_d = \frac{1}{2} t r $$

Where $$ t $$ is a quaternion formed by:

$$ t = \{0, t_x, t_y, t_z \} $$

Which is the standard way of representing a translation as a quaternion.

So what does this get us?
Well, we ordinary use 4x4 matrices to represent arbitrary transformations in 3D space.
However, there are a lot of times where we want rotational and translation components but we don't have any scale.
A composite rotation and translation is sometimes called a **rigid transform**.

If we're implementing rigid body physics, we might only need rotation and translation for each object in the simulation.
In character hierarchies, we very commonly have only rotation and translation for each joint configuration.
Using dual quaternions in these situations gives us a compact representation (8 floats) for something we might ordinarly need 16 floats for.

In addition, there are some techniques based on dual quaternions.
For example, dual quaternion skinning is an alternative to standard linear blend skinning that avoids certain artifacts.



References
----------

### Skinned Animation

* [OpenGL Wiki - Skeletal Animation](https://www.khronos.org/opengl/wiki/Skeletal_Animation)
* [(My) ionEngine - Skinned Animation Sample](https://github.com/iondune/ionEngineSamples/tree/master/SkinnedAnimation)
* [Skinning with Dual Quaternions](https://www.cs.utah.edu/~ladislav/dq/index.html)
* [A Beginners Guide to Dual-Quaternions](http://wscg.zcu.cz/wscg2012/short/A29-full.pdf)
* [Elastic-Inspired Deformers for Character Articulation](http://igl.ethz.ch/projects/eid/)

### Inverse Kinematics

* [Overview and Terminology of IK](https://medium.com/unity3danimation/overview-of-inverse-kinematics-9769a43ba956)
* [(My) ionEngine - Inverse Kinematics Sample](https://github.com/iondune/ionEngineSamples/tree/master/InverseKinematics)
* [FABRIK - Fast and Effective IK Technique](http://www.andreasaristidou.com/publications/papers/FABRIK.pdf)

