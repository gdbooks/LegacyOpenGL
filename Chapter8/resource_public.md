#Public API

With the inner class and helper functions done, let's actually write the public API that the rest of the code will interact with.

###Initialize

The initialize function is pretty simple, first it makes sure that the game is not already initialized, if it is an error gets printed. Then we just make a new ```TextureInstance``` list and set the  ```isInitialized``` variable to true.

Why is this in the code ```managedTextures.Capacity = 100;```? By setting the initial capacity to 100 we ensure that the List (Vector) has enough room for at least 100 elements. Remember, when a vector data structure runs out of room it doubles in size, when the List runs out it will resize to 200, 400, 800, etc...

```cs
public void Initialize(OpenTK.GameWindow window) {
    if (isInitialized) {
        Error("Trying to double intialize texture manager!");
    }
    managedTextures = new List<TextureInstance>();
    managedTextures.Capacity = 100;
    isInitialized = true;
}
```

### Shutdown

Like initialize shutdown is fairly straight forward. It first checks to make sure that the manager has been initialized. 

It then loops trough all the managed textures. If anything has a reference count that is greater than 0 a warning is thrown, but the texture is deleted to make sure that we don't leak memory.

After that we just clear the list, set it to null and set ```isInitialized``` back to false. The Manager is now shut down, it can be re-initialized if needed.

```cs
public void Shutdown() {
    InitCheck("Trying to shut down a non initialized texture manager!");

    for (int i = 0; i < managedTextures.Count; ++i) {
        if (managedTextures[i].refCount > 0) {
            Warning("Texture reference is > 0: " + managedTextures[i].path);

            GL.DeleteTexture(managedTextures[i].glHandle);
            managedTextures[i] = null;
        }
        else if (managedTextures[i].refCount < 0) {
            Error("Texture reference is < 0, should never happen! " + managedTextures[i].path);
        }
    }

    managedTextures.Clear();
    managedTextures = null;
    isInitialized = false;
}
```

### LoadTexture

The public facing ```LoadTexture``` can be misleading. It actually calls to the private helper function ```LoadGLTexture``` to load the texture data. The public facing function is concerned with referenced. To make sure that no texture is loaded twice. 

This function returns an integer, it is a __handle__, it's an index into the managedTextures array. There are three possible situations this function needs to handle:

First, if the texture path requested is already loaded, increment it's reference count and return it's handle. If the texture is not already loaded, a new texture must be allocated, the next two steps deal with this allocation.

Next, it loops trough all the loaded textures in the array. If any of them have a reference count of < 0, that means that texture is already unloaded. We take this unloaded texture, and load the new texture into it's slot, essentially recycling the texture handle.

Finally, if the texture was not already loaded and we cant recycle any texture handles; we will load the texture and add it to the array, creating a new texture handle.

```cs
public int LoadTexture(string texturePath, bool UseNearestFiltering = false) {
    InitCheck("Trying to load texture without intializing texture manager!");

    if (string.IsNullOrEmpty(texturePath)) {
        Error("Load texture file path was null");
        throw new ArgumentException(texturePath);
    }

    // Check if the texture is already loaded. If it is, increment it's reference count and return it's handle.
    for (int i = 0; i < managedTextures.Count; ++i) {
        if (managedTextures[i].path == texturePath) {
            managedTextures[i].refCount += 1;
            return i;
        }
    }

    // Try to recycle an old texture handle
    for (int i = 0; i < managedTextures.Count; ++i) {
        if (managedTextures[i].refCount <= 0) {
            managedTextures[i].glHandle = LoadGLTexture(texturePath, out managedTextures[i].width, out managedTextures[i].height, UseNearestFiltering);
            managedTextures[i].refCount = 1;
            managedTextures[i].path = texturePath;
            return i;
        }
    }

    // Texture was not loaded, and there are no re-cyclable slots in the array
    // Load texture into a new array slot
    TextureInstance newTexture = new TextureInstance();
    newTexture.refCount = 1;
    newTexture.glHandle = LoadGLTexture(texturePath, out newTexture.width, out newTexture.height, UseNearestFiltering);
    newTexture.path = texturePath;
    managedTextures.Add(newTexture);
    return managedTextures.Count - 1;
}
```

###UnloadTexture

Compared to ```LoadTexture```, ```UnloadTexture``` is pretty simple. It decreases the reference count of the provided texture handle by 1. If the reference count of a texture reaches 0, that texture is deleted, it's handle will become eligable to be recycled.

```cs
public void UnloadTexture(int textureId) {
    InitCheck("Trying to unload texture without intializing texture manager!");
    IndexCheck(textureId, "UnloadTexture");

    managedTextures[textureId].refCount -= 1;
    if (managedTextures[textureId].refCount == 0) {
        GL.DeleteTexture(managedTextures[i].glHandle);
    }
    else if (managedTextures[textureId].refCount < 0) {
        Error("Ref count of texture is less than 0: " + managedTextures[textureId].path);
    }
}
```

###GetTextureWidth

This function is a simple getter that returns the width of a texture. This is information that ```LoadTexture``` stored inside the actual ```managedTextures``` element when the texture was loaded.

```cs
public int GetTextureWidth(int textureId) {
    InitCheck("Trying to access texture width without intializing texture manager!");
    IndexCheck(textureId, "GetTextureWidth");

    return managedTextures[textureId].width;
}
```

###GetTextureHeight

This function is a simple getter that returns the height of a texture. This is information that ```LoadTexture``` stored inside the actual ```managedTextures``` element when the texture was loaded.

```cs
public int GetTextureHeight(int textureId) {
    InitCheck("Trying to access texture height without intializing texture manager!");
    IndexCheck(textureId, "GetTextureHeight");

    return managedTextures[textureId].height;
}
```

###GetTextureSize

This function works just like ```GetTextureWidth``` and ```GetTextureHeight```. It packs the return values into a ```Size``` struct.

```cs
public Size GetTextureSize(int textureId) {
    InitCheck("Trying to access texture size without intializing texture manager!");
    IndexCheck(textureId, "GetTextureSize");

    return new Size(managedTextures[textureId].width, managedTextures[textureId].height);
}
```

```cs
public int GetGLHandle(int textureId) {
    InitCheck("Trying to access texture ID without intializing texture manager!");
    IndexCheck(textureId, "GetGLHandle");

    return managedTextures[textureId].glHandle;
}
```