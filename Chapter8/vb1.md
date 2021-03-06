# Generating a vertex buffer
You can generate a new vertex buffer with the following functions:

```
int GL.GenBufer();
void GL.GenBuffers(int n, out int buffers);
```

These functions work the same way that generating textures works. And just like with textures, a generated buffer is not active unless it is bound to be the active buffer.

## Binding buffers
You can use the following function to bind your generated buffer ID to an actual buffer:

```
void GL.BindBuffer(BufferTarget target, int buffer);
```

Just like with textures, if your buffer isn't bound, the function calls will not do what you think they will.

The second argument is a buffer id that was previously created. The first argument is what you want to use that buffer as. There are two values we care about:

* __BufferTarget.ArrayBuffer__ is used when you want to specify vertex data such as position, normals, uv's etc...
* __BufferTarget.ElementArrayBuffer__ is used if you are doing indexed rendering. This buffer will contain the indices for you to render.

## Unbind

When you are done drawing with a buffer, you should unbind that buffer. Just like with textures this is done by binding 0 to the active target. Like so:

```cs
GL.BindBuffer(BufferTarget.ArrayBuffer, 0);
GL.BindBuffer(BufferTarget.ElementArrayBuffer, 0);
```