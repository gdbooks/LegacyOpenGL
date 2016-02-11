#Texture Binding

In most API's that use handles, functions which effect the handle take the handle as their first argument. Take for example how we did our 2D framework:

```
// This is NOT OpenGL, it's from the 2D framewrok

// First we intialize the texture manager
TextureManager.Instance.Initialize(Window);

// Next we generate a texture handle
int texBird = TextureManager.Instance.LoadTexture("Assets/Bird.png");

// Finally we draw the texture by passing the handle
// as the first paramater to the draw function
TextureManager.Instance.Draw(texBird, new Point(0, 0));
```

OpenGL does not work like this.

### Binding

OpenGL has the concept of a "currently bound texture". When you plan to use a texture for anything (rendering usually), you bind it to the OpenGL context (with the ```GL.BindTexture``` function). After a texture is bound, all subsequent operations that use textures will use the currently bound texture.

This is what the function call for binding a texture looks like:

```
GL.BindTexture(TextureTarget proxy, int texId);
```

To bind no texture, set the second argument (texId) to 0. By default, no texture is bound. What is the first argument? That ```TextureTarget``` enumeration.... It's a list of texture types that can be bound. We only care about the following entries:

* TextureTarget.Texture1D
* TextureTarget.Texture2D
* TextureTarget.Texture3D
* TextureTarget.TextureCubeMap

There are a lot of other options, but seeing how those are the four texture types we can actually create, they are the only ones we care about.

What happens if you set the 