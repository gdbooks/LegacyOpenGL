# Drawing a Point with OpenGL

##Rendering a point
It doesn't get any more primitive then a point, so that's what we should look at first. Drawing a point on screen is actually a really powerful tool, if you can draw a single pixel on screen you can draw anything! With that said, you can draw a point on screen by putting this code in your render function: 

```
GL.Begin(PrimitiveType.Points);
    GL.Vertex3(0.0f, 0.0f, 0.0f);
GL.End();
```

Run this code (Seriously, when i say run this code i mean type it in and try it for yourself) and you will see a tiny white pixel in the middle of your screen.

The first line tells OpenGL that we are about to draw points, by passing the Points primitive type to ```GL.Begin```. The next line tells OpenGL to draw a point at the origin (0, 0, 0). The last line tells OpenGL that we are done drawing.

Note, the indentation on the second line is optional. I indent my code that sits betwel Begin / End calls to make it easyer to read.

What if you want to draw a second point at (0, 1 0)? Well you could type out:

```
GL.Begin(PrimitiveType.Points);
    GL.Vertex3(0.0f, 0.0f, 0.0f);
GL.End();
GL.Begin(PrimitiveType.Points);
    GL.Vertex3(0.0f, 1.0f, 0.0f);
GL.End();
```

However this is very inefficient! Every time you see a Begin / End block that is 1 draw call. The above code uses two draw calls to render two points.

A draw call is when the CPU has to upload data to the GPU. It is an expensive operation, you should aim to have as few draw calls as possible.

Take note that the primitive type passed to Begin is Points, plural. This suggests that between a single begin / end call you can render multiple points, and that is exactly the case.

In the real world, you would render two points like so:

```
GL.Begin(PrimitiveType.Points);
    GL.Vertex3(0.0f, 0.0f, 0.0f);
    GL.Vertex3(0.0f, 1.0f, 0.0f);
GL.End();
```

##Modifying Point Size
OpenGL gives you a great deal of control when rendering trough it's state machine. There are many aspects of a single point you can change. Let's take a look at size. To modify the size of a point you use

```
void GL.PointSize(float size);
```

This results in a square whose width and height are both represented by the size argument. The default size is 1.0 If point antialiasing is disabled (which it is by default) the point size will be rounded to the nearest integer (with a minimum of 1). 

If you want to get the current point size, you can do so with ```GL.GetFloat``` by passing ```GetPName.PointSize``` as it's argument