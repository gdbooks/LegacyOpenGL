# Blending

OpenGL allows you to blend incoming framents with pixels already on screen. Which enables you to introduce effects such as transparency into your scenes. With transparency you can simulate water, windows, glass and other objects in the world you can normally see trough.

The term **fragment** might be new to you, but it will come up several times from here on out. This is a good time to discuss what it is.

As OpenGL processes the primitives you pass to it, during the rasterization stage it breaks them down into pixel size chunks called fragments. Sometimes the terms pixel and fragment are used interchangeably, but there is a subtle difference. A pixel is a value that gets written to the color buffer. A fragment is a piece of a primitive that _might_ eventually become a pixel after it is depth-tested, alpha-tested, blended, combined with a texture,  combined with another fragment or it may just become a pixel without being modified.

So, essentially a fragment is data that can become a pixel, but isn't necesarley a pixel yet.

Remember the alpha value we've been ignoring all this time? Well, now that we're talking about blending it's time to learn how to use it. When enabling blending, you are telling OpenGL to combine the color of the incoming primitive with the color that is already in the frame buffer, and store that combined color back into the frame buffer.

Blending operations are typically specified with the RGB values representing color, and the Alpha value representing opacity. But other combinations are possible. From now on,we will refer to the incomming fragment as SOURCE, and the pixel that is already in the frame buffer as DESTINATION.

To enable blending, call the ```GL.Enable``` method with ```EnableCaps.Blend``` as it's argument.