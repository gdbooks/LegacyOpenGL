# Running your code
Now that you have a window set up, running with a blue clear screen (run the game, make sure you do!) the question is where should you write game code?

Turns out that question has a LOT of answers. You need to be able to choose the fastest solution, the one you are most comfortable with and build your project on that.


## 1) Inline
This is the easyest way to do things. I would only suggest writing inline code for small demo programs that you mainly use to test things out for yourself. If you expect the program to be large, don't do this.

Inline code is kind of what it sounds. The ```MainWindow``` class already has static Initialize, Update, Render and Shutdown functions. Just write your code inside of these functions and call it a day.

## 2) Static Reference
This is my personal favorite. You don't really need anything special to make it work. Let's assume you have a ```Game``` class.

```cs
class Game {
    public void Initialize() {
    
    }
    
    public void Update(float dt) {
    
    }
    
    public void Render() {
    
    }
    
    public void Shutdown() {
    
    }
}
```

You simply make a static reference variable to the Game class inside the ```MainWindow``` class, just like you have a ```Window``` variable referencing an OpenTK window.

```cs
using System;
using OpenTK;
using System.Drawing;
using OpenTK.Graphics.OpenGL;

namespace GameApplication {
    class MainGameWindow : OpenTK.GameWindow {
        //reference to OpenTK window
        public static OpenTK.GameWindow Window = null;
        
        // reference to game
        public static Game TheGame = null;
        
        public static void Initialize(object sender, EventArgs e) {
            TheGame.Initialize();
        }
        
        public static void Update(object sender, FrameEventArgs e) {
            float deltaTime = (float)e.Time;
            TheGame.Update(deltaTime);
        }
        
        public static void Render(object sender, FrameEventArgs e) {
            GL.ClearColor(Color.CadetBlue);
            GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
            TheGame.Render();
            Window.SwapBuffers();
        }
        
        public static void Shutdown(object sender, EventArgs e) {
            TheGame.Shutdown();
        }
        
        [STAThread]
        public static void Main() {
            //create static (global) window instance
            Window = new MainGameWindow();
        
            // create static (global) game instance
            TheGame = new Game();
            
            // ...
```

## 3) Singleton
This method should be familiar by now, it's the one we used for both Mario and the DungeonCrawler project. It's very similar to approach #2, except instead of storing a static reference to the ```Game```class in the ```MainGameWindow``` class you store a reference to it in the ```Game``` class trough a singleton.

A singleton is just a static way to access a SINGLE instance of a class. We ensure a class can only have a single instance by making it's constructor private.

```cs
class Game() {
    // Private constructor. 
    // Only the Game class can make a new Game instance.
    priate Game() {
    
    }
    
    // Global reference to the single instance of this calss
    private static Game instance = null;
    
    // Public access (read only) to the game instance
    public static Game Instance {
        get {
            // Lazy initialization
            if (instance == null) {
                // Remember, only this class can make a new instance ;)
                instance = new Game();
            }
            
            return instance;
        }
    }
```