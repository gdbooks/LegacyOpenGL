# GL.DrawArrays

When this function is called, OpenGL iterates over each of the currently enabled arrays, rendering primitives as it goes. Lets take a look at the function to understand how it works:

```cs
void GL.DrawArrays(PrimitiveType type, int first, int count);
```

__type__ serves the same basic function as the paramater of ```GL.Begin```. It specifies what type of primitives the vertex data creates. Valid values are: Points, LineStrip, LineLoop, Lines, TriangleStrip, TriangleFan, Triangles, QuadStrip, Quads and Polygon.

__first__ specifies the index at which we should start drawing. This means you can choose to only draw a part of the array data.

__count__ is how many indices to draw.

It should be noted that after calling ```GL.DrawArrays``` the state of the arrays being processed is undefined. Meaning you have to re-bind them or the behaviour of the next ```DrawArrays``` call is undefined. 

For example, lets try to render two meshes from the same arrays. The following code is bad:

```cs
void GL.EnableClientState(ArrayCap.VertexArray);
void GL.EnableClientState(ArrayCap.NormalArray);

unsafe { 
    fixed (float* pverts = vertices) {
    fixed (float* pnormas = normals) {
        GL.VertexPointer(3, VertexPointerType.Float, 0, pverts);
        GL.NormalPointer(NormalPointerType.Float, 0, pnorms);
        
        GL.DrawArrays(BeginMode.Triangles, mesh1.offset, mesh1.Length); // Draw mesh 1
        GL.DrawArrays(BeginMode.Triangles, mesh2.offset, mesh2.Length); // Draw mesh 2
        GL.Finish(); // Force OpenGL to finish rendering while the arrays are still pinned.
    } // Fixed normals
    } // Fixed vertices
}

void GL.DisableClientState(ArrayCap.NormalArray);
void GL.DisableClientState(ArrayCap.VertexArray);
```

The proper way to render them would be like this:

```cs
void GL.EnableClientState(ArrayCap.VertexArray);
void GL.EnableClientState(ArrayCap.NormalArray);

unsafe { 
    fixed (float* pverts = vertices) {
    fixed (float* pnormas = normals) {
    
        GL.VertexPointer(3, VertexPointerType.Float, 0, pverts);
        GL.NormalPointer(NormalPointerType.Float, 0, pnorms);
        GL.DrawArrays(BeginMode.Triangles, mesh1.offset, mesh1.Length); // Draw mesh 1
        
        GL.VertexPointer(3, VertexPointerType.Float, 0, pverts);
        GL.NormalPointer(NormalPointerType.Float, 0, pnorms);
        GL.DrawArrays(BeginMode.Triangles, mesh2.offset, mesh2.Length); // Draw mesh 2
        
        GL.Finish(); // Force OpenGL to finish rendering while the arrays are still pinned.
    } // Fixed normals
    } // Fixed vertices
}

void GL.DisableClientState(ArrayCap.NormalArray);
void GL.DisableClientState(ArrayCap.VertexArray);
```

Take note of how the VertexPointer and Normal pointers are re-defined between calls to ```DrawArrays```