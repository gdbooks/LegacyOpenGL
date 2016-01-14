This is the cube drawing code i used:

```
public override void Render() {
    // Mark modelview matrix as selected
    GL.MatrixMode(MatrixMode.Modelview);

    // Reset modelview matrix
    GL.LoadIdentity();

    // Apply View Matrix
    LookAt(
        0.5f, 0.5f, 0.5f, 
        0.0f, 0.0f, 0.0f,
        0.0f, 1.0f, 0.0f
    );

    // Render grid, it has no model matrix
    grid.Render();

    // Construct a model matrix for the cube
    // Construct this matrix from rotation, translation and scale

    // Translate last
    GL.Translate(0.20f, 0.0f, -0.25f);
    // Rotate second
    GL.Rotate(45.0f, 1.0f, 0.0f, 0.0f);
    GL.Rotate(73.0f, 0.0f, 1.0f, 0.0f);
    // Scale first
    GL.Scale(0.05f, 0.05f, 0.05f);

    // Now that the modelview matrix is correct, render cube
    GL.Color3(1.0f, 0.0f, 0.0f);
    DrawCube();
}
```