## Array based data

The biggest challenge for graphics is getting data. So far we've used simple primitives like cubes and planes. These are easy to describe directly in code using ```GL.Vertex3```. Complicated models however will not be easily described in code.

Most games use two approaches to solve this. First, it's possible to __generate gemetric data procedurally__. This just entails running some algorithm that will eventually spit out vertices to 3D geometry. We render the spehere in our demos using this method.

More often than not tough, geometric data will be __loaded from an external file__. Later on we will actually discuss how to load complex 3D models from an OBJ file. There are standard model formats out there, like OBJ, FBX and DAE but most games use some custom format. This custom format tends to be one that simply makes sense to the programmers.ctials, or a sphere.

Whichever approach is used, it should be fairly obvious that you don't want to repeat all the work every frame. You certainly dont want to be constantly loading or generating a model. Instead you want to load the data into arrays in initialize, and then just use those arrays in the render function. 

This is the point of vertex arrays, the data is stored in LARGE floating point arrays. Perhaps arrays of 2 to 6 thousand elements.
