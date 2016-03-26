#Review

That was a lot of information in a very short ammount of time. Let's real quick review all the things we've learned.

## Globals

You will need 3 pieces of information, a global reference to your vertex buffer, to your index buffer and you will need to know how many indices you are rendering:

```cs
class SomeClass {
    protected int vertexBuffer;
    protected int indexBuffer;
    protected int numIndices;
```

## Initialize

Inside the initialize function you need to generate your buffers and fill them with data. Make sure to unbind at the end, so that rendering doesn't automatically assume it's supposed to render with VBO's.

```cs
public override void Initialize() {
    grid = new Grid(true);

    float[] vertexData = new float[] {
        ...
    };

    uint[] indexData = new uint[] {
        ...
    };
    
    numIndices = indexData.Length;

    vertexBuffer = GL.GenBuffer();
    GL.BindBuffer(BufferTarget.ArrayBuffer, vertexBuffer);
    GL.BufferData(BufferTarget.ArrayBuffer, new System.IntPtr(vertexData.Length * sizeof(float)), vertexData, BufferUsageHint.DynamicDraw);

    indexBuffer = GL.GenBuffer();
    GL.BindBuffer(BufferTarget.ElementArrayBuffer, indexBuffer);
    GL.BufferData(BufferTarget.ElementArrayBuffer, new System.IntPtr(indexData.Length * sizeof(uint)), indexData, BufferUsageHint.StaticDraw);

    GL.BindBuffer(BufferTarget.ElementArrayBuffer, 0);
    GL.BindBuffer(BufferTarget.ElementArrayBuffer, 0);
}
```

## Render

Rendering is almost the same as rendering vertex arrays. You use the same functions. All you have to do is bind your VBO (Vertex Buffer Object) and possibly IBO (Index Buffer Object), then treat the last element of each function call as a byte offset into the appropriate buffer.

```cs
public override void Render() {
    Matrix4 lookAt = Matrix4.LookAt(new Vector3(-7.0f, 5.0f, -7.0f), new Vector3(0.0f, 0.0f, 0.0f), new Vector3(0.0f, 1.0f, 0.0f));
    GL.LoadMatrix(Matrix4.Transpose(lookAt).Matrix);

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

## Shutdown

Finally, when your program is done, you should delete your buffers. Make sure nothing is bound when doing so:

```cs
public override void Shutdown() {
    GL.BindBuffer(BufferTarget.ElementArrayBuffer, 0);
    GL.BindBuffer(BufferTarget.ElementArrayBuffer, 0);

    GL.DeleteBuffer(vertexBuffer);
    GL.DeleteBuffer(indexBuffer);
}
```