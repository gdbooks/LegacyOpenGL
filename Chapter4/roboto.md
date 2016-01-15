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

Next up, lets animate those rotation variables!