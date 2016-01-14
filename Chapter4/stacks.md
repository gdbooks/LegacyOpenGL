# Matrix Stacks
Rendering the cube scene is actually a pretty big deal! We now have a scene with a cube in it! I would call this the start of a game. What if we want to add another cube to our scene to make the game a bit more interesting? 

After we transform the inital cube, we have to reset the modelview matrix to how it was before we rendered the first cube, otherwise the second cube will not render relative to the world origin, but rather relative to the first cube. 

So this is what you would have to do:

* Select modelview matrix
* Load itentity
* Apply view matrix (LookAt)
* Draw grid
* Apply cube 1 model matrix
* Draw cube 1
* Apply inverse of cube 1 model matrix
* Apply cube 2 model matrix
* draw cube 2

This is what the code for the above steps might look like

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

    // Construct the model matrix for first cube
    GL.Translate(0.20f, 0.0f, -0.25f);
    GL.Rotate(45.0f, 1.0f, 0.0f, 0.0f);
    GL.Rotate(73.0f, 0.0f, 1.0f, 0.0f);
    GL.Scale(0.05f, 0.05f, 0.05f);

    // Render first cube
    GL.Color3(1.0f, 0.0f, 0.0f);
    DrawCube();

    // UNDO model matrix for first cube
    GL.Translate(-0.20f, -0.0f, 0.25f);
    GL.Rotate(-45.0f, 1.0f, 0.0f, 0.0f);
    GL.Rotate(-73.0f, 0.0f, 1.0f, 0.0f);
    GL.Scale(20.0f, 20.0f, 20.0f);

    // Construct model matrix for second cube

    // Now that the modelview matrix is correct, render cube
    GL.Translate(-2.5f, 0.0f, 0.25f);
    GL.Rotate(33.0f, 0.0f, 0.0f, 1.0f);
    GL.Rotate(97.0, 0.0f, 1.0f, 0.0f);
    GL.Scale(0.05f, 0.05f, 0.05f);
    GL.Color3(0.0f, 1.0f, 0.0f);
    DrawCube();
}
```

Study the code, if you run it you will see two cubes, one red and one green. This code does work!

You might be thinking to yourself, i see that applying the inverse of cube 1's model matrix just sets the _modelview_ matrix back to the view. Instead of those 4 lines, why don't i just call LoadIdentity and LookAt again?

That logic is sound. Doing that would work, and you'd make your program a bit more readable. But that's not a maintainable solution! As soon as you have nested objects that approach breaks.

We can't stay with the first approach either! It's verbose, it's messy and it has the potential to introduce a lot of floating point error.