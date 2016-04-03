#Public API

With the inner class and helper functions done, let's actually write the public API that the rest of the code will interact with.

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

```cs
public void Shutdown() {
    InitCheck("Trying to shut down a non initialized texture manager!");

    for (int i = 0; i < managedTextures.Count; ++i) {
        if (managedTextures[i].refCount != 0) {
            Warning("Texture reference is > 0: " + managedTextures[i].path);
        }
        GL.DeleteTexture(managedTextures[i].glHandle);
        managedTextures[i] = null;
    }

    managedTextures.Clear();
    managedTextures = null;
    isInitialized = false;
}
```

```cs
public int LoadTexture(string texturePath, bool UseNearestFiltering = false) {
    InitCheck("Trying to load texture without intializing texture manager!");

    if (string.IsNullOrEmpty(texturePath)) {
        Error("Load texture file path was null");
        throw new ArgumentException(texturePath);
    }

    for (int i = 0; i < managedTextures.Count; ++i) {
        if (managedTextures[i].path == texturePath) {
            managedTextures[i].refCount += 1;
            return i;
        }
    }

    for (int i = 0; i < managedTextures.Count; ++i) {
        if (managedTextures[i].refCount <= 0) {
            GL.DeleteTexture(managedTextures[i].glHandle);
            managedTextures[i].glHandle = LoadGLTexture(texturePath, out managedTextures[i].width, out managedTextures[i].height, UseNearestFiltering);
            managedTextures[i].refCount = 1;
            managedTextures[i].path = texturePath;
            return i;
        }
    }

    TextureInstance newTexture = new TextureInstance();
    newTexture.refCount = 1;
    newTexture.glHandle = LoadGLTexture(texturePath, out newTexture.width, out newTexture.height, UseNearestFiltering);
    newTexture.path = texturePath;
    managedTextures.Add(newTexture);
    return managedTextures.Count - 1;
}
```

```cs
public void UnloadTexture(int textureId) {
    InitCheck("Trying to unload texture without intializing texture manager!");
    IndexCheck(textureId, "UnloadTexture");

    managedTextures[textureId].refCount -= 1;

    if (managedTextures[textureId].refCount < 0) {
        Error("Ref count of texture is less than 0: " + managedTextures[textureId].path);
    }
}
```

```cs
public int GetTextureWidth(int textureId) {
    InitCheck("Trying to access texture width without intializing texture manager!");
    IndexCheck(textureId, "GetTextureWidth");

    return managedTextures[textureId].width;
}
```

```cs
public int GetTextureHeight(int textureId) {
    InitCheck("Trying to access texture height without intializing texture manager!");
    IndexCheck(textureId, "GetTextureHeight");

    return managedTextures[textureId].height;
}
```

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