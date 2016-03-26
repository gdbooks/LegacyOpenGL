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