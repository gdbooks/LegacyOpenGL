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
GL.TexImage2D(TextureTarget target, int level, PixelInternalFormat internalFormat, int width, int height, int border, PixelFormat sourceFormat, PixelType sourceType, IntPtr data)
```

Let's break each argument down
* __TextureTarget target__
  * Which texture are we loading data into? Texture1D, Texture2D, etc..
  * Most often (99% of the time) this will be ```TextureTarget.Texture2D```
* __int level__
  * Specifies how many levels of detail the image has. 
  * Level 0 is the base, each additional level is a new mip-map
  * We will cover mip-maps later. For now, keep this at 0
* __PixelInternalFormat internalFormat__
  * Specifies what the image is formatted for, Color, Light, etc..
  * 99% of the time you will use: ```PixelInternalFormat.Rgba```
  * The other two that we use are:
    * ```PixelInternalFormat.Rgb```
    * ```PixelInternalFormat.Alpha```
  * There are other options, but they are not useful for games. 
* __int width__
  * Specifies the width of the texture being loaded
  * Remember, you should be using a power of 2!
* __int height__
  * Specifies the height of the texture being loaded
  * Remember, you should be using a power of 2!
* __int border__
  * Specifies a memory border, NOT a pixel border
  * When written to OpenGL throws an error 
  * THIS MUST BE 0 (I don't know why they included this paramater)
* PixelFormat sourceFormat
  * Spefifies how the source data is laid out 
  * Values are Argb, Bgra, Rgb, Alpha, etc... 
* PixelType sourceType
  * Specifies what data type we are storing the source as
  * Most often this will be an unsigned byte, values are:
    * PixelType.UnsignedByte
    * PixelType.Short
    * PixelType.Int
  * There are more types, but those are the 3 you will most often use 

Explain arguments

So, how do we get that array of bytes that represents the texture?

## So far


## Other loading methods
The method we used to load textures here is by far the simplest. It relies on Windows to decode texture files for us. Sometimes, this isn't an option tough. For example, on an iPhone. So, how can we decode textures on non-windows platforms?

A common method is to use [stb_image](https://github.com/nothings/stb) or [LodePng](http://lodev.org/lodepng/). Both are C libraries that you have to compile into a .dll file and link against. Once compiled, you can use C#'s interop features to access the C functions.

If that sounds like a lot of work just to load a texture, well that's because it is! Another way to get texture loading to work is to browse NuGet for a png or jpg decoder package. NuGet is the Visual Studio package manager we used to link against OpenGL (In the form of OpenTK) and NAudio. 

We actually use NAudio that we got trough NuGet to decode mp3 files in the OpenTK framework. You can find LodePNG on NuGet if you want to play around with a third party decoder. LodePNG is actually faster than the built in Windows decoder.