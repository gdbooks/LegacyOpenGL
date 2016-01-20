# Examples
All examples below assume the following projection matrix:

```
public override void Resize(int width, int height) {
    GL.Viewport(0, 0, width, height);
    //set projection matrix
    GL.MatrixMode(MatrixMode.Projection);
    float aspect = (float)MainGameWindow.Window.Width / (float)MainGameWindow.Window.Height;
    Matrix4 perspective = Matrix4.Perspective(60, aspect, 0.01f, 1000.0f);
    GL.LoadMatrix(Matrix4.Transpose(perspective).Matrix);
    //switch to view matrix
    GL.MatrixMode(MatrixMode.Modelview);
    GL.LoadIdentity();
}
```

## Example 1

## Example 2