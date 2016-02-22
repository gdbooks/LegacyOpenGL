# Texture Coordinates

Now that we can load and unload textures it's finally time to texture something! In order to apply a texture to a 3D model, you must first specify some texture coordinates for each vertex of that model. So far we've given a vertex a position, a color, a normal; now we add texture coordinates to the list of attributes a vertex can have.

Texture coordinates have some special vocabulary. We call the texture coordinates __UV coordinates__. This is because a texture is not read on an X-Y axis like you might expect, rather a texture is read using a U-V axis. The U axis is horizontal, the V axis is vertical. 0, 0 is in the bottom left. When we use subscript notation, U comes first: (U, V)

The other thing to know about U-V space is that it is normalized. It doesn't matter if a texture is 124x124, 256x256, 512x1024 or whatever. The U-V coordinate space always extends from 0 to 1. The center pixel of an image is always at point (0.5f, 0.5f).

In this image, the left side shows a png picture, the right side shows the picture being applyed onto a triangle. The triangle is also outlined on top of the image. The UV coordinates of this triangles vertices are (0, 0), (1, 1), (1, 0). 

![UV](gl_uv.png)