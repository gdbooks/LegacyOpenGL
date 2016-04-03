# Implementation

Let's start laying out the skeleton of the main class and implementing the helper class. One of the new things you will notice is i have blocks of functions wrapped in ```#region``` and ```#endregion``` tags.

These tags have no effect on the final executable, they are there to visually help the code be more easily readable and navigatable. They get compiled out like comments.

```cs
using System;
using System.Drawing;
using System.Drawing.Imaging;
using OpenTK.Graphics.OpenGL;
using System.Collections.Generic;

namespace GameFramework {
    public class TextureManager {
    
#region Singleton
        private static TextureManager instance = null;
        public static TextureManager Instance {
            get {
                if (instance == null) {
                    instance = new TextureManager();
                }
                return instance;
            }
        }

        private TextureManager() {

        }
#endregion

#region HelperClass
        private class TextureInstance {
            public int glHandle = -1;
            public string path = string.Empty;
            public int refCount = 0;
            public int width = 0;
            public int height = 0;
        }
#endregion

#region MemberVariables
        private List<TextureInstance> managedTextures = null;
        private bool isInitialized = false;
#endregion

#region HelperFunctions
        private void Error(string error) { }
        private void Warning(string error) { }
        private void InitCheck(string errorMessage) { }
        private void IndexCheck(int index, string function) { }
        private bool IsPowerOfTwo(int x) { }
        private int LoadGLTexture(string filename, out int width, out int height, bool nearest) { }
#endregion

#region PublicAPI
        public void Initialize(OpenTK.GameWindow window) { }
        public void Shutdown() { }
        public int LoadTexture(string texturePath, bool UseNearestFiltering = false) { }
        public void UnloadTexture(int textureId) { }
        public int GetTextureWidth(int textureId) { }
        public int GetTextureHeight(int textureId) { }
        public Size GetTextureSize(int textureId) { }
        public int GetGLHandle(int textureId) { }
#endregion
    }
}
```

As you can see this class is a singleton, therefore it has a public ```Instance``` accessor, and it's constructor is marked as __private__ to ensure that only this class can make a new instance of it's self.

We defined a private inner class called ```TextureInstance```. Because it's an inner class, you can only make a new instance of it as ```new TextureManager.TextureInstance``` outside of ```TextureManager```, but inside you can just use ```new TextureInstance```. This however doesn't matter, because the inner class is private, nothing outside of ```TextureManager``` can ever reference it.