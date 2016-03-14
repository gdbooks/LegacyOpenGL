# Vertex Arrays

Thus far, all examples have used the```GL.Begin``` / ```GL.End``` method of drawing objects. This method is  refered to as __Immediate Mode Rendering__.  Immediate mode is useful for simple applications and prototype code as it is easy to understand and visualize. However it comes with some performance penalties that make it less useful for applications that need to render lots of geometry at a high framerate, like games.

For example, let's say you're rendering a model containing 2,000 lit and textured triangles using immediate mode. Assume that you're able to pack all of the vertex data into a single triangle strip (This is the best case scenario, it's often not possible). Your rendering code _might_ look like this:

```cs
GL.Begin(PrimitiveType.TriangleStrip);

for (int i = 0; i < 2002; ++i) { // 2002 because of the triangle STRIP
  GL.Normal3(normals[i][0], normals[i][1], normals[i][2]);
  GL.TexCoord2(uvs[i][0], uvs[i][1]);
  GL.Vertex3(verts[i][0], verts[i][1], verts[i][2]);
}

GL.End();
```

There are several problems here, the first of which is that in order to render this model we make over 6000 function calls! Remember, no function call is free!