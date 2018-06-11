---
layout: page
active: lectures
title: "Lecture 17: Screen-space Ambient Occlusion"
auto-title: true
---


<a href="https://docs.google.com/presentation/d/1sjwh0nepIW6YCHtAqfGzrOtYYwjRZLBsmlPCJh70m1c/edit?usp=sharing" class="btn btn-info">Slide Deck</a>

Notes
-----

### Sample Bias

The middle box shows a set of random points generated with equal distribution across the square region.

The right box shows these points normalized onto the unit circle in black.
In red, we show these points scaled to the distribution used in our SSAO implementation.

On the left box, we have the distribution of points colorized for directions.
Green directions have few points and red directions have many points.

<div id="example1">
  <iframe id="exampleFrame1" src="17-example-sample-bias.html" width="1020px" height="420px"></iframe>
</div>


Resources:
----------

* [Learn OpenGL: SSAO](https://learnopengl.com/Advanced-Lighting/SSAO) -
  A good place to start!
* [John Chapman: SSAO Tutorianl](http://john-chapman-graphics.blogspot.nl/2013/01/ssao-tutorial.html) -
  This is the basis for the Learn OpenGL tutorial, it is a little more advanced but describes the same technique with perhaps
  a little bit more technical accuracy.
* [SSAO With Depth Reconstruction](http://ogldev.atspace.co.uk/www/tutorial46/tutorial46.html) -
  How to reconstruct view space values from depth buffer. I haven't used this exact technique yet, but I have a more expensive approach
  that works as well.
* [ionEngine SSAO Sample](https://github.com/iondune/ionEngineSamples/tree/master/SSAO) -
  My sample code.
* [Comparative Study of SSAO Techniques](http://frederikaalund.com/a-comparative-study-of-screen-space-ambient-occlusion-methods/) -
  Advanced but a good look at the different approaches used.

