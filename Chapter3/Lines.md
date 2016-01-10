## Drawing lines in 3D
A line is just a connection between two points. As such, rendering a line is not too different from rendering two points. And because you already know how to do that, lets dive right in:

```
GL.Begin(PrimitiveType.Lines);
    GL.Vertex3(-2.0f, -1.0f, 0.0f);
    GL.Vertex3(2.0f, 1.0f, 0.0f);
GL.End();
```

This time you start off by passing Lines as the primitive type to Begin / End so that OpenGL knows that for every two vertices it needs to draw one line.

Just like with points, you can draw multiple lines within the Begin / End calls. If you wanted to render wo lines, you would need to supply four vertices.

As with points, OpenGL allows you to change several parameters of the state machine that affect how your line is drawn. In addition to setting the line width and setting anti-aliasing you can also specify a stipple pattern.

##Line Width
