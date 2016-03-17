# Optimization Techniques

At this point you should be fairly comfortable with OpenGL. We've gone trough and done most of the things you would noramlly do on the job. But a lot of times we skipped best practices, especially in terms of performance. This made the code easier to read, but more costly to execute.

This section will focus on two things. How to optimize your rendering code to render more things faster, and how to load and manage resources; so you can render more interesting things. 

We're going to learn how to manage textures, meshes and other objects so that they load and unload automatically, without us having to do much work to keep track of them. We're also going to learn how to parse 3D models from .obj files to load more interesting looking geometry.