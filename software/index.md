---
layout: page
active: software
title: "Software"
---


## C++

C++ is in some ways a combination of Java and C.
It has syntax and a lot of other features borrowed from C,
but also supports object-oriented programming similar to Java.

If you're not familiar with either of these languages,
you may want to learn one of those first.
C++ should probably not be your first language.
It is old, cumbersome, dangerous, and has some odd conventions.

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


# Chapters

- [14 - C++ 1](14-cpp-1)
- [23 - C++ 2](23-cpp-2)

- [1 - C++ (1)](01-cpp1)
- [5 - C++ (2)](05-cpp2)
- [21 - C++ (3)](21-cpp3)

- [C++ Snippets](cpp-snippets)

- [5 - Iterative Prototyping](05-iterative-prototyping)
- [6 - Git Branching Models](06-git-branching)
- [7 - Software Design](07-software-design)
- [8 - Object-Oriented Design](08-object-oriented-design)
- [9 - Data-Oriented Design](09-data-oriented-design)
- [10 - Game Programming Patterns](10-game-patterns)
