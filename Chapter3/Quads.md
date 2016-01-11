#Quadrilaterals

Quadrilaterals, or quads, are four-sided polygons that can be convenient when you want to draw a square or a rectangle. You create them by passing ```PrimitiveType.Quads``` to ```GL.Begin```, then supplying 4 vertices. Like triangles you can draw mulyiple quads within the same ```GL.Begin```/```GL.End``` block.

OpenGL provides Quad strips as a means of improving the rendering of multiple larger quads, but you will use this feature less than you will use triangle strips. They are kind of useless.

When rendering a quad strip, the first 4 vertices specify the first quad, after that every two vertices will be used with the previous two vertices to create new quads.