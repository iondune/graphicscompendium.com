---
layout: page
active: lectures
title: "Lecture 9: OpenGL Introduction"
auto-title: true
---


## The OpenGL Pipeline

### Input Specification

Upload and describe the data (i.e. the mesh, an indexed face-set) to OpenGL.

Create a **vertex array**, **vertex buffers**, and an **index buffer**.

### Vertex Shader

Operate on individual vertices.

Most common and important task is to move from object/world space to screen coordinates, e.g. from the camera's perspective.

This part is written in **GLSL**.

### Rasterization

Your triangles are turned into **fragments**.
A fragment is a pixel with more information associated with it (e.g. depth, other data).
This part happens behind-the-scenes in OpenGL.

### Fragment Shader

Operate on individual fragments.

What color is each pixel?

This part is written in **GLSL**.

### Testing and Blending

Which pixels are visible?

This happens behind-the-scenes in OpenGL, but you are responsible for some configuration.



## GLSL

GLSL is the Open**GL** **S**hader **L**anguage.

It is a c-like language that is designed for writing **shaders** to be used with OpenGL.

It contains built in math functions and useful math types, in particular vectors and matrices.

In fact, it is so convenient to work with vectors and matrices in GLSL that nowadays we use C++ libraries that try to mimic the way GLSL math works.

You will write both **vertex shaders** and **fragment shaders** in GLSL, but they will do different things.
There are other types of shaders but we will learn about those later.

### Overview

A GLSL shader consists of a list of inputs, a list of outputs, and a `void main()` function.


Here is an example vertex shader:

```glsl
in vec2 vPosition;
in vec3 vColor;

out vec3 fColor;

void main()
{
	gl_Position = vec4(vPosition, 0.0, 0.0);
}
```

Here is an example fragment shader:

```glsl
in vec3 fColor;

out vec4 outColor;

void main()
{
	outColor = vec3(fColor, 1.0);
}
```


## How to Draw in OpenGL

**1.** Compile and link your vertex and fragment shaders.


```cpp
// Compile vertex shader
GLint Compiled;
GLuint VertexShader = glCreateShader(GL_VERTEX_SHADER);
glShaderSource(VertexShader, 1, & VertexShaderSource, NULL);
glCompileShader(VertexShader);
glGetShaderiv(VertexShader, GL_COMPILE_STATUS, & Compiled);
if (! Compiled)
{
	std::cerr << "Failed to compile vertex shader!" << std::endl;
	PrintShaderInfoLog(VertexShader);
}

// Compile fragment shader
GLuint FragmentShader = glCreateShader(GL_FRAGMENT_SHADER);
glShaderSource(FragmentShader, 1, & FragmentShaderSource, NULL);
glCompileShader(FragmentShader);
glGetShaderiv(FragmentShader, GL_COMPILE_STATUS, & Compiled);
if (! Compiled)
{
	std::cerr << "Failed to compile fragment shader!" << std::endl;
	PrintShaderInfoLog(FragmentShader);
}

// Compile shader
GLuint ShaderProgram = glCreateProgram();
glAttachShader(ShaderProgram, VertexShader);
glAttachShader(ShaderProgram, FragmentShader);
glBindFragDataLocation(ShaderProgram, 0, "outColor");
glLinkProgram(ShaderProgram);
glUseProgram(ShaderProgram);
```

---

**2.** Create a VAO to contain input specification.

```cpp
GLuint VAO;
glGenVertexArrays(1, & VAO);
glBindVertexArray(VAO);
```


---

**3.** Create VBO(s) to contain vertex data.

```cpp
// Vertex data
GLfloat const Vertices[] = {
	0.0f,  0.5f,
	0.5f, -0.5f,
	-0.5f, -0.5f
};

GLuint VBO;
glGenBuffers(1, & VBO);
glBindBuffer(GL_ARRAY_BUFFER, VBO);
glBufferData(GL_ARRAY_BUFFER, sizeof(Vertices), Vertices, GL_STATIC_DRAW);
glBindBuffer(GL_ARRAY_BUFFER, 0);
```

---

**4.** Create an IBO to contain indices.

```cpp
GLuint const Indices[] = {
	0, 1, 2
};

GLuint IBO;
glGenBuffers(1, & IBO);
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, IBO);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(Indices), Indices, GL_STATIC_DRAW);
```

---

**5.** The VAO contains attributes. Attach the VBOs here.

```cpp
glBindBuffer(GL_ARRAY_BUFFER, VBO);

GLint PositionAttribute = glGetAttribLocation(ShaderProgram, "position");
glEnableVertexAttribArray(PositionAttribute);
glVertexAttribPointer(PositionAttribute, 2, GL_FLOAT, GL_FALSE, 0, 0);

glBindBuffer(GL_ARRAY_BUFFER, 0);
```

---

**6.** Bind an IBO to your VAO

```cpp
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, IBO);
```

---

**7.** Draw!

```cpp
glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
glDrawElements(GL_TRIANGLES, 3, GL_UNSIGNED_INT, 0);
```
