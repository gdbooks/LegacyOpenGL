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

The new thing to note here is the ```viewMatrix``` matrix. We use it to set the view when rendering. Right now it's set to identity, so you should see almost nothing when rendering. We're going to be updating this based on the move code.

## Variables

Let's start by adding some class variables.

```cs
protected float Yaw = 0f;
protected float Pitch = 0f
protected Vector3 CameraPosition = new Vector3(0, 0, 10);
protected Vector2 LastMousePosition = new Vector2();
/*Already exists*/ protected Matrix4 viewMatrix;
```

Yaw and Pitch are the Y and X rotation of the camera respectivley. This represents the cameras orientation in the world.

When we're talking about orientation, we use the terms yaw, pitch and roll. You multiply these together to get an orientation. Order matters!

```
orientation = roll * pitch * yaw;
```

![PRY.gif](PRY.gif)

Next we've added a Vector3 to represent the cameras position in the world. We're going to start the camera off at 10 units in the Z axis. If we started it at 0 it would start INSIDE the 3D model, instead we want to be looking at it.

Last we need to add a Vector2 to maintain the last position of the mouse. We need this because we have to calculate the delta movement of the mouse. Depending on the input library you are using this might not be needed, some input handlers will have a ```GetMouseDelta``` function. OpenTK by default does not.

And of course the ```viewMatrix``` variable was already there in the skeleton framework.

## The Camera

We're going to implement our FPS camera in a helper function. I've documented this function using comments, be sure to read them!

```cs
// Returns the view matrix. Takes in delta time and a movement speed.
Matrix4 Move3DCamera(float timeStep, float moveSpeed = 10f) {
    // Helper variables, we need to know the mouse and keyboard states
    const float mouseSensitivity = .01f;
    MouseState mouse = OpenTK.Input.Mouse.GetState();
    KeyboardState keyboard = OpenTK.Input.Keyboard.GetState();
    
    // Figure out the delta mouse movement
    Vector2 mousePosition = new Vector2(mouse.X, mouse.Y);
    var mouseMove = mousePosition - LastMousePosition;
    LastMousePosition = mousePosition;

    // If the left button is pressed, update Yaw and Pitch based on delta mouse
    if (mouse.LeftButton == ButtonState.Pressed) {
        Yaw += mouseSensitivity * mouseMove.X;
        Pitch -= mouseSensitivity * mouseMove.Y;
        if (Pitch < -90f) {
            Pitch = 90f;
        }
        else if (Pitch > 90f) {
            Pitch = 90f;
        }
    }
    
    // Now that we have yaw and pitch, create an orientation matrix
    Matrix4 pitch = Matrix4.XRotation(Pitch);
    Matrix4 yaw = Matrix4.YRotation(Yaw);
    Matrix4 orientation = /*roll * */ pitch * yaw;

    // Update the position vector based on WASD
    if (keyboard[OpenTK.Input.Key.W]) {
        CameraPosition += new Vector3(0f, 0f, -1f) * moveSpeed * timeStep;
    }
    if (keyboard[OpenTK.Input.Key.S]) {
        CameraPosition += new Vector3(0f, 0f, 1f) * moveSpeed * timeStep;
    }
    if (keyboard[OpenTK.Input.Key.A]) {
        CameraPosition += new Vector3(-1f, 0f, 0f) * moveSpeed * timeStep;
    }
    if (keyboard[OpenTK.Input.Key.D]) {
        CameraPosition += new Vector3(1f, 0f, 0f) * moveSpeed * timeStep;
    }
    
    // Now that we have a position vector, make a position matrix
    Matrix4 position = Matrix4.Translate(CameraPosition);
    // Using position and orientation, get the camera in world space
    Matrix4 cameraWorldPosition = orientation * position;
    // The view matrix is the inverse of the cameras world space matrix
    Matrix4 cameraViewMatrix = Matrix4.Inverse(cameraWorldPosition);

    return cameraViewMatrix;
}
```

The function is verbose, but it's pretty simple. Following the steps outlined below you can construct just about any kind of camera.

Update pitcha and yaw by the mouse delta position. Because Pitch looks up and down, clamp it to the -90 to 90 range. Once we have these, construct a new orientation.

Update the position vector based on the WASD key states. Once we have an updated position, make a position matrix.

Once we have a position and orientation matrix we can figure out where the camera is in world space.

Once you know where the camera is in world space, the view matrix is just the inverse of that.

## Applying the camera

Now that we have the code to move our camera in 3D, we still need to call it.

```cs
public override void Update(float dTime) {
    viewMatrix = Move3DCamera(dTime);
}
```

With that, go ahead and run the application. You should be able to move with WASD, and when you click your mouse button, dragging it should look around the screen.

You can adjust the mouse sensitivity using the ```mouseSensitivity``` constant in the move function. If the WASD movement is too slow, you can adjust it using the functions second argument.