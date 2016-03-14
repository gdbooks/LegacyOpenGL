# Vertex Arrays

Thus far, all examples have used the```GL.Begin``` / ```GL.End``` method of drawing objects. This method is  refered to as __Immediate Mode Rendering__.  Immediate mode is useful for simple applications and prototype code as it is easy to understand and visualize. However it comes with some performance penalties that make it less useful for applications that need to render lots of geometry at a high framerate, like games.

For example, let's say you're rendering a model containing 2,000 lit and textured triangles using immediate mode. Assume that you're able to pack all of the vertex data into a single triangle strip (This is the best case scenario, it's often not possible). Your rendering code _might_ look like this:

```cs
GL.Begin(PrimitiveType.TriangleStrip);

for (int i = 0; i < 2002; ++i) { // 2002 because of the triangle STRIP
  GL.Normal3(normals[i][0], normals[i][1], normals[i][2]);
  GL.TexCoord2(uvs[i][0], uvs[i][1]);
  GL.Vertex3(verts[i][0], verts[i][1], verts[i][2]);
}

GL.End();
```

There are several problems here, the first of which is that in order to render this model we make over 6000 function calls! Remember, no function call is free! Every function call has a very tiny overhead, but with over 6000 calls, this overhead adds up really fast!

The second and third problems are illustrated in this image:

![ISSUE](issue.png)

Assuming that this mesh is made up of triangle strips (perhaps it's a portion of the mesh created in pseudo-code above), each circled vertex is redundant. In other words, these vertices are shared by more than 3 triangles, but since a triangle strip can represent at most three triangles per vertex, each of the circled vertices have to be sent to the graphics card more than once.

This results in additional bandwith when uploading data to the video card. More importantly, those vertices will need to be transformed and lit multiple times, once for each duplicate vertex.

To address these issues OpenGL provides vertex arrays. A Vertex array has the following advantages:

* Large batches of data can be sent with a small number of function calls. 
  * Using vertex arrays we could reduce the above example from 6000 function calls to 2
* Through the use of indexed vertex arrays, vertices can be sent exactly once per triangle mesh, reducing bandwith and lighting calculations
  * This is the reason 90% of the things we render are triangles, not quads or triangle strips
  * When rendered with indexed arrays we can avoid sending duplicate data

Moving forward, whenever possible i want you to avoid immediate mode rendering and only use vertex arrays. It's the only viable way to render and maintain high framerates. No professional game ships with immediate mode, it's only used to prototype.

Now that we've discussed the reasons for using vertex arrays, let's see how they are used. 

## Array based data

So far, we've been using relatively simple objects in our demos, and this, we've been able to describe them explicitly in the code. In a real game, however, you'll be working with models containing hundreds or even thousands of polygons, and describing such complicated models directly in code isn't practical. Instead, one of the following two approaches is usually taken:

* __Load the model from a file__ Dozens of great modeling packages enable you to create a model visually, and then export the geometric data to a file which can be read by your program. This approach offers the greatest flexibility. Model loading will be discussed later.
* __Generate the model procedurally__. Some things you want to represent can be implicitly described with equations due to patterns they contain, or because they posses some random proerties you can generate on the flu. A good example of this is fractials, or a sphere.

Whichever approach is used, it should be fairly obvious that you don't want to repeat all the work every frame. You certainly dont want to be constantly loading or generating a model. Instead you want to load the data into arrays in initialize, and then just use those arrays in the render function. 

This is the thing with vertex arrays, the data is stred in LARGE floating point arrays. Perhaps arrays of 2 to 6 thousand elements.

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

## Working with arrays

After you have enabled the array types that you will be using, the next step is to give OpenGL some data to work with. It's up to you to create arrays and fill them with the data you will be using. After you have filled the arrays with data, you need to tell OpenGL about these arrays so it can draw them. The function used to do this depends on the type of array you're using, let's look at each function in detail:

# TODO