# Getting a window
The ```OpenTK``` namespace contains the ```GameWindow``` class. OpenTK can either manage the game window for you, or be embedded in a windows forms application. For the sake of speed, we will let OpenTK create and manage the main window for us. This is the minimum code to get a window up and running:

```cs
using System;
using OpenTK;
using OpenTK.Input;
using System.Drawing;
using GameFramework;

namespace GameApplication {
    class Window {
        // global reference
        public static OpenTK.GameWindow Window = null; 

        [STAThread]
        public static void Main() {
            //create static (global) window instance
            Window = new OpenTK.GameWindow();

            //run game at 60fps. will not return until window is closed
            Window.Run(60.0f);

            Window.Dispose();
        }
    }
}

```