---
layout: page
active: lectures
title: "Lecture 4: Simulation Time Steps"
auto-title: true
---



## Frames

What is a **frame**?

```cpp
glDrawArrays(...);
glDrawArrays(...);
glDrawArrays(...);
glfwSwapBuffers(); // makes the rendered image visible to the user
```

At the beginning of a frame, we might take in some input from the user.

```cpp
bool playerMoving = KeyPressed[W];

if (playerMoving)
  characterPosition.x += 1.0;

DrawScene();
glfwSwapBuffers(); // makes the rendered image visible to the user
```

This is all happening in a loop, sometimes called the **render loop** or **game loop**.
Each pass through this loop is a frame.

What is a **framerate**?
The number of frames that we can render in a second.
So if a game is rendering at "60 FPS", that means that we are rendering 60 frames-per-second.

### Display Hardware

I say 60 because there is another rate/frequency we need to be concerned with here.
Most monitors have a fixed refresh rate of **60Hz**.
That means they continually display new images to the user at a rate of 60 times a second.

As such, it is commonly the goal of video game developers to make a game run at 60 frames-per-second.
We sometimes say we have a frame budget of 16.67 milliseconds.
If it takes longer than that to render a frame, our frame rate will fall below 60 and the user experience will be impacted.
But, depending on the display hardware, it is sometimes not worth it to be able to render at more than 60 frames per second.

If the hardware refresh rate is 60Hz and we produce multiple rendered frames in the 16 ms window,
then the top half of the first frame will appear at the top of the screen,
and the bottom half of the next frame will appear at the bottom of the screen.

This is referred to as **vertical tearing**.

### Vertical Sync

There is a commonly used setting to avoid this called **vertical synchronization** or **vsync**.
When vsync is enabled, the `glfwSwapBuffers()` function will not return until the display hardware has made the frame visible to the user.

So why is this method called "swap buffers" in the first place?
Graphics applications use **double-buffering** to render and display frames.
There are two screen buffers - a front buffer and a back buffer.
Whenever a draw call is issued, the results are rendered into the back buffer.
When `glfwSwapBuffers()` is called, the contents of the back buffer are copied\* into the front buffer.
The front buffer is then displayed by the graphics hardware.
This way, the

So what's actually happening when vsync is enabled is this:
When we call for a buffer swap, we cannot swap the back buffer with the front buffer
until the display hardware has shown the front buffer *at least once*.

\* This is a bit of a simplification.
But the actual mechanisms of display and graphics hardware are quite complicated,
and we don't need to understand it completely to move on.



## Fix your time step!

<!-- https://gafferongames.com/post/fix_your_timestep/ -->
<!-- http://lspiroengine.com/?p=378 -->
<!-- http://www.koonsolo.com/news/dewitters-gameloop/ -->

There's a problem with this line in the above example.

```cpp
  characterPosition.x += 1.0;
```

We usually want that character to move at a constant rate with respect to *real time*.
That is, we want it to always take, say, 10 seconds for the character to move from point A to point B.

But depending on our frame rate, this character will move at drastically different rates.
If vsync is enabled and graphics hardware can render frames quickly, the character will move 60 units per second.
If vsycn is not enabled, the character may move up to thousands of units per second.
And if the graphics hardware is old and can only render a few frames per second, the character will move agonizingly slow.

### Time-Based Movement

One thing we can do is calculate the time since the last frame and use that to determine how fast the character should move.

```cpp
double currentTime = clock();
double deltaTime = currentTime - lastTime;

bool playerMoving = KeyPressed[W];

if (playerMoving)
  characterPosition.x += 1.0 * deltaTime;

DrawScene();
glfwSwapBuffers(); // makes the rendered image visible to the user

lastTime = currentTime;
```

With this code, the player movement rate will be controlled by the framerate of the application.
If the application runs at a high framerate, the player will move in very small intervals.
If the application runs at a low framerate, the player will move in larger intervals.

This **time-based movement** will make it so that the game doesn't break catastrophically when the framerate is different from expected.

But there's a problem.
Let's say that we check the player for collision with a wall every frame.
This is simple enough to do:

```cpp
if (playerMoving)
{
  characterPosition.x += 1.0 * deltaTime;
  if (characterHitsWall())
    characterPosition.x -= 1.0 * deltaTime;
}
```

If we hit the wall, just move the player back and undo their movement (they can't walk through a wall).
This code will work perfectly fine when the framerate is high, because the player will move a tiny amount each frame
and we'll know immediately if the player hits the wall.
However, let's say the graphics hardware is really struggling and the game is rendering at two frames a second.
If the player moves at a rate higher than two times its width, it will be possible for it to phase through the wall.
This will occur because at one time step, the player is on one side of the wall, and after the next (large) movement update,
they are completely on the other side of the wall.

This is an example of a big problem with time-based movement,
but in general it's a bad idea to have the rate of your game updates affected by the framerate.
It can make your game look and feel slightly different depending on where it is played.

### Semi-Fixed Time Step

So what can we do?

We need our game world to update based on how much time has actually elapsed,
but we also don't want a variable time step for those updates.

What we need to do is **decouple our render framerate from our game update framerate**.

Every time we go through the game loop, we decide *how many* game updates need to take place.
So if our game updates should happen every 10ms and there has been 44ms since the last frame,
we do four game updates.

```cpp
double currentTime = clock();
double deltaTime = currentTime - lastTime;

while (deltaTime > 0.01)
{
  GameUpdate(0.01);
  deltaTime -= 0.01;
}
```

What about those extra 4ms?
Did we lose them?
There's two options:

First, we can just do an extra small game update.

```cpp
while (deltaTime > 0)
{
  float updateTime = min(deltaTime, 0.01);
  GameUpdate(updateTime);
  deltaTime -= updateTime;
}
```

But this brings in the problem that some updates are larger than others.
They're all small, so there won't be big and glaring issues like the wall collision problem,
but for a variety of reasons this might not be desirable.

### Fixed Time Step

The other option is to use an accumulator:

```cpp
double updateAccumulator = 0;
```

```cpp
updateAccumulator += deltaTime;
while (updateAccumulator > 0.01)
{
  GameUpdate(0.01);
  updateAccumulator -= 0.01;
}
```

The nice thing about this setup is that the game updates are **deterministic**.
They will always happen the same way.
If we do variable length game updates, there might be some bugs that only happen when the framerate is very high.
This can be very frustrating to debug.

### Spiral of Death

There is one major problem with both of the above techniques.
If a game updated is supposed to happen every 10ms but there are so many characters being updated that it takes 20ms,
then our game will not be able to keep up.
Every time we do an (expensive) update, we will have more and more updates to make up.
The framerate will drop and the game will be unplayable in short order.

There are a couple of ways to address this.
The first is to simply never allow your game to have so much to do in an update that it takes longer than the expected update time.
Obviously this can be easier said than done.
The other option is set a maximum number of game updates that can happen each frame.
This will have the effect of slowing down time when the game can't keep up,
but it is arguably much preferred over the alternative of having our game grind to a halt.

### Interpolation

With a fixed time step, we will be rendering frames with duplicated contents if the rendering framerate is higher than the game framerate.
We also have a little bit of leftover time in the accumulator after every series of game updates.
What we can do is use this leftover time to interpolate between the current state and the next state.
However, we don't know what the next state will be.
So instead, we can interpolate between the last state and the current state.

This will result in some latency between input and visualization.
Exactly one frame's worth.
But, it makes it so that our game will always look smooth even with very high framerate.
Going forward this is becoming more important since screens with 120Hz and 144Hz refresh rates are becoming more prevalent.

And don't forget that human reaction time is 250ms for visual stimulus.



## References

- https://gafferongames.com/post/fix_your_timestep/
- http://lspiroengine.com/?p=378
- http://www.koonsolo.com/news/dewitters-gameloop/
