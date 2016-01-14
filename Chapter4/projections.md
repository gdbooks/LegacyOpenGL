# Projections
We've mentioned projection transformations several times now, and even used them in code. It's about time we discuss how they work. As we've pointed out there are two general classes of projections available in OpenGL: __orthographic__ (or parallell) and __perspective__.

By setting a projection transform, you are, in effect creating a viewing volume, which serves two purposes. The first is that the viewing volume defines a number of clipping planes, which determine the portion of your 3D world that is visible at any given time. Objects outside the volume are not rendered.

The second purpose of the viewing volume is to determine how objects are draw. This depends on the shape of the viewing volume, which is the primary difference betwen perspective and orthographics render modes.

![PVO](pvo.gif)

Before specifying any king of projection transformation you need to make sure that the projection matrix is the selected matrix stack. As with the modelView matrix, this is done trough the ```GL.MatrixMode``` function:

```
GL.MatrixMode(MatrixMode.Projection);
```

In most cases you want to follow this up with a call to ```GL.LoadIdentity``` to clear out anything that might be stored in this matrix. Then you set the actual matrix. 

Unlike the __modelView__ matrix, it's not likeley that the projection matrix will change much. Usually the only time this changes is when you resize the window.

Lets take a look at how to set the actual matrix

## Orthographic
Orthographics projections are those that involve no perspective correction. In other words, no adjustment is made for the distance of the camera. Objects appear the same size on screen weather they are close up or far away.

OpenGL provides the ```GL.Ortho``` function to set an orthographic projection:

```
void GL.Ortho(float left, float right, float bottom, float top, float near, float far);
```

Left and right specify the X-coordinate clipping planes. Bottom and top specify the Y, and near and far specify the Z. This function essentially builds a box to render things in.

The default values of the orthographic projection are NDC space, or:

```
GL.Ortho(-1, 1, -1, 1, -1, 1);
```

This is cool, but it gives us a few issues. For one, all our geometry has to fit into the -1 +1 range! 

Follow along in a new sample project. Let's use our scene that draws one cube as a starting point:

```
public override void Render() {
    GL.MatrixMode(MatrixMode.Modelview);
    GL.LoadIdentity();

    LookAt(
        0.5f, 0.5f, 0.5f, 
        0.0f, 0.0f, 0.0f,
        0.0f, 1.0f, 0.0f
    );
    grid.Render();

    GL.Translate(0.20f, 0.0f, -0.25f);
    GL.Rotate(45.0f, 1.0f, 0.0f, 0.0f);
    GL.Rotate(73.0f, 0.0f, 1.0f, 0.0f);
    GL.Scale(0.05f, 0.05f, 0.05f);

    GL.Color3(1.0f, 0.0f, 0.0f);
    DrawCube();
}
```

Now, the projection is the last step in the transform pipeline, because of that, it's the first thing we should multiply! Let's set a projection that's 5 times bigger than the default NDC space!

```
public override void Render() {
    GL.MatrixMode(MatrixMode.Projection);
    GL.LoadIdentity();
    GL.Ortho(-5, 5, -5, 5, -5, 5);

    GL.MatrixMode(MatrixMode.Modelview);
    GL.LoadIdentity();

    LookAt(
        0.5f, 0.5f, 0.5f,
        0.0f, 0.0f, 0.0f,
        0.0f, 1.0f, 0.0f
    );
    grid.Render();

    GL.Translate(0.20f, 0.0f, -0.25f);
    GL.Rotate(45.0f, 1.0f, 0.0f, 0.0f);
    GL.Rotate(73.0f, 0.0f, 1.0f, 0.0f);
    GL.Scale(0.05f, 0.05f, 0.05f);

    GL.Color3(1.0f, 0.0f, 0.0f);
    DrawCube();
}
```

Run that code, all of a sudden you can see a lot more of the scene! Play around with the numbers, try 10, maybe even 15. See what a larger orthographic projection does. 

As your projection matrix gets bigger, you can move your camera to positions other than 0.5f, as they will still hold the scene in View! Experiment a bit, see if you can predict what things will look like. Othro projection can be a bit of a trip!