# Cleanup

Cleaning up vertex buffers is pretty easy, the function call is:

```
void GL.DeleteBuffer(int bufferId);
```

This call signals to OpenGL that it's ok to delete the data in the buffer. After this call i suggest setting bufferId to -1 to signify that it is an invalid buffer. 