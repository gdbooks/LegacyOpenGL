# Vertex Arrays

Thus far, all examples have used the```GL.Begin``` / ```GL.End``` method of drawing objects. This method is  refered to as __Immediate Mode Rendering__.  Immediate mode is useful for simple applications and to prototype code, as it is easy to understand and visualize. However it comes with some performance penalties that make it less useful for applications that need to render lots of geometry at a high framerate, like games.

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

There are several problems here, the first of which is that in order to render this model we make over 6000 function calls! Every function call has a very tiny overhead, but with over 6000 calls, this overhead adds up really fast! Remember, no function call is free! 

The second and third problems are illustrated in this image:

![ISSUE](issue.png)

Assuming that this mesh is made up of triangle strips (perhaps it's a portion of the mesh created in pseudo-code above), each circled vertex is redundant. In other words, these vertices are shared by more than 3 triangles, but since a triangle strip can represent at most three triangles per vertex, each of the circled vertices have to be sent to the graphics card more than once.

This results in additional bandwith when uploading data to the video card. More importantly, those vertices will need to be transformed and lit multiple times, once for each duplicate vertex.

To address these issues OpenGL provides vertex arrays. A Vertex array has the following advantages:

* Large batches of data can be sent with a small number of function calls. 
  * Using vertex arrays we could reduce the above example from 6000 function calls to 2
* Through the use of indexed vertex arrays, vertices can be sent exactly once per triangle mesh, reducing bandwith and lighting calculations
  * This is the reason 90% of the things we render are triangles, not quads or triangle strips
  * When rendered with indexed arrays we can avoid sending duplicate data

It's important to understand how Vertex Arrays work, as they are the next logical step from __immediate mode__ vertex rendering. This does not mean that they are the BEST option to render, but they are important to understand.

No professional game ships with immediate mode, vertex arrays used to be the goto solution, you could actually ship a game using vertex arrays (I have). Now that we've discussed the reasons for using vertex arrays, let's see how they are used. 




## Working with arrays

After you have enabled the array types that you will be using, the next step is to give OpenGL some data to work with. It's up to you to create arrays and fill them with the data you will be using. After you have filled the arrays with data, you need to tell OpenGL about these arrays so it can draw them. The function used to do this depends on the type of array you're using, let's look at each function in detail:

In each of the following functions, __stride__ indicates the byte offset between array elements. If the array is __tightly packed__ (meaning there is no padding between elements) you can set this to 0. Otherwise, you use the stride to compensate for padding, or to pack data for multiple attributes into a single array. 

A tightly packed array of two vertices may look like this:

```cs
float[] verts = new float[] {
  3f, 2f, 1f,
  9f, 5f, 6f
}
```

There is no padding between verices in the above array. Therefore the stride of that array is 0. But we could use a single array to hold both vertices and normals, like this:

```cs
float[] modelData = new float[] {
  3f, 2f, 1f, // VERTEX 1
  0f, 1f, 0f, // NORMAL 1
  9f, 5f, 6f, // VERTEX 2
  0f, 1f, 0f  // NORMAL 2
}
```

In the above example vertex 1 and vertex 2 are seperated by 6 floating point numbers. Similarly Normal 1 and Normal 2 are also seperated by 6 numbers. Therefore, the stride of the above array is ```6 * sizeof(float)```

The datatype of the array (float, int short, etc...) is indicated by __type__.

__data__ is a pointer to the first element of your vertex array. This might be a bit indimidating as we havent used Pointers at all yet, after all the whole point of C# is to not need pointers. But OpenGL was designed for C, some functions will need direct memory. Don't worry, we will talk about how to get and use pointers shortly.

Other paramaters will be explained as they are used in the functions.

```cs 
void GL.VertexPointer(int size, VertexPointerType type, int stride, IntPtr data);
```

This array contains positional data for vertices. __size__ is the number of coordinates per vertex, it must be 2, 3, or 4. The above example has 3 floats for every vetex, so it's size is 3. __type__ can be Short, Int, Float or Double.

```cs
void GL.TexCoordPointer(int size, TexCoordPointerType type, int stride, IntPtr data);
```

This array contains texture coordinates for each vertex. __size__ is the number of coordinates, it must be 1, 2, 3 or 4. __type__ can be Short, Int, Float or Double.

```cs
void GL.NormalPointer(NormalPointerType type, int stride, IntPtr data);
```

This array contains normal vectors for each vertex. Normals are always stored with exactly 3 coordinates (x, y, z) so there is __no size paramater__. __type__ can be Byte, Short, Int, Float or Double.

```cs
void GL.ColorPointer(int size, ColorPointerType type, int stride, IntPtr data);
```

This specifies the primary color array (vertex color). __size__ is the number of components per color, which is either 3 or 4 (RGB or RGBA). __type__ can be Byte, UnsignedByte, Short, UnsignedShort, Int, UnsignedInt, Float or Double.


After having specified which arrays OpenGL should use for each vertex attribute, you can begin to have it access the data for rendering. There are several functions you can render with, next we will talk about each of them.

#Pinning

The following section is copied from [This](http://www.opentk.com/doc/graphics/geometry/vertex-arrays) article, which explains how vertex arrays should be used. [This](http://www.opentk.com/doc/chapter/2/opengl/geometry/drawing) page also provides some very usable information on rendering in OpenTK.

Vertex Arrays use client storage, because they are stored in system memory (not video memory). Since .Net is a Garbage Collected environment, the arrays must remain pinned until the GL.DrawArrays() or GL.DrawElements() call is complete.

Pinning an array means that while the array is pinned the garbage collector is not allowed to touch it. If the arrays are unpinned prematurely, they may be moved or collected by the Garbage Collector before the draw call finishes. This will lead to random access violation exceptions and corrupted rendering, issues which can be difficult to trace.

Due to the asynchronous nature of OpenGL, ```GL.Finish()``` must be used to ensure that rendering is complete before the arrays are unpinned. Let's take a look at how this might be used in context:

```cs
struct Vertex {
    public Vector3 Position;
    public Vector2 TexCoord;
}
// Vertex size = 5 * sizeof(float)
 
Vertex[] vertices = new Vertex[100];
 
unsafe { // MUST BE CALLED TO ACCESS POINTERS
    fixed (float* pvertices = vertices) { // Pins memory
        GL.VertexPointer(3, VertexPointerType.Float, 5 * sizeof(float), pvertices);
        GL.TexCoordPointer(2, VertexPointerType.Float, 5 * sizeof(float), pvertices + sizeof(Vector3));
        GL.DrawArrays(BeginMode.Triangles, 0, vertices.Length); // Discussed in next section
        GL.Finish();    // Force OpenGL to finish rendering while the arrays are still pinned.
    }
}
```

Notice that all of the code is wrapped in an __unsafe__ section. In order to use pointers you must tell C# that the code you are using is going to be touching memory directly, since this is considered risky business in the C# world we put all such code in an unsafe block.

The __fixed__ keyword will pin memory to the desired variable until the fixed block is exited. A pointer datatype is denoted by adding an asterix (\*) after the variable type. So a pointer to an array bacomes ```float*```, but a pointer to a single floating point value would also be ```float*```. 


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

# GL.DrawElements

This function is very similar to ```GL.DrawArrays```, but it is even more powerful! With ```GL.DrawArrays```, your only option is to draw all vertices in the array sequentially. Meaning you can't reference the same vertex more than once. So ```GL.DrawArrays``` still has the problem of needing duplicate verts.

```GL.DrawElements ``` on the other hand allows you to specify the array elements in any order, and access each element (vertex) as many times as needed. Let's take a look at the function prototype:

```
void GL.DrawElements(BeginMode mode, int count, DrawElementsType type, IntPtr indices);
```

__mode__ and __count__ are used the same as in ```GL.DrawArrays```. __type__ is the type of the valies in the indices array, it should be UnsignedByte, UnsignedShort, or UnsignedInt. __indices__ is a pointer to an array containing indexes for the vertices you want to render.

Just like with ```GL.DrawArrays``` the last argument for ```GL.DrawElements``` is a pointer to an array. Because it is a pointer this must be included in unsafe code, and the arrays MUST be pinned.

To understand the value of this method, it must be reiterated that not only can you specify the indices in any order, you can also specify the same vertex repeatedly in the series. In games, most vertices will be shared by more than one polygon. By storing the vertex once and accessing it repeatedly by it's index, you can save a substantial amount of memory. 

In addition, OpenGL will only do lighting calculations once for each vertex, this means that by re-using vertices you save on performance by not having to repeat the same computation for identical vertices. Remember, lighting is the most expensive part of the pipeline.

In the next section we're going to implement a demo program using ```GL.DrawElements```.