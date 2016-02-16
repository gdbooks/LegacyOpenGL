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
* __PixelFormat sourceFormat__
  * Spefifies how the source data is laid out 
  * Values are Argb, Bgra, Rgb, Alpha, etc... 
* __PixelType sourceType__
  * Specifies what data type we are storing the source as
  * Most often this will be an unsigned byte, values are:
    * PixelType.UnsignedByte
    * PixelType.Short
    * PixelType.Int
  * There are more types, but those are the 3 you will most often use 
* __IntPtr data__
  * An IntPtr is an unsafe data type
  * It can point to any array of numeric data (int[], byte[], etc...)
  * This is a long, single dimensional array containing the pixels to upload to the GPU

The function looks complicated, but you only have to really write it once in a friendly wrapper. After calling this function, you can discard the pixel data you are holding onto on the CPU, as the function will have uploaded it to the GPU

## Decoder
So, how do we get that array of bytes that represents the texture? We have to decode a texture from it's source format (png, jpg, tga, etc...) into an array of bytes. We're going to use built-in windows functions to do this.

In order to use windows to decode, you must include a reference to __System.Drawing__, as it contains the ```Bitmap``` class that we will use to decode the texture. The following block of code is commented to explain what is involved in devoding a texture. 

```
private int LoadGLTexture(string filename, out int width, out int height) {
    if (string.IsNullOrEmpty(filename)) {
        Error("Load texture file path was null");
        throw new ArgumentException(filename);
    }

    // Generate a handle on the GPU
    int id = GL.GenTexture();
    
    // Bind the handle to the be the active texture.
    GL.BindTexture(TextureTarget.Texture2D, id);

    /* TODO: 
     *    Set appropriate min and mag filters
     *    we will talk about these in the next section
    */
    
    // Allocate CPU system memory for the image
    // This will load the encoded texture into CPU memory
    Bitmap bmp = new Bitmap(filename);
    
    // Decode the image data and store the byte array into CPU memory
    BitmapData bmp_data = bmp.LockBits(new Rectangle(0, 0, bmp.Width, bmp.Height), ImageLockMode.ReadOnly, System.Drawing.Imaging.PixelFormat.Format32bppArgb);

    /* TODO: 
     *    Check bmp.Width and bmp.Height, if they are not a power
     *    of two, throw an error
    */

    // Upload the image data to the GPU
    GL.TexImage2D(TextureTarget.Texture2D, 0, PixelInternalFormat.Rgba, bmp_data.Width, bmp_data.Height, 0, OpenTK.Graphics.OpenGL.PixelFormat.Bgra, PixelType.Short, bmp_data.Scan0);
    
    // Mark CPU memory eligable for GC, disposing it
    bmp.UnlockBits(bmp_data);

    // Return the textures width, height and GPU ID
    width = bmp.Width;
    height = bmp.Height;
    return id;
}
```

Using this function is pretty easy. It returns the texture handle and gives you the width & height of the loaded texture as out paramaters:

```
int texWidth = -1;
int texHeight = -1;
int texHandle = LoadGLTexture("File.png", out texWidth, out texHeight);
```

## Width & Height
By far the easyest way to obtain the width and height of a texture is to store it at the time of loading that texture. However, you don't HAVE to do it this way. You can get the width or height of a texture anytime with the ``` ``` function.

```
int GetWidth(int textureId) {
    GL.BindTexture(TextureTarget.Texture2D, textureId);
    int result = 0;
    GL.GetTexLevelParameter(TextureTarget.Texture2D, 0, GetTextureParameter.TextureWidth, out result);
}
```

## So far


## Other loading methods
The method we used to load textures here is by far the simplest. It relies on Windows to decode texture files for us. Sometimes, this isn't an option tough. For example, on an iPhone. So, how can we decode textures on non-windows platforms?

A common method is to use [stb_image](https://github.com/nothings/stb) or [LodePng](http://lodev.org/lodepng/). Both are C libraries that you have to compile into a .dll file and link against. Once compiled, you can use C#'s interop features to access the C functions.

If that sounds like a lot of work just to load a texture, well that's because it is! Another way to get texture loading to work is to browse NuGet for a png or jpg decoder package. NuGet is the Visual Studio package manager we used to link against OpenGL (In the form of OpenTK) and NAudio. 

We actually use NAudio that we got trough NuGet to decode mp3 files in the OpenTK framework. You can find LodePNG on NuGet if you want to play around with a third party decoder. LodePNG is actually faster than the built in Windows decoder.