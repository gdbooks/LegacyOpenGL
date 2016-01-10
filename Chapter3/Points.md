# Drawing a Point with OpenGL

It doesn't get any more primitive then a point, so that's what we should look at first. Drawing a point on screen is actually a really powerful tool, if you can draw a single pixel on screen you can draw anything! With that said, you can draw a point on screen by putting this code in your render function: 

```
GL.Begin(PrimitiveType.Points);
GL.Vertex3(0.0f, 0.0f, 0.0f);
GL.End();
```

Run this code (Seriously, when i say run this code i mean type it in and try it for yourself) and you will see a tiny white pixel in the middle of your screen.

The first line tells OpenGL that we are about to draw points, by passing the Points primitive type to ```GL.Begin```. The next line tells OpenGL to draw a point at the origin (0, 0, 0). The last line tells OpenGL that we are done drawing.

