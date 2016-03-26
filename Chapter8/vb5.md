#Review

That was a lot of information in a very short ammount of time. Let's real quick review all the things we've learned.

## Initialize

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