#Fill Data

Now that you have generated a new vertex buffer, and you bound it it's time to fill it with data. Just like with textures, once you fill the buffer object with data you no longer need to keep a CPU copy of that data. This is because the data we put into the vertex buffer will be managed by OpenGL.

You fill a buffer with data using the following function:

```
void GL.BufferData(BufferTarget target, System.IntPtr size, float[] data, BufferUsageHint usage);
```

The first argument is super important, it tells OpenGL WHAT the __currently bound__ buffer will be used for. There are two enum values we care about here:

* __BufferTarget.ArrayBuffer__ contains a large array of floating point values. These are your positions, normals and uv's
* __BufferTarget.ElementArrayBuffer__ contains a large array of unsigned integers. This is used to index your geometry if you want to render with ```GL.DrawElements```.

The second argument is misleading. Even tough it's technically a pointer, the value of that pointer should be an integer, representing how many bytes are to be uploaded to the GPU. It's used like this:

```
new System.IntPtr(cubeData.Length * sizeof(float))
```

The third argument is either a ```float``` or a ```uint``` array. This of course depends on the first argument. It's a large array, while you could fill it with interleaved data, i suggest filling it with consecutive data segments.

* __interleaved data__ is just data with a stride. That is the memory is laid out as: position, normal, uv, position, normal, uv, position, normal, uv....
* __consecutive data__ is laid out linearly. You have all your positions at the front of the array, then all your normals, then all your uvs, like so: position, position, position, normal, normal, normal, uv, uv, uv

Using consecutive data helps speed up the rendering process just a little bit, and makes it easyer to write the actual render code.

