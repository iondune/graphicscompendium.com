---
layout: page
active: lectures
title: "Lecture 11: Introduction to Animation"
auto-title: true
---


<a href="https://docs.google.com/presentation/d/1kpt6oyzqrPBA2e3MvSqC-ioQsuWYg2DdfmdEDiTNNt8/edit?usp=sharing" class="btn btn-info">Slide Deck</a>



How do we animate?

Modifying scene parameters as a function of time.

## Scripting

Specifiying the parameters at every frame.

```python
define spinningCube()
  rotAngle = pi*frameNumber / 50
```

```python
define carScript()
   carTranslation = 10*(frameNumber / 100)
   wheelRotation  = pi*frameNumber / 5
```

### Hierarchical Modeling

<div id="example1">
  <iframe id="exampleFrame1" src="11-example-hierarchical.html" width="820px" height="420px"></iframe>
</div>

This is the order you typically apply rotations:

$$ T * R * S $$

Why?

* Non-uniform scale will mess up rotations
* Translation will mess up everything

In a hierarchical model, generally do T and R, then `push()` for S, draw, and `pop()` before continuing on.
Unless your models are positioned with the origin at where you want it to pivot, you typically also need to do a translate after the rotate but before the scale (because translates mess up everything).

Here's the (simplified) javascript code for the above example.


```js
push(); // upper-arm rectangle (red)

  rotate(upperArmRotation);
  drawAnchor();
  push();
    translate(100, 0);
    drawRect(200, 100); // box
  pop();

  push(); // lower-arm rectangle (green)

    translate(200, 0);
    rotate(lowerArmRotation);
    drawAnchor();
    push();
      translate(100, 0);
      drawRect(200, 80); // box
    pop();

    push(); // hand rectangle (blue)

      translate(200, 0);
      rotate(handRotation);
      drawAnchor();
      push();
        translate(75, 0);
        drawRect(150, 60); // box
      pop();

    pop();

  pop();

pop();
```

The boxes we draw are *centered* at the origin, so we need to translate out $$1/2$$ the width to get the pivots/anchors we want.


## Key-Framing

**Key-framing** means defining specific animation frames and interpolating between them to get frame-by-frame animation poses.
Typically an artist creates the keyframes.

But how do we interpolate the poses?
Can we just use linear interpolation?

For poses $$ p_0 $$ and $$ p_1 $$ and $$ t $$, which goes from 0 for pose 0 and to 1 for pose 1.

$$ p = p_0 * (1 - t) + p_1 * t $$

- For positions? Absolutely?
- Scales? Yes.
- Rotations? Nope.


---


## Outline

1. Forward Kinematics/Key framing
1. Inverse Kinematics
1. Motion capture
1. Simulation
  1. Behavioral Animation
  1. Physically based (Dynamics)



- Key framing
- Interpolation
- Rotations
  - Euler angles
  - Quaternions
- Splines
- Skinning
- Kinematics
- Inverse kinematics
- Motion capture
- Collision detection
  - Spatial data structures
- Principles of animation
  - Squash and stretch
  - Timing and motion
  - Anticipation
  - Follow through
  - Arcs
  - Ease in/out
