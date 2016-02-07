## Blending and Fog

Up to this point we've used RGBA for a lot of things. Every material color has an alpha component. Even lights have an alpha component! Yet, setting this to 0.5 does not add alpha to our scene at all.

OpenGL let's you actually use the Alpha component to draw translusent and transparent primites trough a process known as blending. 

In addition to blending, OpenGL also offers fog. Fog is great to fade objects in the distance out. If something is at the end of your view frustum, it's better to fade it out and make your world look like it's in a fog than it is to have a hard cut-off point that shows players that they are viewing the world trough a limited window.

We're going to explore both of these concepts.