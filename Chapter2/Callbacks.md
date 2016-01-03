## Initialize

## Update

#Callbacks
## Render

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