---
layout: chapter
file: 00-gamedev
---


<a href="https://docs.google.com/presentation/d/1RGD4st6F9-GlvgijIMWyA3J9Rdj8T_6ikbtW7QPRIf4/edit?usp=sharing" class="btn btn-primary">Slide Deck</a>

## Games Are Difficult

Why?

There are a lot of technologies to put together:

- Graphics
- Animation
- User interface design
- Sounds/audio
- Artificial intelligence

Putting everything together involves some difficulties:

- Timing and sequencing: need to keep track of what things are happening and when
- Performance: there's a lot going on and it needs to be fast to be playable

Game Designer Liz England has a great motivating example for describing the difficulties of game design,
which she refers to as "The Door Problem".
It's a quick read that I highly recommend.

[The Door Problem](http://www.lizengland.com/blog/2014/04/the-door-problem/)

We can expand the door problem to talk about some of the problems a door poses
from a game *development* perspective.

- You need to find a mesh for the door
- It needs to be transformed in such a way that it can rotate about its hinge
- How do you open the door? Need to tie a key press event to the door opening
- Do you need to be close to it or do you need to look at it? Maybe we need picking
- Can you open a door when you're dead? Okay, we need to filter door controls
- What happens if the character is in the doorway and the door is closed?
  - Is the character pushed out of the way, or is the door stopped?
- Is there a sound effect associated with the door?
- Are AI characters aware the door is open/closed?
  - It's a piece of the scene that can sometimes be seen through, sometimes not
  - This poses interesting problems
