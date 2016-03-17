# GL.DrawElements

This function is very similar to ```GL.DrawArrays```, but it is even more powerful! With ```GL.DrawArrays```, your only option is to draw all vertices in the array sequentially. Meaning you can't reference the same vertex more than once. So ```GL.DrawArrays``` still has the problem of needing duplicate verts.

_```GL.DrawElementson```_ on the other hand allows you to specify the array elements in any order, and access each element (vertex) as many times as needed. Let's take a look at the function prototype:

```
void GL.DrawElements(BeginMode mode, int count, DrawElementsType type, uint indices);
```

__mode__ and __count__ are used the same as in ```GL.DrawArrays```. __type__ is the type of the valies in the indices array, it should be UnsignedByte, UnsignedShort, or UnsignedInt. __indices__ is an array containing indexes for the vertices you want to render.

The last argument for ```GL.DrawElements``` is an array of unsigned integers ```uint[]```. It could also be unsigned byte or short, depending on the __type__ paramater.

To understand the value of this method, it must be reiterated that not only can you specify the indices in any order, you can also specify the same vertex repeatedly in the series. In games, most vertices will be shared by more than one polygon. By storing the vertex once and accessing it repeatedly by it's index, you can save a substantial amount of memory. 

In addition, OpenGL will only do lighting calculations once for each vertex, this means that by re-using vertices you save on performance by not having to repeat the same computation for identical vertices. Remember, lighting is the most expensive part of the pipeline.

In the next section we're going to implement a demo program using ```GL.DrawElements```.