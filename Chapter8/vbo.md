  # Vertex Buffers
  
Vertex Arrays are awesome, they are leaps and bounds more performant than the immediate mode functions. But even vertex arrays have drawbacks. The biggest one is that vertex data lives in CPU memory. This means, that every frame when you call a function like ```GL.VertexPointer``` that memory has to be uploaded to the GPU.

This means not only are we paying extra performance to upload the memory every frame, for some time identical data exists in both CPU and GPU memory.

Vertex Buffer Objects fix this. A vertex buffer is simply memory that lives on the GPU, much like a texture. Using Vertex buffers is actually very similar to using textures. first you create a buffer, then you fill it with data. Now you are ready to render every frame. Once you're done, you tell OpenGL to delete the buffer.

## Code Re-Use

The interesting thing about vertex buffers is that they re-use 90% of the same code that array buffers use. So, even tough we have a buffer filled with data, we still need to enable vertex arrays; we just tell OpenGL that instead of using an actual array for the vertex array (VA) data, we want to use a vertex buffer object (VBO).

This does get a little confusing at times. Because both techniques use the same function calls, using a VBO veruses using a VA comes down to weather or not an active buffer is bound.

If an active buffer is bound, OpenGL assumes you want to use Vertex Buffers, otherwise it assumes you want Vertex arrays. This will become clear in the next section.

## Sample

After having read trough the general guide, if the process still doesn't make sense, there is a decent [VBO Example](http://www.opentk.com/node/2302) on the OpenTK website.