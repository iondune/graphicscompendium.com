---
layout: page
active: lectures
title: "Lecture 8: Object-Oriented Design"
auto-title: true
---



To talk about object-oriented design, let's distill some thoughts from Bob Martin.
I would also recommend giving the whole talk a listen as well: [Bob Martin SOLID Principles of Object Oriented and Agile Design](https://youtu.be/TMuno5RZNeE?t=12m19s)

Bob Martin defines some terms that can help us thing about software design and object-oriented programming.

**Modules** general term of a component of the software system.
Can be a small as a function or class, generally refers to a group of functions and classes that serve a single purpose.

**Rigidity** is when making a change to your system requires changes all over the code base.

**Fragility** is when making a change to your system cause seemingly unrelated modules to break.

**Dependencies** are any time one piece of code has to know about another piece of code.
In C++ this is usually any place you have to `#include` something.

> "It can be said that the bulk of software design is managing dependencies.
> Figuring out where to put code and cutting the dependencies so that dependencies don't run
> in strangely bizarre directions."
-- Bob Martin

**Coupling** is when modules depend on each other in undesirable ways.

---

So what's good about Object Oriented languages?

**Encapsulation**: Functionality and data related to a single topic can be contained within a class.
(Bob Martin argues that C++ ruined encapsulation by bringing in hacking concepts like `public` and `private`).

**Inheritance**: General functionality and data can be inherited in multiple places from a single class.

**Polymorphism**: Objects with a variety of types and implementation details can be treated as though they are a simpler interface.
Allows the compile-time dependency to point against the flow of control.
This is a powerful tool to combat rigidity and fragility through interfaces.


## S.O.L.I.D Principles of Design

- **S**: Single Reponsibility Principle

> Classes should have only a single responsibility to change.

You shouldn't have two methods in a class that change for wildly different reasons.

```cpp
class GameObject
{
public:
  bool CheckCollision();
  void DrawShapes();
  void TakeDamage(int damage);
};
```

If the way you handle collision changes, `CheckCollision` must be changed.
If there's a problem with your OpenGL code, `DrawShapes()` will change.
If you decide to implement an armor system that affects damage, `TakeDamage` will have to change.

This class violates the single responsibility principle.


- **O**: Open/Closed Principle

> "Modules should be open for extension, but closed for modification."
-- Bertrand Meyer

You should be able to change the behaviour of the module without changing the module.
This sounds like a paradox.
This depends on polymorphism.

```cpp
class Particle
{
public:
  virtual vec3 GetPosition() const = 0;
  virtual void SetPosition(const vec3 & p) = 0;
};

class ParticleAffector
{
public:
  virtual void AffectParticles(const vector<Particle *> & particles) = 0;
};

class ParticleSystem
{
public:
  void Update(const vector<ParticleAffector *> & affectors);
  void DrawParticles();
};
```

In this example, we can change the behaviour of our particle systems by creating new affectors -
objects that cause the particles to move in different ways.
Doing so requires that we write no additional code in `ParticleSystem.cpp`!

Note that this requires that our interfaces (e.g. `Particle`) have all the exposed functionality that we need for adding new affectors.
This isn't always easy.

It also doesn't always work.
What if we need to make it so that affectors of a certain kind take precedence over other affectors?
Or maybe the order that affectors are applied is important?


- **L**: Liskov Substitution Principle

> Derived classes must be usable through the base class interface,
> without the need for the user to know the difference.

```cpp
class Rectangle
{
public:
  void SetWidth(const double w);
  void SetHeight(const double h);
private:
  double width;
  double height;
};
```

```cpp
class Square : public Rectangle
{
public:
  void SetWidth(const double w);
  void SetHeight(const double h);
};

void Square::SetWidth(const double w)
{
  width = height = w;
}

void Square::SetHeight(const double h)
{
  width = height = h;
}
```

A square "is a" rectangle and that's what inheritance is all about, right?
So `Square` inherits from `Rectangle`.

But a user of the `Rectangle` class might not expect `height` to change when they call `SetWidth`.
Suddenly, they need to know about the derived class (`Square`) in order to know how the `Rectangle` interface is going to behave.

Classes represent real-world objects (some times), but they are not themselves the objects.
Just because squares are rectangles in the real world doesn't mean that our `Square` class needs to inherit from `Rectangle`.
Doing so makes our interface confusing, because a `Square` is not substituable for a `Rectangle`.
`Rectangle` and `Square` should both inherit from e.g. `Shape`, but not from each other.


- **I**: Interface Segregation Principle

> No client should be forced to depend on methods it does not use.

To implement the Open/Closed principle you might want to put as many possible things in your interfaces!
That way your module never has to change to provide new functionality for other members of the team.

But if your interfaces are cluttered, it's hard for other team members to know how to use your classes.
And, it increases the number of places in your code that might possibly be able to call a particular method.

This is dependency! This is coupling! This is bad.

Instead, you should have many smaller interfaces for specific purposes.


- **D**: Dependency Inversion Principle

**High-Level Policy** should not be polluted with **Low-Level Detail**.

> A. High-level modules should not depend on low-level modules. Both should depend on abstractions.
> B. Abstractions should not depend on details. Details should depend on abstractions.

Let's say you have character class, and you write some code to make the character jump whenever the user presses `space`.
You do this by setting the upward velocity of the player to some positive amount.
Later in the quarter, you realize that your hand-written physics code isn't sufficient for what you want to do,
so you decide to replace all the physics code with `bullet3`, the open source physics engine.

If the part of your code that maps input events to player actions has to change as a result of this,
you have violated teh dependency inversion principle.

If, instead of doing this:

```cpp
if (IsKeyDown[Key::Space])
{
  player->velocity.y = 3.0;
}
```

you had done this:

```cpp
if (IsKeyDown[Key::Space])
{
  player->collisionComponent->setUpwardVelocity(3.0);
}
```

less code would need to change.

