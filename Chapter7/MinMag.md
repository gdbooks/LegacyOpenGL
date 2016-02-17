# Min and Mag Filters
We're almost done! We've managed to load some memory onto the GPU at this point, but if we tried to render that memory as a texutre, nothing would show up. This is because our texture loading code neglected to specify min and mag filters. There is a TODO comment section in the function.

Min and mag filters describe to OpenGL what to do when we are trying to render a 256x256 texture on a 512x512 surface, or even a 128x128 surface! There is no clear way to map the pixels of a smaller image to a larger surface or a larger image to a smaller surface. Each approach in handling the problem has it's own ups and downs, so OpenGL lets you pick what to do.

## What is a Min filter

Min filter is the minification filter. It is applied when an image is zoomed out so far that multiple pixels (texels) on the source image make up a single pixel (fragment) on the display screen. There are two common settings

* __Nearest__
  * Nearest neigbor filtering, does not attempt to scale the image
  * Result is sharp / pixelated
* __Bilinear__
  * Bilinear filtering, attempts to resize the image
  * Result is soft / blurred

Either way you set the min filter, the resulting image will be less than ideal because certain pixels have to be skipped. Usually, min filtering only needs to effect far away objects, so this isn't too big of an issue.

![MIN](min_filter.png)

## What is a Mag filter

Mag filter is the magnification filter. It is applied when an image is zoomed in so close that one pixel (texel) on the source image takes up multiple pixels (fragments) on the display screen. There are two common settings for mag filtering:

* __Nearest__ 
  * Nearest neightbor filtering means no scale is applied to the texture
  * The resulting image will be sharp, but very pixelated
* __Bilinear__
  * Use bilinear filtering to resize (enlarge) the source image
  * The result will be blured, but the image will look smooth

![MAG](mag_filter.png)

## Together now

99% of the time you will set these properties to be the same. That is you will have a nearest min and mag, or a bilinear min and mag. It's RARE (I have never done this) to need a nearest mag and a bilinear min.

2D games (Like our OpenTK framework) tend to use nearest neighbor filtering. This helps keep pixels looking sharp and crisp! It's an art style, after all you don't want mario to have blurry edges.

In contrast 3D games tend to use bilinear filtering. Because the world is smooth and continous, you really want to maintain that illusion, even if it means blurring your image a little.

There are of course exceptions. Minecraft for instance uses nearest neighbor filtering despite being a 3D game. And most Klei games use bilinear filtering, even tough they are 2D games.

## Code
Setting the min and mag filters in code is pretty straight forward. You call the ```GL.TexParameter``` function two times, once for the min and once for the mag filters.

```
GL.TexParameter(TextureTarget target, TextureParameterName param, int value)
```

The first argument, __target__ is of course which texture this command targets. More often than not the value of this is going to be ```Texture2D```. The second paramater, __param__ is the important one, it tells OpenGL what texture paramater you are setting. We want to set ```TextureMagFilter``` or ```TextureMinFilter```. The last paramater is an integer, this is a bit of a magic number. There is an enumeration ```TextureMagFilter```, you can cast the values of this enum into an int for the last paramater

Knowing what the function looks like, this is how you would go about setting a linear filter

```
GL.TexParameter(TextureTarget.Texture2D, TextureParameterName.TextureMinFilter, (int)TextureMinFilter.Linear);
GL.TexParameter(TextureTarget.Texture2D, TextureParameterName.TextureMagFilter, (int)TextureMagFilter.Linear);
```

And this is how you would set a nearest filter

```
GL.TexParameter(TextureTarget.Texture2D, TextureParameterName.TextureMinFilter, (int)TextureMinFilter.Nearest);
GL.TexParameter(TextureTarget.Texture2D, TextureParameterName.TextureMagFilter, (int)TextureMagFilter.Nearest);
```

## Load Texture

Let's retrofit setting the min and mag filters into the ```LoadGLTexture``` function we wrote in the last section. We're going to take advantage of the fact that both min and mag filters tend to be set to the same value by simply adding one new argument to the function.

This new argument, __bool nearest__ if true will make the function use nearest filtering. If false, it will use bilinear.

```
private int LoadGLTexture(string filename, out int width, out int height, bool nearest) {
    if (string.IsNullOrEmpty(filename)) {
        Error("Load texture file path was null");
        throw new ArgumentException(filename);
    }

    // Generate a handle on the GPU
    int id = GL.GenTexture();
    
    // Bind the handle to the be the active texture.
    GL.BindTexture(TextureTarget.Texture2D, id);
    
    ///////////////////////////////////////////////
    // THIS IS NEW
    Set appropriate filters
    if (nearest) {
        GL.TexParameter(TextureTarget.Texture2D, TextureParameterName.TextureMinFilter, (int)TextureMinFilter.Nearest);
        GL.TexParameter(TextureTarget.Texture2D, TextureParameterName.TextureMagFilter, (int)TextureMagFilter.Nearest);
    }
    else {
        GL.TexParameter(TextureTarget.Texture2D, TextureParameterName.TextureMinFilter, (int)TextureMinFilter.Linear);
        GL.TexParameter(TextureTarget.Texture2D, TextureParameterName.TextureMagFilter, (int)TextureMagFilter.Linear);
    }
    ///////////////////////////////////////////////

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

In this example we set the texture filter after the texture was bound, but before it is filled with data. So long as the texture is bound, we can set its filtering mode any time, you don't HAVE to set it before it is filled with data. As a matter of fact, you can change this during runtime!

However it's considered best practice to set the filtering before filling a texture with data, and changing the filtering at runtime has a MUCH higher performance penalty than just having a second, duplicate texture with different filtering. So, follow the above convention