#Using a texture map

As mentioned, textures are images that you apply to polygons. These images are either loaded from a file, or created in code. Once you have a texture in memory, you need to map it to an OpenGL texture object. But before you can do ANY of this, you must first enable texture mapping!

To enable / disable texture mapping you will most often use this call:

```
GL.Enable(EnableCap.Texture2D);
```
Other acceptable arguments are:
* EnableCap.Texture1D
* EnableCap.Texture2D
* EnableCap.Texture3D
* EnableCap.TextureCubeMap

You must enable the type of texture you want to use. More often than not that is going to be ```Texture2D```. It's important that you only enable 1 texture type at a time. If you have ```Texture1D``` AND ```Texture2D``` enabled the render operation might or might not work. It's undefined. You must disable the old texturing before using the new. For example:

```
GL.Enable(EnableCap.Texture2D);
// Draw textured objects
GL.Disable(EnableCap.Texture2D);

GL.Enable(EnableCap.TextureCubeMap);
// Draw reflective objects
GL.Diable(EnableCap.TextureCubeMap);
```

This brings up the next point. When you have texturing enabled, all color info will come from the texture. So, what if you want to draw a textured polygon, but still render an untextured grid? You just disable the texture after you drew the polygon! You could also disable texturing before you draw the grid!

```
// Draw the grid unlit and untextured
GL.Disable(EnableCap.Texture2D);
GL.Disable(EnableCap.Lighting);
grid.Render();
// Now that the grid is drawn, re-enable lighting and texturing!
GL.Enable(EnableCap.Lighting);
GL.Enable(EnableCap.Texture2D);
```