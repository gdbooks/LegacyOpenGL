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
    class MainGameWindow {
        //reference to OpenTK window
        public static OpenTK.GameWindow Window = null; 

        public static void Initialize(object sender, EventArgs e) {
            // Do initialization code here
        }
        
        [STAThread]
        public static void Main() {
            //create static(global) window instance
            Window = new MainGameWindow();

            //hook up the initialize callback
            Window.Load += new EventHandler<EventArgs>(Initialize);
            
            //set window title and size
            Window.Title = "Game Name";
            Window.ClientSize = new Size(800, 600);
            
            // turn on vsynch, prevents screen tearing when swapping buffers
            // This is on by default, but take no changes!
            Window.VSync = VSyncMode.On;
            
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
    class MainGameWindow {
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

This code contains the first two lines of OpenGL we are going to write. Don't worry about what they mean yet, for now just copy them in.

```cs
// ...

namespace GameApplication {
    class MainGameWindow {
        public static OpenTK.GameWindow Window = null; 
        
        // ...
        
        public static void Render(object sender, FrameEventArgs e) {
            // Tell OpenGL what color to clear the screen to
            GL.ClearColor(Color.CadetBlue);
            // Tell OpenGL to clear the screen.
            GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
            
            // TODO: Render your game here
            
            Window.SwapBuffers();
        }
        
        // ..
        
        [STAThread]
        public static void Main() {
            // ...
            
            Window.RenderFrame += new EventHandler<FrameEventArgs>(Render);
            
            // ...
            
            Window.Run(60.0f);
            Window.Dispose();
        }
    }
}
```

##Resize
The resize callback is a bit special. Not in the good way. This callback is the whole reason that our ```MainGameWindow``` class has to extend
## Shutdown
The shutdown callback is similar to the initialize callback. The function takes a sender ```object``` and a ```EventArgs``` event, it returns void. You hook shutdown up the the windows ```Unload``` callback

```cs
// ...

namespace GameApplication {
    class MainGameWindow {
        
        // ...
        
        public static void Shutdown(object sender, EventArgs e) {

        }
        
        [STAThread]
        public static void Main() {
            
            // ...
            
            Window.Unload += new EventHandler<EventArgs>(Shutdown);

            // ...
            
            Window.Run(60.0f);
            Window.Dispose();
        }
    }
}
```

# All together
The code below puts all of the above callbacks in place. IT also sets the window Title and Size, trough public getters and setters exposed to the ```GameWindow``` class. This should be everything we need for the main window.

If you're curious, a complete list of the callbacks and properties of the ```GameWindow``` class can be found on the [OpenTK Doxygen](http://www.opentk.com/files/doc/class_open_t_k_1_1_game_window.html) documentation page.

```cs
using System;
using OpenTK;
using OpenTK.Input;
using System.Drawing;

namespace GameApplication {
    class MainGameWindow : OpenTK.GameWindow {
        //reference to OpenTK window
        public static OpenTK.GameWindow Window = null; 

        public static void Initialize(object sender, EventArgs e) {
        
        }
        public static void Update(object sender, FrameEventArgs e) {
            float deltaTime = (float)e.Time;
        }
        public static void Render(object sender, FrameEventArgs e) {
            GL.ClearColor(Color.CadetBlue);
            GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
            
            // TODO: Render your game here
            
            Window.SwapBuffers();
        }
        public static void Shutdown(object sender, EventArgs e) {

        }
        [STAThread]
        public static void Main() {
            //create static(global) window instance
            Window = new MainGameWindow();

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
            
            // turn on vsynch, prevents screen tearing when swapping buffers
            // This is on by default, but take no changes!
            Window.VSync = VSyncMode.On;

            //run game at 60fps. will not return until window is closed
            Window.Run(60.0f);

            Window.Dispose();
        }
    }
}
```