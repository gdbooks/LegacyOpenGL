#Helper Fucntions

Let's start off by implementing all of the helper functions we're going to need

###Error

This function will print something to the console in red. We use it to message any errors to the developer. When an error occurs it will not break the game, but fixing it should be the number 1 priority

```cs
private void Error(string error) {
    ConsoleColor old = Console.ForegroundColor;
    Console.ForegroundColor = ConsoleColor.Red;
    Console.WriteLine(error);
    Console.ForegroundColor = old;
}
```

### Warning

This function will print something to the console in yellow. Unlike errors warnings are not super high up on the priority list, they just have to be fixed before the game ships.

```cs
private void Warning(string error) {
    ConsoleColor old = Console.ForegroundColor;
    Console.ForegroundColor = ConsoleColor.Yellow;
    Console.WriteLine(error);
    Console.ForegroundColor = old;
}
```

###InitCheck

This functions takes a look at the member variable ```isInitialized``` and throws an error if the manager has not been initialized. We use this in EVERY public API function to make sure no function is called before the manager is initialized.

```cs
private void InitCheck(string errorMessage) {
    if (!isInitialized) {
        Error(errorMessage);
    }
}
```

### IndexCheck

A large number of our public API functions take a handle (index into the ```managedTextures``` array) as an argument. Those functions will call this function to make sure that the index passed in is valid.

```cs
private void IndexCheck(int index, string function) {
    if (managedTextures == null) {
        Error(function + " is trying to access element " + index + " when managedTextures is null");
        return;
    }
    if (index < 0) {
        Error(function + " is trying to access a negative element: " + index);
        return;
    }
    if (index > managedTextures.Count) {
        Error(function + " is trying to access out of bounds element at: " + index + ", bounds size: " + managedTextures.Count);
        return;
    }
    if (managedTextures[index].refCount <= 0) {
        Warning(function + " is acting on a texture with a reference count of: " + managedTextures[index].refCount);
    }
}
```

### IsPowerOfTwo

Given an integer, this function returns true of the integer is a power of two, false otherwise. Remember, we only want to load textures that are POT. This function is called in the LoadTexture function to ensure that the loaded texture is a POT texture.

I didn't actually write this function, i just googled "C# Is Power Of Two" and copied the best answer from a stack overflow article. I'd give you the link, but i've since lost the article.

```cs
private bool IsPowerOfTwo(int x) {
    if (x > 1) {
        while (x % 2 == 0) {
            x >>= 1;
        }
    }
    return x == 1;
}
```

###LoadGLTexture

This function is the powerhouse of the manager. It actually loads a texture and returns it's OpenGL texture handle. It also returns the textures width and height in arguments marked as ```out```.

Most of the code should be fairly familiar, you've loaded plenty of textures before. Pay attention to the error logging, this is the only place that ```IsPowerOfTwo``` is actually called.

```cs
private int LoadGLTexture(string filename, out int width, out int height, bool nearest) {
    int id = GL.GenTexture();
    GL.BindTexture(TextureTarget.Texture2D, id);

    if (nearest) {
        GL.TexParameter(TextureTarget.Texture2D, TextureParameterName.TextureMinFilter, (int)TextureMinFilter.Nearest);
        GL.TexParameter(TextureTarget.Texture2D, TextureParameterName.TextureMagFilter, (int)TextureMagFilter.Nearest);
    }
    else {
        GL.TexParameter(TextureTarget.Texture2D, TextureParameterName.TextureMinFilter, (int)TextureMinFilter.Linear);
        GL.TexParameter(TextureTarget.Texture2D, TextureParameterName.TextureMagFilter, (int)TextureMagFilter.Linear);
    }

    Bitmap bmp = new Bitmap(filename);
    BitmapData bmp_data = bmp.LockBits(new Rectangle(0, 0, bmp.Width, bmp.Height), ImageLockMode.ReadOnly, System.Drawing.Imaging.PixelFormat.Format32bppArgb);

    if (!IsPowerOfTwo(bmp.Width)) {
        Warning("Texture width non power of two: " + filename);
    }

    if (!IsPowerOfTwo(bmp.Height)) {
        Warning("Texture height  non power of two: " + filename);
    }

    if (bmp.Width > 2048) {
        Warning("Texture width > 2048: " + filename);
    }

    if (bmp.Height > 2048) {
        Warning("Texture height > 2048: " + filename);
    }

    GL.TexImage2D(TextureTarget.Texture2D, 0, PixelInternalFormat.Rgba, bmp_data.Width, bmp_data.Height, 0, OpenTK.Graphics.OpenGL.PixelFormat.Bgra, PixelType.Short, bmp_data.Scan0);

    bmp.UnlockBits(bmp_data);

    width = bmp.Width;
    height = bmp.Height;
    return id;
}
```