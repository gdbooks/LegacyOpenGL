# Camera

So far the most interesting camera we've had has been the one rotating around the scene when we want it to. It's ok to look at lighting, but it's not great. In this chapter we're going to go ahead and create a FPS camera.

There are plenty of tutorials out there on this topic, my favorite one is [this](http://in2gpu.com/2016/02/26/opengl-fps-camera/), it's written for C++ OpenGL. But there are a lot of OpenTK specific ones too, like [this one](http://neokabuto.blogspot.com/2014/01/opentk-tutorial-5-basic-camera.html) or [this one](http://www.opentk.com/node/1492?page=1).

Instead of following those tutorials (which can be overcomplicated at times) we're just going to set up our own. Making a camera is simple, you need to first figure out the world position of the camera. For this you need to figure out it's rotation and orientation. You then take the world position and invert it to get a view matrix.

How you figure out the world position of a camera is the difference between the FPS, RTS, 3rd person, etc... camera models. For an FPS camera, the rotation on the Y and X axis updates based on mouse movement, while the position updates based on the WASD keys.

## Framework

I'm going to provide the code for the framework we're going to be working in. It's more or less a copy / paste of the OBJ test scene. Take note of the Using directives, we want to use ```OpenTK.Input``` for mouse / keyboard and ```Math_Implementation``` for Matrix and Vector classes

```cs
using System;
using OpenTK.Graphics.OpenGL;
using Math_Implementation;
using OpenTK.Input;

namespace GameApplication {
    class CameraExample : Game {
        Grid grid = null;
        OBJModel model = null;
        protected Vector3 cameraAngle = new Vector3(0.0f, -25.0f, 10.0f);
        protected float rads = (float)(Math.PI / 180.0f);

        // TODO: Set based on camera input
        protected Matrix4 viewMatrix = new Matrix4();

        public override void Resize(int width, int height) {
            GL.Viewport(0, 0, width, height);
            GL.MatrixMode(MatrixMode.Projection);
            float aspect = (float)width / (float)height;
            Matrix4 perspective = Matrix4.Perspective(60.0f, aspect, 0.01f, 1000.0f);
            GL.LoadMatrix(Matrix4.Transpose(perspective).Matrix);
            GL.MatrixMode(MatrixMode.Modelview);
            GL.LoadIdentity();
        }

        public override void Initialize() {
            GL.Enable(EnableCap.DepthTest);
            GL.Enable(EnableCap.CullFace);
            GL.Enable(EnableCap.Lighting);
            GL.Enable(EnableCap.Light0);

            Resize(MainGameWindow.Window.Width, MainGameWindow.Window.Height);

            grid = new Grid(true);
            model = new OBJModel("Assets/test_object.obj");

            GL.Light(LightName.Light0, LightParameter.Position, new float[] { 0.0f, 0.5f, 0.5f, 0.0f });
            GL.Light(LightName.Light0, LightParameter.Ambient, new float[] { 0f, 1f, 0f, 1f });
            GL.Light(LightName.Light0, LightParameter.Diffuse, new float[] { 0f, 1f, 0f, 1f });
            GL.Light(LightName.Light0, LightParameter.Specular, new float[] { 1f, 1f, 1f, 1f });
        }

        public override void Shutdown() {
            model.Destroy();
        }

        public override void Update(float dTime) {
            // TODO: Move 3D camera
        }

        public override void Render() {
             GL.LoadMatrix(Matrix4.Transpose(viewMatrix).Matrix);

            GL.Disable(EnableCap.Lighting);
            GL.Disable(EnableCap.DepthTest);
            grid.Render();
            GL.Enable(EnableCap.DepthTest);
            GL.Enable(EnableCap.Lighting);

            GL.Color3(1f, 1f, 1f);
            model.Render(true, false);
        }
    }
}
```