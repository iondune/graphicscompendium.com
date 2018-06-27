---
layout: page
active: lectures
title: "Lecture 4: Meshes"
auto-title: true
---

A **mesh** is generally just a set of polygons, with some associated data such as color, which we render.

- How many floats for a triangle? 9
- What about duplicated vertices? This is wasteful.

In computer graphics we will often store meshes as what's called an **indexed face-set**.


## Indexed Face-Set

An indexed face-set consists of two arrays that store the contents of the mesh.

The first is simply a list of vertices.
Each vertex is assigned an index, so the first vertex in the list is vertex 0, the next is vertex 1, etc.

The second array in the mesh representation is a list of indices.
Each group of three indices in the index list determines a triangle in the mesh.

So, if the first three elements of the indices are `0`, `6`, and `7`, then the first triangle in the mesh
consists of the vertices at index `0`, `6`, and `7`.

There's one aspect about this that gets quite confusing - three is both the number of floats per a 3D vertex as well as the number of vertices in a triangle
(and therefore the number of indices per triangle).
Make sure you're accounting for all of your threes!
