# Getting a window
Most of this is going to look familiar. We  used OpenTK to manage windows in the OpenTK2DFramework project. This chapter just provides more detailed information about what is happening.

The ```OpenTK``` namespace contains the ```GameWindow``` class. OpenTK can either manage the game window for you, or be embedded in a windows forms application. For the sake of speed, we will let OpenTK create and manage the main window for us. This is the minimum code to get a window up and running:

```cs
using System;
using OpenTK;
using System.Drawing;
using OpenTK.Graphics.OpenGL;

namespace GameApplication {
    class MainGameWindow : OpenTK.GameWindow {
        // global reference
        public static OpenTK.GameWindow Window = null; 

        [STAThread]
        public static void Main() {
            //create static (global) window instance
            // take note, we set make a new MainGameWindow object, which inherits from OpenTK.GameWindow
            Window = new MainGameWindow();

            //run game at 60fps. will not return until window is closed
            Window.Run(60.0f);
            
            // We are finished with the window, clean up memory
            Window.Dispose();
        }
    }
}

```

## What happened?
We created a static variable called ```Window``` of type ```OpenTK.GameWindow```. This is a public static variable to allow global access. If you don't need to access the window from outside your class you could have that variable be local to the Main function. It doesn't have to be static.

The ```Main``` function has to be marked as ```STAThread``` as with other non-form C# applications. 

The first line of ```Main```,  ```Window = new MainGameWindow();``` creates a new window object. When created, the window object allocates memory for the new window and intializes everything the window needs.

Why do we create a ```MainGameWindow``` object instead of a raw ```OpenTK.GameWindow``` object? Because the ```MainGameWindow``` extends the ```OpenTK.GameWindow``` class this will work. For now you could replace that with a ``` new OpenTK.GameWindow()```  call and everything would still work. But later we will need to override methods in the ```OpenTK.GameWindow``` class.

The next line ```Window.Run(60.0f);``` actually shows the window. After the window is shown this method will enter the update loop. The window will try to run at 60FPS if it can. The actual update speed is up to you.

The ```Run``` method of the window only returns when the window is closed. For the duration of the application this method will be executing.

Once the window is closed and ```Run``` returns we have to clean up any memory it allocated and all resources it's eating up. That's what the ```Dispose``` method does.