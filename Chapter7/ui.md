# Textures for UI
Texturing 3D objects is cool, but one of the most often used places for textures is UI. In this section i'm going to walk you trough how to add ui to any scene. Then, in the on your own section you will implement some UI on top of the 3D textured test scene we've been working with so far.

## Multiple coordinates

UI relies on using multiple coordinates. After all, if you have a world with a perspective camera, you don't want your UI to be perspective. Unless some UI is in world space (Like the health bar of an RTS game) the UI should be in an orthographic projection.

We not only give the UI it's own projection matrix, we also give it it's own modelview matrix. This allows us to work easily in ui space. Generally, the code for UI will go something like this:

```cs
void Render() {
// FIRST, We render the 3D scene!
    
    // Assume the matrix mode is ModelView
    // Load world modelview matrix for the 3D scene
    Matrix4 lookAt = Matrix4.LookAt(new Vector3(-7.0f, 5.0f, -7.0f), new Vector3(0.0f, 0.0f, 0.0f), new Vector3(0.0f, 1.0f, 0.0f));
    GL.LoadMatrix(Matrix4.Transpose(lookAt).Matrix);
    
    // Render 3D scene as normal
    RenderWorld();
    
// THEN, render the UI

    // Clear only the depth buffer. This way our UI will not z-test against
    // the 3D scene, because the depth buffer will be empty.
    GL.Clear(ClearBufferMask.DepthBufferBit);
    
    // Switch to projection matrix mode, backup 3D projection, load UI projection
    GL.MatrixMode(MatrixMode.Projection); // Switch
    GL.PushMatrix(); // Backup
    GL.Ortho(-1, 1, -1, 1, -10, 10); // Load UI Projection
    
    // Switch back to modelview mode, backup 3D modelview, clear
    GL.MatrixMode(MatrixMode.ModelView); // Switch
    GL.PushMatrix(); // Backup
    GL.LoadIdentity(); // Clear
    
    // Render the UI
    RenderUI();
    
    // Restore world (3D) projection
    GL.MatrixMode(MatrixMode.Projection();
    GL.Pop();
    
    // Restore world (3D) modelview
    GL.MatrixMode(MatrixMode.ModelView();
    GL.Pop();
    
    // We make sure the matrix mode is modelview for the next render iteration
}
```