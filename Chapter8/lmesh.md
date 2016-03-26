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

The member variables are all protected becasue they are only used for rendering.

## Loading

Loading can be broken up into two parts, first reading all of the data in, then parsing all of the data. I'm going to provide some skeleton code for you to work in for this one.

First let's make 6 arrays. One to hold sequential vertex information, one for normals and one for texCoords. When you encounter a line like

```
v 0 10.0 20.0
```

That should add 3 floats to the vertices array, 0, 10 and 20. Same for normals and tex-coords. 

Then we have three more arrays, all unsigned integers. These are for the actual triangle data. For example, if a triangle is listed as such

```
f 1//2 4//5 6//8
```

That should put 1, 4 and 6 into the vertex index array, 2, 5 and 8 into the normal index array and nothing into the uv index array. Do take note, ANY of those numbers could be missing, be in the double digits, etc...

```cs
public OBJModel(string path) {
    List<float> vertices = new List<float>();
    List<float> normals = new List<float>();
    List<float> texCoords = new List<float>();

    List<uint> vertIndex = new List<uint>();
    List<uint> normIndex = new List<uint>();
    List<uint> uvIndex = new List<uint>();

    using (TextReader tr = File.OpenText(path)) {
        string line = tr.ReadLine();
        while (line != null) {
            if (string.IsNullOrEmpty(line) || line.Length < 2) {
                continue;
            }

            // TODO Parse Line, fill out above arrays

            line = tr.ReadLine();
        }
    }
```

Once all the data is parsed, it's time to process it into something that's a bit more sequential. For this i'm going to make 3 new arrays that contain positions, normals and uv's all in order.

```cs
    List<float> vertexData = new List<float>();
    List<float> normalData = new List<float>();
    List<float> uvData = new List<float>();
```
  
  Then, we're going to loop trough the index arrays we've build up and fill the sequential data up in order. One of the things you will notice is ```index * 3 + 1```, why is this needed all over the place? 
  
  Because indexin assumes that we have an array of float3's, that is, each array element is 3 floats. C# would support this with a multidimensional array, but we can modify the indexing of our big array to emulate that effect. 
  
```cs
    for (int i = 0; i < vertIndex.Count; ++i) {
        vertexData.Add(vertices[(int)vertIndex[i] * 3 + 0]);
        vertexData.Add(vertices[(int)vertIndex[i] * 3 + 1]);
        vertexData.Add(vertices[(int)vertIndex[i] * 3 + 2]);
    }
    for (int i = 0; i < normIndex.Count; ++i) {
        normalData.Add(normals[(int)normIndex[i] * 3 + 0]);
        normalData.Add(normals[(int)normIndex[i] * 3 + 1]);
        normalData.Add(normals[(int)normIndex[i] * 3 + 2]);
    }
    for (int i = 0; i < uvIndex.Count; ++i) {
        uvData.Add(texCoords[(int)uvIndex[i] * 2 + 0]);
        uvData.Add(texCoords[(int)uvIndex[i] * 2 + 1]);
    }
```

We now have enough data to fill in the class member variables:

```cs
    hasNormals = normalData.Count > 0;
    hasUvs = uvData.Count > 0;

    numVerts = vertexData.Count;
    numUvs = uvData.Count;
    numNormals = normalData.Count;

```

Finally it's time to upload all this data to the GPU, i'm going to make one last array, this is going to be used to transfer ALL of the above properties to OpenGL. Then we're just going to make a buffer and populate it with data from this new array.

```
    List<float> data = new List<float>();
    data.AddRange(vertexData);
    data.AddRange(normalData);
    data.AddRange(uvData);

    vertexBuffer = GL.GenBuffer();
    GL.BindBuffer(BufferTarget.ArrayBuffer, vertexBuffer);
    GL.BufferData(BufferTarget.ArrayBuffer, new IntPtr(data.Count * sizeof(float)), data.ToArray(), BufferUsageHint.StaticDraw);
    GL.BindBuffer(BufferTarget.ArrayBuffer, 0);
}
```