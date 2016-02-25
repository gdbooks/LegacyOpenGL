# Putting it all together

Now we know everything we need to know to make a simple textured scene! Nothing left to do except actually make something textured! In this section i'm going to walk you trough setting up a simple textured scene, but the actual texturing work will be up to you.

## Putting together the test scene

The name of this new test scene will be ```TexturedPlanes```, so create it in a file called **TexturedPlanes.cs**. The initialize function for now will not need to do anything. Neither will the shutdown.

```cs
using System;
using System.Drawing;
using System.Collections.Generic;
using OpenTK.Graphics.OpenGL;
using Math_Implementation;

namespace GameApplication {
    class TexturedPlanes : LightingExample {
        public override void Initialize() {
            base.Initialize();
        }
        
        public override void Shutdown() {
            base.Shutdown();
        }
```

The render function is going to set the camera at position (-7, 5, -7), looking at point (0, 0, 0). So we are going to load the appropriate view matrix. Then we're going to render our standard grid background.

```cs
        public override void Render() {
            Matrix4 lookAt = Matrix4.LookAt(new Vector3(-7.0f, 5.0f, -7.0f), new Vector3(0.0f, 0.0f, 0.0f), new Vector3(0.0f, 1.0f, 0.0f));
            GL.LoadMatrix(Matrix4.Transpose(lookAt).Matrix);

            GL.Disable(EnableCap.DepthTest);
            GL.Disable(EnableCap.Lighting);
            grid.Render();
            GL.Enable(EnableCap.Lighting);
            GL.Enable(EnableCap.DepthTest);

        }
```

Finally the resize function is going to set the viewport and load an updated projection matrix, then set the active matrix back to the modelview matrix.

```cs
        public override void Resize(int width, int height) {
            GL.Viewport(0, 0, width, height);
            GL.MatrixMode(MatrixMode.Projection);
            float aspect = (float)width / (float)height;
            Matrix4 perspective = Matrix4.Perspective(60.0f, aspect, 0.01f, 1000.0f);
            GL.LoadMatrix(Matrix4.Transpose(perspective).Matrix);
            GL.MatrixMode(MatrixMode.Modelview);
        }
    }
}
```

At this point, the test scene should look like this:

![TEX1](tex1.png)

## On your Own

## Adding some detail

## On Your Own