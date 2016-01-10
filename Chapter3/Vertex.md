#GL.Vertex
Lets talk about one particularly important function, the ```GL.Vertex``` function. This function needs to be called between ```GL.Begin``` and ```GL.End``` function calls. It is used to specify a point in space (A vertex), which is then interpreted appropriateley depending on the argument of ```GL.Begin```.

The ```GL.Vertex``` function is so important because every object you draw with OpenGL is ultimateley described as an ordered set of vertices.