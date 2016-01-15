# Mr.Roboto
Now that we can set the perspetive, model-view matrix and can position different elements using push and pop matrix lets make a complex scene. Instead of just drawing a few boxes, we're going to draw a box robot. At that, one that moves!

The final project will look something like this:

# TODO: IMAGE

So, lets get started! Make a new demo scene that extends the ```Game``` class. Add the DrawCube, LookAt and Perspective helper functions to this class. Also, add a grid.

We're ging to add 8 floats to this class. The first 4 are the angles for the arms and legs of the robot. The last 4 are the direction in which the arms / legs are moving!

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using OpenTK.Graphics.OpenGL;

namespace GameApplication {
    class MrRoboto : Game {
        Grid grid = null;

        float leftArmRot = -15.0f;
        float rightArmRot = 15.0f;
        float leftLegRot = 15.0f;
        float rightLegRot = -15.0f;

        float leftArmDir = 1.0f;
        float rightArmDir = -1.0f;
        float leftLegDir = -1.0f;
        float rightLegDir = 1.0f;

        public override void Initialize() {
            grid = new Grid();
            // THIS CALL IS NEEDED
            // We will discuss what it does later!
            GL.Enable(EnableCap.DepthTest);
        }

        protected static void LookAt(float eyeX, float eyeY, float eyeZ, float targetX, float targetY, float targetZ, float upX, float upY, float upZ) {
            // copy / paste
        }

        public static void Perspective(float fov, float aspectRatio, float znear, float zfar) {
            // copy / paste
        }

        public static void DrawCube() {
            // copy / paste
        }

        public override void Update(float dTime) {
            // todo
        }

        public override void Render() {
            // todo
        }
    }
}
```

The only thing to note in the above code is the ```GL.Enable(EnableCap.DepthTest)``` call. We have not discussed what that does yet, we will in a later chapter. For now, just turn it on, it's needed for the game to run.

#Update
Next up, lets animate those rotation variables! In the update we're going to animate the rotation of each limb by 50 degrees / second. If a limb's rotation is > +20 or < -20 we will reverse it's direction

```
public override void Update(float dTime) {
    // Update rotations
    leftArmRot += 50.0f * dTime * leftArmDir;
    rightArmRot += 50.0f * dTime * rightArmDir;
    leftLegRot += 50.0f * dTime * leftLegDir;
    rightLegRot += 50.0f * dTime * rightLegDir;

    // Clamp & change direction at edge
    if (leftArmRot > 20.0f || leftArmRot < -20.0f) {
        // Clamp to -15 or +15, depending on if the number
        // is negative or positive
        leftArmRot = (leftArmRot < 0.0f) ? -20.0f : 20.0f;
        // Change direction
        leftArmDir *= -1.0f;
    }
    
    if (rightArmRot > 20.0f || rightArmRot < -20.0f) {
        rightArmRot = (rightArmRot < 0.0f) ? -20.0f : 20.0f;
        rightArmDir *= -1.0f;
    }

    if (leftLegRot > 20.0f || leftLegRot < -20.0f) {
        leftLegRot = (leftLegRot < 0.0f) ? -20.0f : 20.0f;
        leftLegDir *= -1.0f;
    }

    if (rightLegRot > 20.0f || rightLegRot < -20.0f) {
        rightLegRot = (rightLegRot < 0.0f) ? -20.0f : 20.0f;
        rightLegDir *= -1.0f;
    }
}
```