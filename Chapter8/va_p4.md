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