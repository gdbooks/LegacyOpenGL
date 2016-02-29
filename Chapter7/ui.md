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
    GL.LoadIdentity(); // Clear
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

That's a LOT of code, but it kind of shows you how rendering the UI is almost an entireley different render path alltogether. It's like rendering an overlay. Perhaps the most important thing when rendering the UI is the call to clear the depth buffer:

```
GL.Clear(ClearBufferMask.DepthBufferBit);
```

This call clears all Z-values that have been written into the depth buffer so far. By doing this, we ensure that our UI never clips against anything in world space. This should look familiar, a similar call is used in Program.cs to clear the depth and color bufers.

After the depth buffer is cleared, we back up the world space projection and reset it to UI space, next we back up the world space view matrix and replace it with the UI view matrix. And that all there is to it, now we just render the UI.

I STRONGLY suggest breaking your scene into two render functions, ```RenderWorld``` and ```RenderUI```, and calling them like i did above. This will keep the main render function ```Render``` of your scene from getting unmaintanable.

## Positioning the UI
Right now positioning the UI is a pain in the ass. Because the screen goes from -1 to 1 we have to figure out how much space a ui element takes up, then normalize that into the screen. We might end up with UI code that looks like this:

```
GL.Begin(PrimitiveType.Quads);
    GL.TexCoord2(0, 1);
    GL.Vertex3(0.1345f, 0.3345f, 0.0f);
    
    GL.TexCoord2(1, 1);  
    GL.Vertex3(0.435f, 0.3345f, 0.0f);
    
    GL.TexCoord2(1, 0);
    GL.Vertex3(0.435f, 0.1245f, 0.0f); 
    
    GL.TexCoord2(0, 0);
    GL.Vertex3(0.1345f, 0.1245f, 0.0f); 
GL.End();
```

And that is kind of awefull! I mean, what if you need to change the screen position later, this becomes a nightmare to maintain. Worse yet, it does not account for aspect ration. Worse, worse yet, because of the aspect ratio error compounded with possible floating point error it's nearly impossible to get a pixel perfect UI.

And that's the 