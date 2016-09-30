# Structuring your application

Wow, almost done with this chapter! The last thing we need to discuss before wrapping things up is where to put what functions. You may have noticed that the width and height of your window does not change, therefore neither does your projection matrix. Is there any reason to re-calculate it every frame? Not really. So where should this calculation take place?

You only need to set the viewport, and the projection matrix when the window is created or resized. Further-more so long as you set the matrix mode to modelview every time after setting the projection matrix you will not need to select a matrix mode in your render function.

The view matrix by definition needs to be re-calulated each frame. And so does the model matrix. Let's take a look at how all of this fits into your application.

The first thing to do is to add a ```Resize``` function to your __Game.cs__ class. This will be where we set our projection:

```
namespace GameApplication {
    class Game {
        public virtual void Initialize() {

        }
        public virtual void Update(float dTime) {

        }
        public virtual void Render() {

        }
        public virtual void Shutdown() {

        }
        public virtual void Resize(int width, int height) {

        }
    }
}
```

Make sure to call the function in __Program.cs__, like so

```
protected override void OnResize(EventArgs e) {
    base.OnResize(e);
    Rectangle drawingArea = ClientRectangle;
    TheGame.Resize(drawingArea.Width, drawingArea.Height);
}
```

We want to make sure that a default projection matrix is set as soon as the window appears, so we need to change the main function in __Program.cs__ to manually call resize as soon as the game is created.

```
[STAThread]
public static void Main(string[] args) {
    //create new window
    Window = new MainGameWindow();
    Axiis = new Grid();
    TheGame = new MrRoboto();
    // ----------------------------------------
    // v This is new
    TheGame.Resize(Window.Width, Window.Height);
    // ^ This is new
    // ----------------------------------------
    Window.Load += new EventHandler<EventArgs>(Initialize);
    Window.UpdateFrame += new EventHandler<FrameEventArgs>(Update);
    Window.RenderFrame += new EventHandler<FrameEventArgs>(Render);
    Window.Unload += new EventHandler<EventArgs>(Shutdown);

    Window.Title = "Game Name";
    Window.ClientSize = new System.Drawing.Size(800, 600);
    Window.VSync = VSyncMode.On;
    //run 60fps
    Window.Run(60.0f);

    //Dispose at end
    Window.Dispose();
}
```

And that's all the infrastructure support we need. From now on, when you make a new demo scene, structure it like this:

```
using OpenTK.Graphics.OpenGL;

namespace GameApplication {
    class SimpleScene : Game {
        public override void Resize(int width, int height) {
            GL.MatrixMode(MatrixMode.Projection);
            GL.Viewport(0, 0, width, height);
            // TODO: Set projection matrix
            GL.MatrixMode(MatrixMode.Modelview);
        }

        public override void Initialize() {

        }

        public override void Update(float dTime) {

        }

        public override void Render() {
            // TODO: Set view matrix
            GL.PushMatrix();
            // TODO: Render Scene
            GL.PopMatrix();
        }

        public override void Shutdown() {

        }
    }
}
```
