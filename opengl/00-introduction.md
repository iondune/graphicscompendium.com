---
layout: page
active: lectures
title: "Lecture 0: Introduction"
auto-title: true
---


<a href="https://docs.google.com/presentation/d/1D_htUiKc0UhsAPWfNC6GqLFaIGWDT9Q0uGryFbODB9s/edit?usp=sharing" class="btn btn-info">Slide Deck</a>
<a href="https://docs.google.com/presentation/d/1-M4FVo_CUWjC2PGK8w-ykNfbrTZ7kw8ux8bAO1AVE44/edit?usp=sharing" class="btn btn-info">Slide Deck - Dr. Sueda</a>


## About The Class

- Syllabus
  - GradeScope
- Website overview
- Communication and questions
- GitHub Classroom
  - [Git usage]({{ "/references/git" | prepend: site.baseurl }})
- Deadlines and late penalties

Subject Matter

- We will be learning and using C++
- We will be learning and using OpenGL
- We will be using Git to turn things in

Questions:

- Most comfortable programming language?
- Which class would you rather take a math class or drawing class?
- Why are you taking computer graphics?
  - Game development (game industry)
  - Animation (cg industry)
  - Offline rendering (cg industry)
  - Research (academic graphics)

[Quiz Time!](http://area.autodesk.com/fakeorfoto/)



## GPU vs. CPU

<img src="00-figure-cpu-vs-gpu.png" alt="cpu vs gpu" class="img-thumbnail" />



## C++

C++ is in some ways a combination of Java and C.
It has syntax and a lot of other features borrowed from C, but also supports object-oriented programming similar to Java.
One interesting aspect of C++ programming is that it is (typically, for most use cases and with a few small syntax changes) backwards compatibile with C.
You can, for all intents and purposes, write a program in C and use a C++ compiler to compile it.

Perhaps one of the most notorious things about C++ (or at least one of the biggest problems the language currently faces) is building,
especially when it comes to using and compiling with libraries.

Just like C, C++ can be compiled using Makefiles.
But usually, we'll use another program to automatically generate those Makefiles, called [CMake](https://cmake.org/).

This is a bit odd, because we've got quite a chain in order to compile our source code.

First we use **CMake** to generate a Makefile.
Then, we use **Make** to execute this make file, in turn calling **g++** or another actual compiler to compile the code.

If we opt to use and IDE instead of compiling on the commandline, we can still use **CMake**.
That's one of the reasons its so commonly used for C++ compilation.

* On OS X, CMake can generate XCode project files for us.
* On Windows, CMake can generate Visual Studio project files for us.

CMake can actually generate a ton of different build files,
so chances are good that if you are using some other IDE it will still be supported.

However, I am an avid Visual Studio user and I kind of don't care for using CMake to build my projects.
So in addition to CMake configuration, I also include ready-built Visual Studio project files for all base codes.
So as long as you are using the latest version of Visual Studio (2017), you don't need to bother with CMake - you can just use my provided files.



## Assignments

- Labs
  - Smaller assignments, checked off in lab
  - Sometimes build pieces for assignments
- Programs
  - Larger assignments, turned in via GitHub Classroom



## Units

- Introduction
  - Build a software rasterizer
  - Start talking about transformations
  - Start talking about geometry and representations
- OpenGL
  - How to use the API
  - The OpenGL Graphics Pipeline
- Transforms
  - More transforms??
  - Hierarchical modeling
- Lighting
  - Shading and lighting
  - Reflection models
  - Color theory
- Camera & Wrap-up
  - Camera controls
  - Scene setup
  - A brief look at common graphics techniques

