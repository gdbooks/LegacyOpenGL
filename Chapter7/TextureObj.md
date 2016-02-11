#Texture Objects

Texture objects are internal memory to OpenGL that hold texture data and paramaters. Essentially, it's memory on the GPU. Managing memory on the GPU is dangerous, because of this OpenGL will not give you direct access to it. Instead you cant rack this memory with a unique unsigned integer that acts as a handle. 

This concept should already be familiar. In our 2D OpenTK Framework, when you loaded a texture it returned an integer. You then used this integer to draw the texture onto screen, and eventually to unload the texture. OpenGL does the same thing.

You get get a new texture handle with: