#GL.Vertex
Lets talk about one particularly important function, the ```GL.Vertex``` function. This function needs to be called between ```GL.Begin``` and ```GL.End``` function calls. It is used to specify a point in space (A vertex), which is then interpreted appropriateley depending on the argument of ```GL.Begin```.

The ```GL.Vertex``` function is so important because every object you draw with OpenGL is ultimateley described as an ordered set of vertices.

The version of this function you will use most often is ```GL.Vertex3``` which takes 3 floating point arguments (x, y, z coordinates) you need to get used to this multidimensional notation.

Lets see an example:
```
// Tell OpenGL we want to draw a line
GL.Begin(BeginMode.Lines);

// Tell OpenGL where the first point of the line is
GL.Vertex3(2.0f, 1.0f, 3.0f);

// Tell OpenGL where the second point of the line is
GL.Vertex3(6.0f, -1.0f, 8.0f);

// Tell OpenGL we are done drawing the line
GL.End();
```

Every time you specify a vertex other data becomes associated with it based on the current state of the OpenGL state machine. Data like the color of the vertex, texture and fog. We will cover all of this later, so no need to worry about it right now. The important thing to understand is that none of these things matter without a vertex to use them!