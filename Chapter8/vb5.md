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