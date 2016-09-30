## The ModelView Matrix
The __modelView__ matrix defines the coordinate system that is used to place and orient objects.
This 4x4 matrix can either transform vertices, or it can be transformed its-self by other matrices. 

Before you can do anything, you must specify if you are going to be working with the __modelView__ or __projection__ matrix. You can do this with the ```GL.MatrixMode``` function. This is the signature:

```
void GL.MatrixMode(MatrixMode);
```

The ```MatrixMode``` enum has two values we use, but the enum its-self has 5 values
* MatrixMode.Modelview
* MatrixMode.Projection
* MatrixMode.Color - we don't use this
* MatrixMode.Texture - we don't use this
* MatrixMode.Modelview0Ext

Usually (99% of the time) you want to set your modelView matrix to identity. So that you start around origin. you can achieve this by calling the ```GL.LoadIdentity``` function. This function loads the identity matrix into the OpenGL state machines active matrix. (You specified the active matrix with ```GL.MatrixMode```)

This snippet resets the modelview matrix:
```
GL.MatrixMode(MatrixMode.Modelview);
GL.LoadIdentity(); // Reset modelview matrix
// Do other transformation
```

## Multiplying OpenGL matrices
We're about to talk about 3 functions:

* ```GL.Translate```
* ```GL.Rotate```
* ```GL.Scale```

Before we discuss how these functions work, i want you to be aware of what they actually do. When you call ```GL.MatrixMode``` with ```MatrixMode.Modelview``` as an argument, you select the __modelView__ matrix of the OpenGL state machine as the active matrix.

Once you have a matrix set as the active matrix, all further matrix operations will be performed to that matrix. ```GL.LoadIdentity``` is one such operation. It takes the current matrix and sets it to identity.

The three functions above will generate an appropriate matrix and multiply the current matrix! ```GL.Rotate``` for instance will make a rotation matrix and multiply the current matrix by it.

The following OpenGL snippet

```
// Set the currently active matrix
GL.MatrixMode(MatrixMode.Modelview);

// Reset modelview matrix
GL.LoadIdentity();

// Create a rotation matrix
// then multiply the active matrix by it
GL.Rotate(90.0f, 0.0f, 0.0f, 1.0f);
```

Is the equivalent of:

```
void Render(ref Matrix4 modelView, ref Matrix4 projection) {
    // Set the currently active matrix
    Matrix4 currentMatrix = modelView;
    
    // Reset modelview matrix
    for (int i = 0; i < 4; ++i) {
        for (int j = 0; j < 4; ++j) {
            currentMatrix[i, j] = (i == j)? 1.0f : 0.0f;
        }
    }
    
    // Create a rotation matrix
    Matrix4 rotation = Matrix4.AngleAxis(90.0f, 0.0f, 0.0f, 1.0f);
    
    // then multiply the active matrix by it
    currentMatrix = rotation * currentMatrix;
}
```

OpenGL just wraps all the math up for you. But the following function create and multiply matrices for you!