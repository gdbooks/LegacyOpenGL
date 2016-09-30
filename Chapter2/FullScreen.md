#Full Screen
Most games have the ability to switch between full screen mode and windowed mode. A lot of times this is implemented as a menu option. It used to be standard to toggle fullscreen mod with the ```Alt+Enter shortcut```, however that trent has been slowly dying out. 

How you want to implement your fullscreen toggle logic is up to you, i'm just going to show you how to actually toggle. If you end up accidentally going into fullscreen mode press ```Alt+Tab``` to bring up the program selection toolbox. Navigate to Visual studio (Keep alt pressed down while tapping the tab button to move between programs) and use the red square to terminate the application.

## API
We can switch our game to fullscreen mode by setting a few variables inherited from ```OpenTK.GameWindow```.

First, we need to set the ```WindowBorder``` variable. It is an enum of type ```WindowBorder```. The values that interest us are ```WindowBorder.Resizable``` for a regular window and ```WindowBorder.Hidden``` for a fullscreen window.

Next, we want to set the ```WindowState``` variable. It is an enum of type ```WindowState```. The values that we care about are ```WindowState.Normal``` for windowed mode and ```WindowState.Fullscreen``` for fullscreen mode.

One last thing, when going from fullscreen to windowed mode you need to reset the client size the same way you did when we first created the window:

```cs
ClientSize = new Size(800, 600);
```

Failure to do so will result in undefined behaviour. On my machine the window shrinks by 1 pixel every frame until it has a sie of 0


## Implementation
The first thing we need to do is add a new boolean to the ```MainGameWindow``` class to keep track of the current window mode (windowed or full screen).

```cs
using System;
using OpenTK;
using System.Drawing;
using OpenTK.Graphics.OpenGL;
using GameFramework;

namespace GameApplication {
    class MainGameWindow : OpenTK.GameWindow {
        // reference to OpenTK window
        public static OpenTK.GameWindow Window = null;
        
        // keep track of window mode
        public static bool IsFullscreen = false;
```

Next we implement the actual toggle function

```cs
        public static void ToggleFullscreen() {
            if (IsFullscreen) {
                Window.WindowBorder = WindowBorder.Resizable;
                Window.WindowState = WindowState.Normal;
                Window.ClientSize = new Size(800, 600);
            }
            else {
                Window.WindowBorder = WindowBorder.Hidden;
                Window.WindowState = WindowState.Fullscreen;
            }
            IsFullscreen = !IsFullscreen;
        }
        
        // ... Rest of MainGameWindow class
```

Keep in mind, when exiting fullscreen mode you MUST reset the window to its original size, otherwise the behaviour is undefined. 

That's it. You can now call the ```ToggleFullscreen``` function from a menu, or hook it up to ```Alt+Enter``` your call.