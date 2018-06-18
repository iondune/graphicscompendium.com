---
layout: page
active: lectures
title: "Lecture 7: Software Design"
auto-title: true
---


<a href="https://docs.google.com/presentation/d/1DRA5PmtHwlgXofexA2OzfP0Q0F3m8wSKMeOsvur0hF8/edit?usp=sharing" class="btn btn-info">Slide Deck</a>


One of the central ideas of this class is that we don't have enough time.
Games are complicated to make, so we're trying to keep our ideas as simple as possible,
and we're preparing to spend a lot of time coding.

When we're trying to move as fast as possible, it might seem like anything non-essential that takes up time should be ignored, in particular:

- Design Patterns and anything else that makes our code cleaner but takes longer to write
- Documentation
- Testing

However, these are things that definitely waste time:

- **Wasted effort** on features that end up needing to change or be removed to allow for other features.
  Planning ahead can only go so far in addressing this.
  Designing code to be as modular as possible can make it easier to pivot.

- **Difficulty** in adding new features and changing how old features work, due to interconnected dependencies within the codebase.
  Design patterns can be used to address this.

- **Uncertainty** within the team about what is done, what needs to be done, and how things should be done.
  Documentation can be used to address this.

- **Frustration** at trying to solve a bug that is difficult to reproduce, and which could be caused by any number of components.
  Unit tests can be used to address this.


## Bad Code

How many of you have been slowed down by bad code?

Why do we write bad code?

Some people say that bad code comes from a lack of discipline.
We're rushing, we need to meet deadlines, we have other things to do.
So we purposefully write bad code just to get the job done.

I think that happens sometimes, but that it's not the main problem.
Writing good code from scratch is incredibly difficult.
Usually when we talk about good code, we're talking about structure -
we have appropriate classes, effective interfaces, the code is all terse but readable.
But code is bad both when it is unstructured (just a huge pile of loops in a single `main.cpp`)
as well as when it has the wrong structure, or too much structure.

So we need to learn how to add the right structure.
How do we do this?

This is my approach:

- Absorb as much information about good software design as you can.
  Learn about OOP, DOD, Functional Programming - and distill the relevant concepts into your code.

- Make refactoring a consistent and essential part of your development process.
  Always write code with the intention of cleaning and organizing it once the functionality falls into place.


