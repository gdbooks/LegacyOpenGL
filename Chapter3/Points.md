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

If you want to get the current point size, you can do so with ```GL.GetFloat``` by passing ```GetPName.PointSize``` as it's argument.

Here is an example of how this could be used:
```
float oldSize = GL.GetFloat(GetPName.PointSize);
// if a point was small, make it big. Otherwise make it 1!
if (oldSize < 1.0f) {
    GL.PointSize(5.0f);
}
else {
    GL.PointSize(1.0f);
}
```

##Antialiasing points
Altough you can specify primitives with almost infinite precision, there are a finite number of pixels on screen. This causes the edges of those primitives to look jagged. Anti-Aliasing is a method to smooth those edges, giving the polygon a more natural look.

You can enable anti-aliasing by passing ```EnableCap.PointSmooth``` to ```GL.Enable```. You can disable it by passing the same paramater to ```GL.Disable```. If you are unsure if point smoothing is currently enabled in the state machine, use ```GL.IsEnabled``` with the same argument.

```
// If anti-aliasing is disabled, enable it
if (!GL.IsEnabled(EnableCap.PointSmooth)) {
    GL.Enable(EnableCap.PointSmooth);
}
```

When anti-aliasing is enabled, not all pixel sizes may benefit. The specs say only a point size of 1.0 is guaranteed to get anti-aliased. Other sizes depend on your graphics card and OpenGL driver.

When anti aliasing is on, the current point size is used as the diameter of a circle, centered around the vertex.

##Effect of distance
Normally points always occupy the same amount of space onscreen, regardless of how far away they are from the viewer. For some applications (particles, stars) points need to be larger if they are closer, and smaller if they are farther. You can do this with the GL.PointParamater function. The signature looks like this

```
void GL.PointParameter(PointParameterName param, int value);
void GL.PointParameter(PointParameterName param, int[] value);
void GL.PointParameter(PointParameterName param, float value);
void GL.PointParameter(PointParameterName param, float[] value);
```

The following are valid arguments:
* __PointSizeMin__ Sets the lower bounds OpenGL will scale a point to, second argument is a float.
* __PointSizeMax__ Sets the upper bounds OpenGL will scale a point to, second argument is a float.
* __PointDistanceAttenuation__ Taes an Array of 3 floats that corespond to a, b, c of the attenuation algorithm ```1 / (a + b * d + c * (d * d))```
* __PointFadeThreshold__ Takes a single float, points smaller than this will start to fade out

## Example
Follow along with this example, see what the results look like on your screen when you add this code to your render block:

```
float pointSize = 0.5f;
for (float pointPosition = -1.0f; pointPosition < 1.0f; pointPosition += 0.25f) {
    GL.PointSize(pointSize);

    GL.Begin(PrimitiveType.Points);
        GL.Vertex3(pointPosition, 0.0f, 0.0f);
    GL.End();

    pointSize += 1.0f;
}
```

## Space
__This is very important__

Take note of where the points on screen are rendered. In the middle. This is because we start X at -1.

In OpenGL, __Point(0, 0)__ is the middle of the screen. The left size is __X: -1__ the right side is __X: 1__. Similarly the top is __Y: 1__ and the bottom is __Y: -1__. Z also ranges from -1 to 1. 

Another way to think about this, OpenGL draws inside of a cube. The far left corner of the cube is at (-1, -1, -1), the far right corner is at (1, 1, 1). These coordinates are called __Normalized Device Coordinate__s or __NDC__ for short.