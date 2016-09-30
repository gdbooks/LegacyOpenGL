## Enabling vertex arrays

Like most OpenGL features, to be able to use vertex arrays, you must first enable them. You might expect this to be done with ```GL.Enable```, but it's not. OpenGL provides a seperate pair of functions to control vertex array support:

```cs
void GL.EnableClientState(ArrayCap stateArray);
void GL.DisableClientState(ArrayCap stateArray);
```

The ```stateArray``` parameter is a flag indicating which type of array you're enabling (or disabling). Each type of vertex attribute (position, normal, color, uv) can be stored in an array, you need to enable whichever attributes you are using indevidually. These are the valid flags:

* __ArrayCap.VertexArray__ Enables an array containing the position of each verted
* __ArrayCap.NormalArray__ Enables an array containing the normal of each vertex
* __ArrayCap.ColorArray__ Enables an array containing color information for each vertex
* __ArrayCap.SecondaryColorArray__ Enables an array containing color information for each vertex
* __ArrayCap.IndexArray__ Enabled an array containing indices into a _color pallete_ for each vertex
* __ArrayCap.TextureCoordArray__ Enabled an array containing the uv coordinates for each vertex
* __ArrayCap.EdgeFlagArray__ Enables an array containing an edge flag for each vertex

For example, if you wanted to render a model that has vertex positions, normals and texture coordinates,you'd have to do the following:

```cs
void GL.EnableClientState(ArrayCap.VertexArray);
void GL.EnableClientState(ArrayCap.NormalArray);
void GL.EnableClientState(ArrayCap.TextureCoordArray);

// TODO: Render

void GL.DisableClientState(ArrayCap.TextureCoordArray);
void GL.DisableClientState(ArrayCap.NormalArray);
void GL.DisableClientState(ArrayCap.VertexArray);
```