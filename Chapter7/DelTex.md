#Deleting textures

Now that we know exactly what we have to do in the initialize function of a textured application, let's take a quick look at how to clean up a texture handle. This could not be any easyer.

First, make sure the texture you are deleting isn't bound! Remember, you can always bind 0 as the active texture (effectivley binding null). Next, you need to pass the textures handle to the ``` ``` function. This function has two variations:

```
void GL.DeleteTexture(int texture);
void GL.DeleteTextures(int n, int[] textures);
// The first argument of the second function (n) is
// the length of the textures array.
```

One of them takes a single handle and deletes it, the other takes an array of handles and deletes them all. It should be common sense, but once a texture is deleted you should not use it. If you do, no error will be thrown, but undefined behaviour will happen!

## So Far

```
int texture1 = -1;
int texture2 = -1;

void Initialize() {
    // Enable Texturing
    GL.Enable(EnableCap.Texture2D);
    
    int width = -1;
    int height = -1;
    
    // Load in textures, we don't even care 
    // about the width / height right now
    // Be sure to use the version of this 
    // function that specifies a min & mag filter!
    texture1 = LoadGLTexture("file.png", out width, out height, true);
    texture2 = LoadGLTexture("file2.png", out width, out height, false);
}

void Render() {
    // TODO: Render textures!
}

void Shutdown() {
    // Now that we are done, delete the texture handles!
    GL.DeleteTexture(texture1);
    GL.DeleteTexture(texture2);
    
    // And i like to set any int references to invalid values
    // just so i know that these are no longer usable
    texture1 = texture2 = -1;
}
```