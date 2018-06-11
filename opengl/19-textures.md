---
layout: page
active: lectures
title: "Lecture 19: Textures"
auto-title: true
---

<a href="https://docs.google.com/presentation/d/12p68PGV0_iGpp5iei6tqa0ChdOcJ7vV9Ep17kpbSQ4E/edit?usp=sharing" class="btn btn-info">
  Slide Deck From Class
</a>

[This is also a very good overview](https://learnopengl.com/#!Getting-started/Textures)


## How To Use Textures

Create a handle for the texture.

```cpp
GLuint TextureID;
```


Create and bind the texture.

```cpp
glGenTextures(1, &TextureID);
glBindTexture(GL_TEXTURE_2D, TextureID);
```


Set any parameters

```cpp
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);

glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
```


Upload image data.

```cpp
std::string const path = resourceDirectory + "/Image.jpg";
int width, height, channels;
unsigned char * data = stbi_load(path.c_str(), & width, & height, & channels, 0);
if (data)
{
	glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE, data);
	glGenerateMipmap(GL_TEXTURE_2D);
	stbi_image_free(data);
}
```


Now, in the render loop.
Active a texture slot and bind your texture to it.

```cpp
glActiveTexture(GL_TEXTURE0);
glBindTexture(GL_TEXTURE_2D, TextureID);
```

Specify a uniform that contains the **texture slot** you just bound the texture to (NOT the texture handle itself).

```cpp
glUniform1i(prog->getUniform("uTexture"), 0);
```


In the fragment shader (or vertex shader, if you want to), you can access the texture using the uniform we just bound.
It has type `sampler2D`, even though we used the integer uniform method in our C++ code.

```glsl
uniform sampler2D uTexture;

void main()
{
	vec4 color = texture(uTexture, fTexCoords);

	// ...
}
```
