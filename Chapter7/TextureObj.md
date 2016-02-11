#Texture Objects

Texture objects are internal memory to OpenGL that hold texture data and paramaters. Essentially, it's memory on the GPU. Managing memory on the GPU is dangerous, because of this OpenGL will not give you direct access to it. Instead you cant rack this memory with a unique unsigned integer that acts as a handle. 

This concept should already be familiar. In our 2D OpenTK Framework, when you loaded a texture it returned an integer. You then used this integer to draw the texture onto screen, and eventually to unload the texture. OpenGL does the same thing.

There are two ways to generate new texture handles. You can either request a single one at a time, or if you know how many textures you will need ahead of time you can request them in one big batch.

```
// Generate a single texture
int GL.GenTexture();
// Generate multiple textures
void GL.GenTextures(int n, out int[] textures);
```

Generating a single texture handle is straight forward. When generating multiple, the function takes as it's first paramater the number of textures you want. As it's second paramater it takes an out qualifyed integer array that is large enough to hold all requested handles.

Before generating textures, you must enable texturing. This is what we've learned so far put to code:

```
// Enable Texturing
GL.Enable(EnableCap.Texture2D);
// Generate a texture handle
int handle = GL.GenTexture();
// ??? Profit?
```