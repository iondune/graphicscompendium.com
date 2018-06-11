---
layout: page
active: lectures
title: "Lecture 9: Data-Oriented Design"
auto-title: true
---



[CppCon 2014: Mike Acton "Data-Oriented Design and C++"](https://www.youtube.com/watch?v=rX0ItVEVjHc)

> The purpose of all programs, and all parts of those programs, is to transform data from one form to another.
>
> If you don't understand the data, you don't understand the problem.
>
-- Mike Acton

There's essentially two core principles of DOD:

1. You need to understand the data to understand your software system - so design around the data.
2. There is never "one" of anything important - so design around the "multiple" case, not the single case.

### Bad OO Hierarchies

Mike say something similar to what Bob Martin says about classes.
Another good example to consider:
In the real world, chairs are all similar to other chairs.
In a game, you might have a variety of chair classes -
`physicsChair`, `breakableChair`, `staticChair`.
In your engine code, these objects have almost nothing in common,
so having them all inherit from `Chair` is a bad idea.

### Data Pipelines vs. Abstract Real-World Model

The problem with OOP is that it organizes the data of your program based on this abstract, real-world influenced model of your system.
But at the fundamental level, what we're doing is transforming data in ways that you likely have some understanding of.
Your rendering pipeline transforms data about scene objects into gpu-ready serialized data into render calls.
Your physics system (or collision detection system) transforms a list of positions and other data into a list of new positions and other data.
If you think of this as a pipeline from physics, to scene/render, to OpenGL, it becomes clear that that's what's going on.

### Cache Coherency vs. Data Isolation

One problem with OOD is that it isolates the data involved in various operations.
We usually do things one at a time - update physics, animate, render.
Yet we take the data for those operations and spread it all over memory.
This in turns makes systems where it can be difficult to reason about what is being updated when,
what data is involved in which operations,
and what the exact steps to perform an update are.

### On Branches, Caches, and Pre-mature Optimization

A common thread in DOD discussions is the idea that a complex object hierarchy designed to prune unnecessary updates
might cost more in cache-coherency (or lack thereof) and branches than it costs to simply always do the update.
This can include a discussion of the fact that a cache-miss is an order of magnitude more expensive than something
traditionally considered expensive, like a `sqrt` computation.

Nobody in this class should be micro-optimizing code.
But it's definitely worth considering that if updating everything every frame is the simplest thing,
and it may *possibly* be the fastest thing as well,
it might not be worth your time (and potential bugs) to implement a system that avoids calculating, e.g. absolute transform matrices,
until you *know* it's actually something that needs to be optimized.

I think it's also important to say that it's not that a hierarchy is never the right choice -
just that it's a very commonly used one and it comes with its own problems.
Later this quarter we're going to talk extensively about view-frustum culling, a technique that relies on hierarchical data structures.

### Why DOD?

Organizing your code around your data can also make these (traditionally troublesome) things easier to reason about in your engine:

- Multi-threading
- Networking
- Testing



## Example

Consider this node class that we might use as the base class for every object in our scene:

```cpp
class Node
{
public:
  void UpdateCollision();
  void UpdateAnimations();
  void UpdateTransforms();
  void Draw();

protected:
  Collision * m_Collision;
  vector<Animation *> m_Animations;

  mat4 m_Transform;
  mat4 m_AbsoluteTransform;
  vector<Node *> m_Children;
  Node * m_Parent;

  bool m_NeedsUpdate;

  MeshData * m_MeshData;
  Shader * m_Shader;
  vector<Texture *> m_Textures;
};
```

This approach by design spreads everything throughout memory.

Consider a Data-Oriented approach to this:

```cpp
struct CollisionData
{
  vec3 m_Position;
  vec3 m_Velocity;
  vec3 m_Bounds;
};

void UpdateCollisions(vector<CollisionData> data);

struct TransformData
{
  mat4 m_Transform;
  mat4 m_AbsoluteTransform;
  uint TransformParent;
};

void ApplyCollision(vector<CollisionData> collisions, vector<TransformData> xforms)
void ComputeAbsoluteTransforms(vector<TransformData> data);

struct RenderData
{
  uint shaderHandle;
  uint textureHandles[2];
  uint vboHandle;
  uint iboHandle;
};

void RenderAll(vector<TransformData> xforms, vector<RenderData> render);
```

We might still want `Animation` or other things in a more object-oriented approach,
depending on how they

One big take-away from DOD is that if you can figure out the main pieces of your pipeline (like above),
you can write things up in a very simple way that is easy to reason about and happens to also be incredibly friendly to caches (see: fast).
Abstraction for the sake of abstraction can be confusing for both the programmer and the cpu.



## Resources

- [Data Oriented Design - Great Overview](http://gamesfromwithin.com/data-oriented-design)
- [Data Locality - Game Programming Patterns Chapter](http://gameprogrammingpatterns.com/data-locality.html)
- [CppCon 2014: Mike Acton "Data-Oriented Design and C++"](https://www.youtube.com/watch?v=rX0ItVEVjHc)
- [Data Oriented Design with Balázs Török - CppCast Podcast](https://cppcast.com/2018/01/balazs-torok/)
- [Introduction to Data Oriented Design - Slide Deck](http://www.dice.se/wp-content/uploads/2014/12/Introduction_to_Data-Oriented_Design.pdf)
- [Practical Examples in Data Oriented Design - Slide Deck](https://docs.google.com/presentation/d/17Bzle0w6jz-1ndabrvC5MXUIQ5jme0M8xBF71oz-0Js/present#slide=id.i0)
