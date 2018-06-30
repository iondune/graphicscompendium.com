---
layout: page
active: lectures
title: "Lecture ??: DirectX Porting Guide"
auto-title: true
---


## Wind-Order

D3D11 uses the opposite wind order by default.
It also has backface culling enabled by default (unlike OpenGL).

However, you can rather easily switch D3D11 to use the same backface culling as OpenGL:

```cpp
D3D11_RASTERIZER_DESC RasterizerDesc = {};
RasterizerDesc.CullMode = D3D11_CULL_BACK;
RasterizerDesc.FrontCounterClockwise = true;
// ... other fields

ID3D11RasterizerState * RasterizerState = nullptr;
Device->CreateRasterizerState(& RasterizerDesc, & RasterizerState);
ImmediateContext->RSSetState(RasterizerState);
```

## Uniforms

Uniforms in D3D11 are grouped into structs that are uploaded as a single piece,
and the name for them is **Constant Buffers**.

Modern OpenGL has a similar feature called Uniform Buffers that can be optionally used.
The premise is that certain data (like view and projection matrices) is going to be consistent across many draw calls,
even if different shaders and whatnot are used.
Therefore there's no reason to upload it over and over.
Why not just bind that bundle of data once?

Typically the HLSL syntax for a constant buffer looks like this:

```cpp
cbuffer ConstantBuffer
{
  float4x4 uModelMatrix;
  float uCurrentTime;
};
```

Then in your C++ code, you create a constant buffer, fill the data for those two fields, and upload it in one piece.

Note that this doesn't create a `ConstantBuffer` scope for the variables, e.g. you just use
`uCurrentTime` in your HSLS code, not `ConstantBuffer.uCurrentTime`.
Consequently, you can't have variables with the same name in different constant buffers.

It's also possible to just outright declare the fields as global variables:

```cpp
float4x4 uModelMatrix;
float uCurrentTime;
```

However, on the C++ end this looks like two fields in an implicit constant buffer called `$Globals`.



## HLSL

HLSL is a lot like GLSL but with a few minor differences.
First off, instead of types like `vec3` you use `float3`.
A matrix is not `mat4`, it's `float4x4`.

The largest thing you'll need to change when switching from GLSL to HLSL is how you multiply matrices.
Using the `*` operator results in a component-wise multiplication - not at all what we typically want.
Instead you must use the `mul` operator.

```hlsl
output.Pos = mul(View, output.Pos);
```


## Input Layouts

In OpenGL, the layout of vertex data within vertex buffers is part of the Vertex Array Object.
In DirectX, it is stored in a special InputLayout object.
The odd part is that creating this object requires a pointer to the compiled shader code that uses this object.
This may seem to imply that an InputLayout must be created for every vertex buffer and shader pair,
but the input layout can be re-used with shaders that use the same input specification.

Providing the shader object to the InputLayout may be simple in some cases,
but if you're rewriting a graphics engine you may now have things set up such that it is easy to pass along the shader to the vertex buffer creation.
However, you can also simply compile a dummy shader for each VBO.
This is what I did for ionEngine.

### In Shader Code

In OpenGL shader code, you have `in` variables that take data from the previous stage of the graphics pipeline,
and `out` variables that you can write to (which turn into `in` variable for the next stage).

In HLSL, you need to group these into structs, and the semantics are slightly different.
Instead of a `void main()` for each shader stage, you use a function that takes your input struct as a parameter
and returns your output struct as a return value,
e.g.:

```hlsl
PS_INPUT vertexMain(VS_INPUT intput)
{
  // ...
}
```

These structs, when specifically used to transfer data between stages,
must also have new qualifiers called "semantics" attached to them.
Typically it looks something like this:

```hlsl
struct PS_INPUT
{
  float4 fPosition : SV_POSITION;
  float2 fTexCoords : TEXCOORD0;
  float3 fColor : COLOR0;
};
```

Some of the semantics are used by DirectX to impart specific meaning.
These "system value" semantics are prefixed by `SV_`.
The most common one is `SV_POSITION`, which is the equivalent of `gl_Position` in a vertex shader -
the way you communicate to the rasterizer what the clip space position of the vertex is.

For all other semantics, there are built in options that cover the gamut of what you typically use for shader inputs and outputs.
You've got `COLOR`, `TEXCOORD`, `POSITION`, etc.
These semantics are also postfixed with a number indicating the "semantic slot".
This combination of semantic name and semantic slot number is how you refer to the variables in your Input Layout specification.
This is a departure from OpenGL in which the variable name itself is commonly used when setting up the input layou specification,
or perhaps an explicit layout specification in the shader.

However, for non-system-value semantics, you also have the option of using "user-defined" semantics.
In practice this means that each element of the input layout really just has two names -
how it's referenced in HLSL code and how it's reference in the input layout.
And these names can be the same thing:

```hlsl
struct VS_INPUT
{
  float2 vPosition : vPosition;
  float2 vTexCoords : vTexCoord;
  float3 vColor : vColor;
};
```



## Shader Compilation

DirectX is set up to be used on Windows and using Visual Studio.
As such, they built shader compilation into the IDE.
The standard way to work with shaders is to add them to your Visual Studio project,
let the shaders be compiled during the regular build process producing compiled shader objects (`.cso` files),
then upload these to the GPU at run time.

However, this is a departure from the way things are done with OpenGL
(where we typically compile shaders at run-time)
and may also be an annoying limitation if you want to e.g. dynamically construct an ubershader
(or build a dummy shader for Input Layout creation).
Thankfully it is also possible to compile shaders at run-time using the `<D3DCompiler.h>` interface.


