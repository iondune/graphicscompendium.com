---
layout: page
active: lectures
title: "Lecture ??: Homegenous Divide"
auto-title: true
---


## Outline

Planned lecture on the topic of the homogenous divide including:

This could also make more sense in 476.

- Why it's necessary to have the w at all for perspective (can't we just divide by z?)
  - i.e. the difference between w and z in clip space
- Mathematics behind it (projected coordinate systems)
- History behind left hand and right handed use in graphics (explanation of why OpenGL uses left-handed in clip space?)
- Possibility of using a projection matrix with inifinite far
- Precision issues in the depth buffer and ways to fix it
  - Using the reversed "DirectX" mapping (0 to 1)
  - Real logarithmic depth buffer using `gl_FragDepth` writes


## References

- [http://deltaorange.com/2012/03/08/the-truth-behind-homogenous-coordinates/](http://deltaorange.com/2012/03/08/the-truth-behind-homogenous-coordinates/)
- [https://stackoverflow.com/questions/4124041/is-opengl-coordinate-system-left-handed-or-right-handed](https://stackoverflow.com/questions/4124041/is-opengl-coordinate-system-left-handed-or-right-handed)
- [https://softwareengineering.stackexchange.com/questions/17519/why-does-directx-use-a-left-handed-coordinate-system/88776#88776](https://softwareengineering.stackexchange.com/questions/17519/why-does-directx-use-a-left-handed-coordinate-system/88776#88776)
- [http://www.songho.ca/math/homogeneous/homogeneous.html](http://www.songho.ca/math/homogeneous/homogeneous.html)
- [http://www.terathon.com/gdc07_lengyel.pdf](http://www.terathon.com/gdc07_lengyel.pdf)
- [http://www.songho.ca/opengl/gl_transform.html#projection](http://www.songho.ca/opengl/gl_transform.html#projection)
- [http://www.songho.ca/opengl/gl_projectionmatrix.html](http://www.songho.ca/opengl/gl_projectionmatrix.html)
- [https://www.scratchapixel.com/lessons/3d-basic-rendering/perspective-and-orthographic-projection-matrix/opengl-perspective-projection-matrix](https://www.scratchapixel.com/lessons/3d-basic-rendering/perspective-and-orthographic-projection-matrix/opengl-perspective-projection-matrix)
- [https://stackoverflow.com/questions/31686664/perspective-divide-why-use-the-w-component](https://stackoverflow.com/questions/31686664/perspective-divide-why-use-the-w-component)
- [https://en.wikipedia.org/wiki/Z-buffering#W-buffer](https://en.wikipedia.org/wiki/Z-buffering#W-buffer)

