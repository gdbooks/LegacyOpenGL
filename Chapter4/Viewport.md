#Viewport

Some projection functions we just discussed are closely related to the viewport size. Take Perspective for example, we had to divide the viewport width and height. We know that the viewport transformation happens after the projection transformation, so now is as good a time as any to discuss it.

In essence the viewport specifies the dimensions and orientation of the 2D window on which you will be rendering. You can set it with:

```
void GL.Viewport(int x, int y, int width, int height);
```

x and y specify the coordinates of the __lower left__ corner of the viewprot. Width and height specify window size in pixels. 

The reson our windows didn't look the same up until this point is because we have had different default viewport values set. Once you are setting your own values for this our windows will look the same.

Let's take the scene we rendered in the Projections section, and manually specify a viewport for it

```
public override void Render() {
    GL.Viewport(0, 0, MainGameWindow.Window.Width, MainGameWindow.Window.Height);

    GL.MatrixMode(MatrixMode.Projection);
    GL.LoadIdentity();
    float aspect = (float)MainGameWindow.Window.Width / (float)MainGameWindow.Window.Height;
    Perspective(60.0f, aspect, 0.1f, 100.0f);

    GL.MatrixMode(MatrixMode.Modelview);
    GL.LoadIdentity();
    LookAt(
        10.0f, 10.0f, 10.0f,
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