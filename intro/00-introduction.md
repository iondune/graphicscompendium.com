---
layout: page
active: lectures
title: "Chapter 0: Introduction"
auto-title: true
---


<a href="https://docs.google.com/presentation/d/1D_htUiKc0UhsAPWfNC6GqLFaIGWDT9Q0uGryFbODB9s/edit?usp=sharing" class="btn btn-info">Slide Deck</a>
<a href="https://docs.google.com/presentation/d/1-M4FVo_CUWjC2PGK8w-ykNfbrTZ7kw8ux8bAO1AVE44/edit?usp=sharing" class="btn btn-info">Slide Deck - Dr. Sueda</a>

Throughout this entire book we will be using C++.
C++ is the primary language used for computer graphics.
If you're already familiar with C or Java, you likely know enough to be able to do some basic graphics programming.
To learn more about C++, you may want to read some of the chapters in the C++ part of this book.

Eventually, we are going to cover a variety of topics related to programming in OpenGL.
OpenGL is a graphics API that allows you to use the dedicated graphics hardware in your computer.
You can read more about OpenGL in the second part of this book -
first, we are going to cover some fundamentals of graphics.
In particular, we're going to cover everything necessary to write a software rasterizer.
This makes it possible to study graphics in a more pure form, before diving into OpenGL programming,
which carries a lot of OpenGL-related baggage.
However, everything we cover in this part is relevant to OpenGL programming.

Before diving into graphics, you may want to take this fun quiz:

[Quiz Time!](http://area.autodesk.com/fakeorfoto/)

It asks you to guess whether or not a given image is a photograph or a computer generated image.
It's a fun exercise in seeing how realistic modern computer graphics is, and it also highlights
one of the fundamental purposes of computer graphics: to create images that look like photographs.
It is not always the case that we use computer graphics to make things that look like photographs,
but it is common, so a lot of ideas and metaphors from photography leak in.
For example, we'll often discuss the idea of a "virtual camera" that we implement and simulate.



## GPU vs. CPU

Nearly all computers have at least two processors.
You may already be familiar with the CPU, or central processing-unit,
which is responsible for carrying out most of the tasks performed in software.
However, most modern computers have an additional processor (sometimes called a co-processor),
the sole purpose of which is to perform graphics computations.

This processor is the GPU, or graphics processing-unit.
At its most basic level, the GPU is responsible for determining the color of pixels.
It also is often involved in the process of displaying pixels on display hardware (e.g. a monitor or screen).
If you're reading this on a screen, it's very likely that a GPU is involved in the process of rendering the
text and making those pixels visible to you.

However, you have to take some special steps in order to write programs that run on the GPU.
If you write a program in Python, C, Java, or whatever, then compile and run it, you're using the CPU.
One way to make programs that utilize the GPU is by using OpenGL.
However, as we will see, programs that run on a GPU are much more structured that CPU programs.
Understanding the functionality that GPUs are designed for is essential to being able to write GPU programs.

A GPU is designed to be a hardware rasterizer.
That is, it's a processor that's designed to be very fast at the process of rasterization.
So before we dive into GPU programming, we're first going to write a rasterizer.
Such a rasterizer (one that runs on the CPU) is sometimes called a software rasterizer,
since we are writing software to perform the rasterization process.

Such naming is a little bit odd, though, since in both the "hardware rasterization"
and "software rasterization" process, a rasterization algorithm is being executed on processing hardware.
The difference is whether that hardware is a GPU or a CPU.
But it's common usage and it does make sense in some respects, so we're going to stick to it.

If you're wondering a little bit more about what exactly a GPU is,
you can think of it is a processor that has a lot of cores which are not very fast.

<img src="00-figure-cpu-vs-gpu.png" alt="cpu vs gpu" class="img-thumbnail" />

While a fast, modern CPU might have four cores that run at, say, 2.5GHz,
a modern GPU might have 1500 cores that run at 750MHz.

A GPU is also a vector processor.
On a CPU, typically each instruction refers to one or two registers.
If your CPU runs an `add` instruction, a single register is added to another.
On a GPU, each instruction applies to multiple variables.
So each `add` instruction might add 16 numbers to 16 other numbers.
This type of instruction is called **SIMD**, which means
Same Instruction, Multiple Data.
SIMD instructions are also sometimes supported on CPUs,
but it's what GPUs are designed for.

There is of course a lot more to GPU hardware than described here,
but that should give you a basic idea of what we'll be working with.


## In This Part

The purpose of this part is to learn how to write a software rasterizer.
Rasterization is the process of converting a primitive shape (such as a point, line, or triangle),
into pixels that can show up on the screen.
To understand how rasterization works, we'll cover some geometry concepts
(related to triangles) and some linear algebra (vectors and matrices).
