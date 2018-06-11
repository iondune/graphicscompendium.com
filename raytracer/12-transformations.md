---
layout: page
active: lectures
title: "Lecture 12: Transformations"
auto-title: true
---


## Introduction

Recall from 471, that the general model for handling modeling placement in teh world (and subsequent animations) was handled through matrix transforms.

$$ M * v = v^\prime $$

Where $$ M $$ is the model matrix, sometimes called the **current-transform matrix**, $$ v $$ is a given vertex, and $$ v^\prime $$ is the vertex transformed out of **object space** and into **world space**.
Object space is sometimes referred to as **model space**.

This allowed for instancing (perhaps used by yourselves in hierarchical modeling), where you defined an object in its object frame, which could then be transformed by matrices and multiple instanced created by specifying different transformation matrices.


### Transformation Sample Code

This C++ code shows how to construct a hierarchical arm using pure GLM transformation calls.
This is effectively a matrix stack construction.

```cpp
glm::mat4 m1 = glm::mat4(1.f);
m1 = glm::rotate(m1, Theta, glm::vec3(0, 0, 1));
m1 = glm::translate(m1, glm::vec3(1.75f, 0, 0));

glm::mat4 m2 = m1;
Sphere1Frame->SetTransformation(m2);
m2 = glm::scale(m2, glm::vec3(2, 1, 1));
SphereObject1->SetTransformation(m2); // Upper Arm

glm::mat4 m3 = m1;
m3 = glm::translate(m3, glm::vec3(1, 0, 0));
Sphere2Frame->SetTransformation(m3);
m3 = glm::rotate(m3, Phi, glm::vec3(0, 0, 1));
m3 = glm::translate(m3, glm::vec3(0.75f, 0, 0));
m3 = glm::scale(m3, glm::vec3(1.5f, 1, 1));
SphereObject2->SetTransformation(m3); // Fore Arm
```

We create two instances of the sphere object that are transformed (hierarchically) into world space.



## Ray Tracing

This same system of mesh instancing is useful for us in ray tracing, but there is another more valuable feature provided my matrix transforms.
Our sphere-ray intersection code is relatively straightforward compare to what ellipose-ray intersection code might look like.
But, an ellipsoid is just a sphere which has undergone scale and rotation transformations.
Those matrix transformations can be described just as we described transformations in 471.
So, if we can find a way to use matrix transformations in our ray tracers then we can support several new classes of objects.


### How?

In 471, we used the transformation matrix to transform the vertices of the model from object space to world space.
In 473, we can't really do that - we don't have vertices that represent our spheres, we have an equation.
What we can do instead is **transform the ray from world space into object space**.
Then we can compute intersections in object space.


### Object Space

Use the inverse of the model matrix to transform from world space to object space.

#### Positions vs. Vectors

Recall that the transformation matrix is a 4x4 matrix, so we need to use it to transform `vec4`s.
Also recall that the fourth coordinate of our vec4s indicates whether the vector is a **position** or a **vector**.

For positions, we set the fourth component to `1`.

```c++
glm::vec4 v = glm::vec4(position.x, position.y, position.z, 1.0);
```

For vectors/directions, we set the fourth component to `0`.

```c++
glm::vec4 v = glm::vec4(direction.x, direction.y, direction.z, 0.0);
```

#### Is Everything in Object Space?

We can compute intersections in object space, but we will still need to be able to shade in world space.
This means both computing light vectors and normals for shading, and also computing things like reflection and refraction vectors.

The two things we need to get from object space back to world space are the **intersection point** and the **surface normal**.


### Intersection Points

We could simply calculate the intersection point in object space (i.e. using the transformed ray) by multiplying the object space intersection point by the model matrix, but there is a simpler way.
Or rather, there is a way that doesn't require us to keep both the inverted and non-inverted model matrix.

*Side Note*: You should definitely compute the inverted model matrix for each object **ONCE**, after parsing, rather than inverting the model matrix at each intersection calculation.
Inverting a matrix is not cheap!

When you transform the ray into object space, there is a good chance that your ray direction $$ \vec d $$ is no longer a unit vector.
Normally in computer graphics, we get scared of any vector that's not a unit vector and immediately normalize it.
You must suppress this instinct in this case!
If you leave the $$ \vec d $$ vector as is, not normalized, then $$ t $$ values for that ray will be equivalent to $$ t $$ values for the original, untransformed, world space ray.

Thus you can compute the world space intersection point simply by plugging in the $$ t $$ intersection value to the world space ray equation, just like we have been doing.


### Normals

We also need to answer the question of normals.
We currently have a method to calculate normals of our objects in object space.
You might think we can simply take these normals and multiply by the model matrix to get into world space, but it's not always as simple as that.


### Normal Matrix

![CircleNormal]({{ site.baseurl }}/images/CircleNormalScaling.png)

When some surface undergoes a non-uniform scale transformation, the normals must be rescaled by the inverse of that transformation.
A matrix which contains the same rotation as the model matrix but with the inverse scale is called the **normal matrix**.
We can ignore the translation component of this matrix because we are transforming a normal with w component = 0.

The normal matrix $$ N $$ is constructed by taking the inverse transpose (tranpose of the inverse) of the model matrix:

$$ N = (M^{-1})^T $$

Note that by definition, the inverse-tranpose of any invertable matrix is the same as the tranpose-inverse, so we can effectively do this instead:

$$ N = (M^T)^{-1} $$

Just in case you were curious.

#### Why?

The **reason** why we can't just use the model matrix on the normals is because the normals are not vectors, they are bi-vectors.
When we transform the surface, the effect on the normal is different than how the surface itself is transformed.

If you're curious why the inverse transpose accomplishes what it does, it can be explained somewhat vaguely by noting that:

1. The transpose of a rotation is the inverse of a rotation.
2. The transpose of a scale is the original scale.

So the inverse-transpose of a matrix containing scale and rotation transformations will contain the original rotation but the inverted scale.



## Implementation

The interesting thing about this setup is that we don't ever need the model matrix in our ray tracing code:

* We use the inverse model matrix to transform rays into object space from world space.
* We can calculate our intersection points by using the object-space **t** value and the world-space ray equation.
* We need to use the **normal matrix** to transform normals out of object space into world space, and the normal matrix is the tranpose of the inverse model matrix.

Thus we can calculate the model matrix of the object as described in the `.pov` specification, compute the inverse, and store only that.


### POV-Ray

So, how do we calculate the model matrix?

Let's consider the following `.pov` snippet which describes the transformation of an object:

```
scale <1, 2, 1>
rotate <0, 0, 25>
translate <-1.2, 0, 0>
```

This means the object is:

* First, scaled
* Then, rotated
* Finally, translated

Accordingly, the equivalent `glm` code to produce this model matrix is:

```c++
glm::mat4 ModelMatrix = glm::mat4(1.f); // Identity matrix

ModelMatrix = glm::scale(glm::mat4(1.f), glm::vec3(1, 2, 1)) * ModelMatrix;
ModelMatrix = glm::rotate(glm::mat4(1.f), glm::radians(25.f), glm::vec3(0, 0, 1)) * ModelMatrix;
ModelMatrix = glm::translate(glm::mat4(1.f), glm::vec3(-1.2f, 0, 0)) * ModelMatrix;
```

Note that rotations in POV-Ray are specified as Euler angles in the `ZYX` order, so the glm code to produce the following rotation:

```
rotate <a, b, c>
```

would look like this:

```c++
glm::mat4 Rotation = glm::mat4(1.f);

Rotation = glm::rotate(glm::mat4(1.f), glm::radians(c), glm::vec3(0, 0, 1)) * Rotation;
Rotation = glm::rotate(glm::mat4(1.f), glm::radians(b), glm::vec3(0, 1, 0)) * Rotation;
Rotation = glm::rotate(glm::mat4(1.f), glm::radians(a), glm::vec3(1, 0, 0)) * Rotation;
```
