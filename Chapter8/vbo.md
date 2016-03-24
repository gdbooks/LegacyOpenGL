  # Vertex Buffers
  
Vertex Arrays are awesome, they are leaps and bounds more performant than the immediate mode functions. But even vertex arrays have drawbacks. The biggest one is that vertex data lives in CPU memory. This means, that every frame when you call a function like ```GL.VertexPointer``` that memory has to be uploaded to the GPU.

This means not only are we paying extra performance to upload the memory every frame, for some time identical data exists in both CPU and GPU memory.

Vertex Buffer Objects fix this. A vertex buffer is simply memory that lives on the GPU, much like a texture. Using Vertex buffers is actually very similar to using textures. first you create a buffer, then you fill it with data. Now you are ready to render every frame. Once you're done, you tell OpenGL to delete the buffer.