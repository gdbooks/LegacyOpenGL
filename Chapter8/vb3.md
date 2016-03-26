# Rendering

This is where things get interesting! OpenGL tries to re-use the same functons it did for vertex array rendering, but with the arguments interpreted differently. This in my opinion is a VERY bad idea, but it's how they decided to do it. 

So if the functions are the same then what tells OpenGL that we want to use vertex buffers instead of arrays? The bound buffer. If a buffer is bound, OpenGL assumes you are rendering VBO's, if not it assumes you are rendering vertex arrays. 


## Example

Let's take a look at some code that we used to render vertex arrays:


```cs
    GL.EnableClientState(ArrayCap.VertexArray);
    GL.EnableClientState(ArrayCap.ColorArray);
            
    GL.VertexPointer(3, VertexPointerType.Float, 0, cubeVertices);
    GL.ColorPointer(3, ColorPointerType.Float, 0, cubeColors);

    GL.DrawElements(PrimitiveType.Triangles, cubeIndices.Length, DrawElementsType.UnsignedInt, cubeIndices);

    GL.DisableClientState(ArrayCap.VertexArray);
    GL.DisableClientState(ArrayCap.ColorArray);
```

Now let's convert this to use VBO's. The first thing we need to do is to enable buffers before the call to ```GL.VertexPointer```. I'm also going to make sure to disable them once i'm done rendering. Like so:

```cs
    GL.EnableClientState(ArrayCap.VertexArray);
    GL.EnableClientState(ArrayCap.ColorArray);

    // NEW
    GL.BindBuffer(BufferTarget.ArrayBuffer, vertexBuffer);
    GL.BindBuffer(BufferTarget.ElementArrayBuffer, indexBuffer);

    GL.VertexPointer(3, VertexPointerType.Float, 0, cubeVertices);
    GL.ColorPointer(3, ColorPointerType.Float, 0, cubeColors);

    GL.DrawElements(PrimitiveType.Triangles, cubeIndices.Length, DrawElementsType.UnsignedInt, cubeIndices);
    
    // NEW
    GL.BindBuffer(BufferTarget.ArrayBuffer, 0);
    GL.BindBuffer(BufferTarget.ElementArrayBuffer, 0);

    GL.DisableClientState(ArrayCap.VertexArray);
    GL.DisableClientState(ArrayCap.ColorArray);
```

Next up we need to change the ```GL.VertexPointer``` and ```GL.ColorPointer``` functions. The only thing that changes is the last argument. Instead of being an array, it now becomes an int pointer, but it does not point to anything! 

Instead of using an int-pointer we just pack an integer into the pointer If this sounds stupid, that's because it is, it's a leftover thing from OpenGL being a C-API. So this last paramater now signifies the offset from the begenning of the buffer that our data is located at.

```cs
    GL.VertexPointer(3, VertexPointerType.Float, 0, new System.IntPtr(0));
    GL.ColorPointer(3, ColorPointerType.Float, 0, new System.IntPtr(sizeof(float) * 8 * 3));
```

In my buffer the first 8 * 3 floats are the position of my cube. So, the cube vertices are at offset 0. But then the colors come after, so those are 

```8 * 3 * sizeof(float)```  bytes away from the front of the buffer.

Now, if we didn't have an index buffer bound, this would already render. Just because you have a vertex buffer doesn't mean you _have_ to use an index buffer. But more often than not you will. This means that we must also change the ```GL.DrawElements``` function. 

The ```GL.DrawElements``` fucntion changes the same way the others did. The last argument becomes an int-pointer. It's value is an integer, not a real pointer. This integer specifies how many bytes from the begenning of the buffer the data we are rendering is located. The new call becomes:

```
GL.DrawElements(PrimitiveType.Triangles, numIndices, DrawElementsType.UnsignedInt, System.IntPtr.Zero);
```

## Putting it all together

Let's take a look at the new render function in full context

```cs
public override void Render() {
    Matrix4 lookAt = Matrix4.LookAt(new Vector3(-7.0f, 5.0f, -7.0f), new Vector3(0.0f, 0.0f, 0.0f), new Vector3(0.0f, 1.0f, 0.0f));
    GL.LoadMatrix(Matrix4.Transpose(lookAt).Matrix);

    GL.Disable(EnableCap.DepthTest);
    grid.Render();
    GL.Enable(EnableCap.DepthTest);

    GL.EnableClientState(ArrayCap.VertexArray);
    GL.EnableClientState(ArrayCap.ColorArray);

    // Bind vertex and index buffer, OpenGL will use VBO's from here on out
    GL.BindBuffer(BufferTarget.ArrayBuffer, vertexBuffer);
    GL.BindBuffer(BufferTarget.ElementArrayBuffer, indexBuffer);

    // 0 bytes from the begenning of the buffer
    GL.VertexPointer(3, VertexPointerType.Float, 0, new System.IntPtr(0));
    // The buffer first contains positions, which are 8 vertices, made up of 3 floats each.
    // after that comes the color information, therefore the colors are:
    // 8 * 3 * sizeof(float) bytes away from the begenning of the buffer
    GL.ColorPointer(3, ColorPointerType.Float, 0, new System.IntPtr(sizeof(float) * 8 * 3));

    // The index buffer only contains indices we want to draw, so they are 0 bytes
    // from the begenning of the array. You can use the constant i use here instead
    // of making a new IntPtr, if the offset you are looking for is 0
    GL.DrawElements(PrimitiveType.Triangles, numIndices, DrawElementsType.UnsignedInt, System.IntPtr.Zero);

    // Unbind vertex and index buffers, OpenGL will draw VA's from here on out
    GL.BindBuffer(BufferTarget.ArrayBuffer, 0);
    GL.BindBuffer(BufferTarget.ElementArrayBuffer, 0);

    GL.DisableClientState(ArrayCap.VertexArray);
    GL.DisableClientState(ArrayCap.ColorArray);
}
```