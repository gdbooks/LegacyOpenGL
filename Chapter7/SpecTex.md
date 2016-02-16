#Specifying Textures
Now that a texture object has been created on the GPU and it's associated handle has been bound as the active texture, it's time to actually fill that texture object with data.

It's important to make the distinction between GPU memory and CPU memory. You have control of CPU memory, but not GPU memory. Rendering is done mostly using GPU memory. Therefore, to render a texture, you must upload it to GPU memory.

The process of getting a texture into GPU memory is fairly straight forward. 

* First, you must have the texture object you want to fill with data bound. 
* Read the texture data into CPU memory.
* Upload to the GPU with ```GL.TexImage2D```
* At this point, you can manually delete CPU memory
    * Or let the garbage collector do it, depends on how you loaded the texture

The ```GL.TexImage2D``` function is fairly straight forward, but it does have a lot of arguments

```
            (TextureTarget.Texture2D, 0, PixelInternalFormat.Rgba, bmp_data.Width, bmp_data.Height, 0, OpenTK.Graphics.OpenGL.PixelFormat.Bgra, PixelType.UnsignedByte, bmp_data.Scan0);

```

Explain arguments

So, how do we get that array of bytes that represents the texture?

## So far


## Other loading methods
The method we used to load textures here is by far the simplest. It relies on Windows to decode texture files for us. Sometimes, this isn't an option tough. For example, on an iPhone. So, how can we decode textures on non-windows platforms?

A common method is to use [stb_image](https://github.com/nothings/stb) or [LodePng](http://lodev.org/lodepng/). Both are C libraries that you have to compile into a .dll file and link against. Once compiled, you can use C#'s interop features to access the C functions.

If that sounds like a lot of work just to load a texture, well that's because it is! Another way to get texture loading to work is to browse NuGet for a png or jpg decoder package. NuGet is the Visual Studio package manager we used to link against OpenGL (In the form of OpenTK) and NAudio. 

We actually use NAudio that we got trough NuGet to decode mp3 files in the OpenTK framework. You can find LodePNG on NuGet if you want to play around with a third party decoder. LodePNG is actually faster than the built in Windows decoder.