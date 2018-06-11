---
layout: page
active: lectures
title: "Lecture 11: OpenGL Pipeline 2"
auto-title: true
---

In this lecture we will modify the base code for Lab 3 to draw two objects, with two VAOs and two shaders.

[You can find the source code for this lab here.](https://github.com/calpoly-csc471/TwoObjectDemo)


## 1. Animation

[Changes](https://github.com/calpoly-csc471/TwoObjectDemo/commit/4666582a65debda3f7e4ffb9819c20fae86a8d4d)

First, we add the uTime variable like last lecture and use it to animate the triangle.
Note the fancy GLSL feature we use to add to the x and y components of `position`.

```glsl
position.xy += vec2(cos(uTime), sin(uTime)) * radius;
```

Even though position is a `vec3`, using this syntax lets us modify the `x` and `y` components
as if they were contained in a `vec2`.



## 2. Color

[Changes](https://github.com/calpoly-csc471/TwoObjectDemo/commit/d4c65fa51004b28b10db07f93e7efa365cd93b7f)

Next, we add a new uniform to specify the color of the triangle.
This is just another example of how to color something.



## 3. Two Render Calls

[Changes](https://github.com/calpoly-csc471/TwoObjectDemo/commit/61ed41c58109533d17d88c53c70b556e07ebb7bd)

Now things get interesting.
We add just three new lines to draw a second triangle.
By specifying two of the uniforms again, we can provide different values.
Then we add an additional `glDrawArrays` call.
Now there are two triangles on the screen, one is a different color, and they are in different positions!

Note that we didn't specify the other two uniforms `P` and `MV` - that's fine, their values carry over
from the previous `glUniform*` calls.
The same goes for the VAO - it's still bound, we can still use it.

This is one of the advantages of the state-system approach to the OpenGL API.
When parts of our scene description are shared we can leave them as is.
This is not only simple from the programming perspective, it also saves run time.



## 4. Two Shaders

[Changes](https://github.com/calpoly-csc471/TwoObjectDemo/commit/d7c4f8b887403673efada9afc74be2d9849485ab)

Of course we don't just want to render two of the same triangle.
Let's add a new shader and compile it.

Now, we **do** have to send the `P` and `MV` matrices again.
When switch to a different shader, all uniforms are cleared and we must start from scratch.

We're still using the same VAO though.
And note that we don't have to rebind it or anything - it remains bound even when switching to another shader.


---

[Changes](https://github.com/calpoly-csc471/TwoObjectDemo/commit/f14e5de90b690622ee456b2a0a1e0a937dddcb03)

Of course it won't be apparent that we're using a new shader unless it's different.
We increase the radius of the rotation animation.

Side note: there's no reason we couldn't just make `radius` a uniform and stick with just a single shader.
But the whole purpose of this exercise is to use two shaders, so we do it this way!



## 5. Two Separate Objects

[Changes](https://github.com/calpoly-csc471/TwoObjectDemo/commit/ce3fc9751dba392f723ced68da688a79baab6f40)

Finally, let's add a new VAO and VBO for our second object.
We'll make it a rhombus instead of a triangle.

Don't forget to make the `glDrawArrays` call use 6 vertices instead of 3.

Now the two draw calls are mostly separate.
They each have their own shader and VAO + VBO.
Separate data and separate rendering details - we can make them as different as we want.



## 6. GLSL Fun

[Changes](https://github.com/calpoly-csc471/TwoObjectDemo/commit/a1a0b3862a8421d48da3017f5b9ca13835094200)

Just for fun, let's make the rhombus look a little more interesting.

If we pass the `vertPos` as a varying from the vertex shader to fragment shader,
we'll have access to both object coordinates and image coordinates.

We can make a rather interesting effect using these two variables.

Note on this line another advanced usage of GLSL swizzle:

```glsl
color.rg = color.gr;
```

This is a quick way of swapping the red and green channels of the output color.
Swizzles let you do some interesting things.
For example, this expression is a `vec4` that contains four copies of the `z` element of some vector:

```glsl
vec.zzzz
```
