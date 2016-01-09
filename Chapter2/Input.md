#Input
For the sake of completeness i'm going to show you how to read raw input here. While this is usefull, the ```InputManager``` from the ```GameFramework``` still works just fine (Assuming you set it up correctly). I highly suggest using the ```InputManager``` or rolling your own version of it.

After reading this, go ahead and browse trough the ```InputManager``` class to get a better understanding of what it's doing and how it's doing it. The ```InputManager``` is just a convenient abstraction that provides buffered input instead of raw input.

## OpenTK.Input
All input related function reside within the ```OpenTK.Input``` namespace. This namespace has several enumerations and classes we care about:

* OpenTK.Input.Key
  * Enumeration of keyboard keys
* OpenTK.Input.MouseButton
  * Enumeration of mouse buttons
* OpenTK.Input.KeyboardState
  * Structure containing keyboard information 
* OpenTK.Input.MouseState
  * Structure containing mouse  information 

For a compleate overview of the namespace, check out [the doxygen](http://www.opentk.com/files/doc/namespace_open_t_k_1_1_input.html) pages for OpenTK.

## Keyborad Input
In order to read the keyboard, all you need is access to a ```KeyboardState``` instance.
Read [this article](http://www.opentk.com/doc/input/keyboard) before proceeding.

There above article get a ```KeyboardState``` reference using this code:

```cs
KeyboardState state = OpenTK.Input.Keyboard.GetState();
```

However this functionality is deprecated. It might get compleatly removed from future versions of OpenTK. Instead, to get a ```KeyboardState``` instance, you need access to an instance of ```OpenTK.GameWindow``` class. 

Luckly with our windowing code:
```cs
using System;
using OpenTK;
using System.Drawing;
using OpenTK.Graphics.OpenGL;

namespace GameApplication {
    class MainGameWindow : OpenTK.GameWindow {
        // reference to OpenTK window
        public static OpenTK.GameWindow Window = null;
```

The static variable ```MainGameWindow.Window```is accessable from anywhere. So, get a reference to the ```KeyboardState``` structure using the ```Keyboard``` getter of the ```OpenTK.GameWindow``` class. Like so:

```cs
// Call somewhere in the update loop
void CheckInput() {
    KeyboardState keboard = MainGameWindow.Window.Keyboard;
}
```

There is only one property of ```KeyboardState``` that we care about, and that is the fact that the ```[]``` accessor is implemented to return a ```bool```. This override takes a ```OpenTK.Input.Key``` enum value as an argument and returns true if the button is being held down, false if it is up.

This is how you could check if the button A is down

```cs
// Call somewhere in the update loop
void CheckInput() {
    KeyboardState keboard = MainGameWindow.Window.Keyboard;
    
    if (keyboard[OpenTK.Input.Key.A]) {
        Console.WriteLine("A is down");
    }
}
```

We can take advantage of the fact that each enum entry is essentially an integer and print out any key that is down:

```cs
// Call somewhere in the update loop
void CheckInput() {
    KeyboardState keboard = MainGameWindow.Window.Keyboard;
    
    // LastKey is the number of keys on the keyboard + 1
    int numKeys = (int)OpenTK.Input.Key.LastKey;
    
    for (int i = 0; i < numKeys; ++i) {
        OpenTK.Input.Key iAsKey = (OpenTK.Input.Key)i;
        
        if (keyboard[iAsKey]) {
            Console.WriteLine(iAsKey.ToString() + " is down");
        }
    }
}
```

## Mouse Input
Mouse input is similar to the keyboard input. Just like with the keyboard there are some [outdated docs](http://www.opentk.com/doc/input) on the OpenTK site about working with a mouse.

```cs
// Call somewhere in the update loop
void CheckInput() {

}

Again, the first thing you need is an instance of the ```OpenTK.Input.MouseState``` class. You can get this trough the ```Mouse``` getter of the ```OpenTK.GameWindow``` class.

## Gamepad Input
Gamepad support in OpenTK is in a very sad state. It's super duper broken. I'm not even going to cover it here. If you are interested, you can check out the ```InputManager``` code behind joystick support, but it's a verbose ugly hack!

## Buffered Input
Using the ```InputManager``` we can check more states of buttons. For example we don't just have access to a bool indicating if a key is down or up, we can also check if the key was pressed this frame, or released this frame. That's called buffered input, and it's the main high level concept that the ```InputManager``` provides.

If you want to implement your own buffered input, it's not difficult. The same principles that apply to double buffered rendering apply to buffered input:

* Make a front-buffer.
  * Array of bool, size is the number of available keys
  * Will hold the key states of this frame
* Make a back buffer 
  * Array of bool, size is the number of available keys
  * Will hold the key states of the last frame
* In update
  * Copy the contents of the front-buffer INTO the back-buffer
  * Copy the current keyboard state INTO the fron-buffer
* In update, after the input buffers have been updated
  * A key is down if:
    * it is true in the front buffer 
  * A key is up if:
    * it is false in the front buffer 
  * A key was pressed this frame if:
     * it is true in the front-buffer
     * it is false in the back-buffer
  * A key was released this frame if:
    * it is false in the front-buffer
    * it is true in the back-buffer

The code for this might look something like this:
```cs
using System;
using OpenTK;
using System.Drawing;
using OpenTK.Graphics.OpenGL;
using OpenTK.Input;

namespace GameApplication {
    class MainGameWindow : OpenTK.GameWindow {
        public static OpenTK.GameWindow Window = null;
        
        private static bool[] KeyboardFront = null;
        private static bool[] KeyboardBack = null;
        
        public static void Initialize(object sender, EventArgs e) {
            int numKeys = (int)OpenTK.Input.Key.LastKey;

            KeyboardFront = new bool[numKeys];
            KeyboardBack = new bool[numKeys];
            
            for (int i = 0; i < numKeys) {
                KeyboardFront[i] = false;
                KeyboardBack[i] = false;
            }
        }
        
        private static void UpdateInput() {
            int numKeys = (int)OpenTK.Input.Key.LastKey;

            for (int i = 0; i < numKeys; ++i) {
                KeyboardBack[i] = KeyboardFront[i];
            }
            for (int i = 0; i < numKeys; ++i) {
                OpenTK.Input.Key iAsKey = (OpenTK.Input.Key)i;
                KeyboardFront[i] = Window.Keyboard[iAsKey];
            }
        }
        
        private static bool KeyDown(OpenTK.Input.Key key) {
            return KeyboardFront[key];
        }
        
        private static bool KeyUp(OpenTK.Input.Key key) {
            return !KeyboardFront[key];
        }
        
        private static bool KeyPressed(OpenTK.Input.Key key) {
            return KeyboardFront[key] && !KeyboardBack[key];
        }
        
        private static bool KeyReleased(OpenTK.Input.Key key) {
            return !KeyboardFront[key] && KeyboardBack[key];
        }
        
        public static void Update(object sender, FrameEventArgs e) {
            float deltaTime = (float)e.Time;
            UpdateInput();
            bool altDown = KeyDown(OpenTK.Input.Key.AltLeft) || KeyDown(OpenTK.Input.Key.AltRight);
            
            if (altDown && KeyPressed(OpenTK.Input.Key.Enter)) {
                ToggleFullscreen();
            }
        }
        
        // ... Rest of the MainGameWindow class
```

That's all it takes to implement buffered input. In this example, we use the keyboard ```Alt+Enter``` to toggle in and out of full screen. If course in general you don't want to clutter up your window with this kind of code, unless it makes sense to you. 