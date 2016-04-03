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
        private class TextureInstance {
            public int glHandle = -1;
            public string path = string.Empty;
            public int refCount = 0;
            public int width = 0;
            public int height = 0;
        }

        private List<TextureInstance> managedTextures = null;
        private bool isInitialized = false;

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
