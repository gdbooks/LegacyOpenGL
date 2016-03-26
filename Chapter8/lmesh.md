# Implementation

It may not be immediateley obvious, but because an OBJ file has 3 indices per vertex attribute (Look at the f paramater in the last section) it does not suit its-self well for indexed rendering. This is because a vertex position and normal might not appear at the same index.

Because of this we will need to serialize the data, meaning make it linear. So first we're going to load the data into temp arrays, then loop trough the arrays and flatten them out. Then we can render un-indexed using ```GL.DrawArrays```.

## Skeleton
I'm going to get you started with a class skeleton for the OBJ loader:

```cs
using System;
using System.IO;
using System.Collections.Generic;
using OpenTK.Graphics.OpenGL;

namespace GameApplication {
    class OBJModel {
        protected int vertexBuffer = -1;

        protected bool hasNormals = false;
        protected bool hasUvs = false;

        protected int numVerts = 0;
        protected int numNormals = 0;
        protected int numUvs = 0;

        public OBJModel(string path) {
            // TODO: Load
        }

        public void Destroy() {
            // TODO: Destroy
        }

        public void Render(bool useNormals = true, bool useTextures = true) {
            if (vertexBuffer == -1) {
                return;
            }

            if (!hasNormals) {
                useNormals = false;
            }

            if (!hasUvs) {
                useTextures = false;
            }

            // TODO: Render
        }
    }
}
```