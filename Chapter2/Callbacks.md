#Callbacks
In OpenTK you don't have access to the windows message loop. Instead OpenTK provides a number of very useful callbacks that you can use to do things during the lifecycle of the window. Set all of the callbacks before calling the ```Run``` method of the window.

## Initialize
The initialize function gets called right after the window is created, before the windows first update / render cycle. 

The function takes a sender ```object``` and a ```EventArgs``` event, it returns void. This is how you can hook up the ```Initialize``` callback:

```cs
using System;
using OpenTK;
using OpenTK.Input;
using System.Drawing;

namespace GameApplication {
    class Window {
        //reference to OpenTK window
        public static OpenTK.GameWindow Window = null; 

        public static void Initialize(object sender, EventArgs e) {
            // Do initialization code here
        }
        
        [STAThread]
        public static void Main() {
            //create static(global) window instance
            Window = new OpenTK.GameWindow();

            //hook up the initialize callback
            Window.Load += new EventHandler<EventArgs>(Initialize);
            
            //set window title and size
            Window.Title = "Game Name";
            Window.ClientSize = new Size(800, 600);

            //run game at 60fps. will not return until window is closed
            Window.Run(60.0f);

            Window.Dispose();
        }
    }
}
```

## Update

The Update function gets called 1 time every frame. The function takes a sender ```object``` and a ```FrameEventArgs``` event, it returns void. You need to hook up to the ```UpdateFrame``` method of the OpenTK window.

You can get the delta time of the update loop trough the ```FrameEventArgs``` argument of the function.

```cs
// ...

namespace GameApplication {
    class Window {
        // ...
        
        public static void Update(object sender, FrameEventArgs e) {
            float deltaTime = (float)e.Time;
        }
        
        [STAThread]
        public static void Main() {
            // ...
            
            Window.UpdateFrame += new EventHandler<FrameEventArgs>(Update);
            
            // ...
            
            Window.Run(60.0f);

            Window.Dispose();
        }
    }
}
```

## Render
The Render function is similar to the Update function. The function gets hooked up to the windows ```RenderFrame``` callback. Just like Update the Render function takes a sender ```object``` and a ```FrameEventArgs``` event, it returns void.

Because the second argument is a ```FrameEventArgs``` event you can access delta time the same way you did for Update if you need it, tough generally in a Render function you wont.

Inside this function we need to do the following
* Tell OpenGL what color to clear the screen to
* Clear the screen
* Render our game
* Swap the back and front display buffers
  * These are managed by OpenTK.
  * OpenTK is by default double buffered.

```cs
// ...

namespace GameApplication {
    class Window {
        // ...
        
        public static void Render(object sender, FrameEventArgs e) {
            GL.ClearColor(clearColor);
            }
            GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
        }
        
        // ..
        
        [STAThread]
        public static void Main() {
            // ...
            
            Window.RenderFrame += new EventHandler<FrameEventArgs>(Render);
            
            // ...
            
            //run game at 60fps. will not return until window is closed
            Window.Run(60.0f);

            Window.Dispose();
        }
    }
}
```

## Shutdown

# All together
The code below puts all of the above callbacks in place. IT also sets the window Title and Size, trough public getters and setters exposed to the ```GameWindow``` class. This should be everything we need for the main window.

If you're curious, a complete list of the callbacks and properties of the ```GameWindow``` class can be found on the [OpenTK Doxygen](http://www.opentk.com/files/doc/class_open_t_k_1_1_game_window.html) documentation page.

```cs
using System;
using OpenTK;
using OpenTK.Input;
using System.Drawing;

namespace GameApplication {
    class Window {
        //reference to OpenTK window
        public static OpenTK.GameWindow Window = null; 

        public static void Initialize(object sender, EventArgs e) {

        }
        public static void Update(object sender, FrameEventArgs e) {
            float deltaTime = (float)e.Time;
        }
        public static void Render(object sender, FrameEventArgs e) {

        }
        public static void Shutdown(object sender, EventArgs e) {

        }
        [STAThread]
        public static void Main() {
            //create static(global) window instance
            Window = new OpenTK.GameWindow();

            //hook up the initialize callback
            Window.Load += new EventHandler<EventArgs>(Initialize);
            //hook up the update callback
            Window.UpdateFrame += new EventHandler<FrameEventArgs>(Update);
            //hook up render callback
            Window.RenderFrame += new EventHandler<FrameEventArgs>(Render);
            //hook up shutdown callback
            Window.Unload += new EventHandler<EventArgs>(Shutdown);

            //set window title and size
            Window.Title = "Game Name";
            Window.ClientSize = new Size(800, 600);
            
            //run game at 60fps. will not return until window is closed
            Window.Run(60.0f);

            Window.Dispose();
        }
    }
}
```