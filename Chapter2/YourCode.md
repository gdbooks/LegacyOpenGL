# Running your code
Now that you have a window set up, running with a blue clear screen (run the game, make sure you do!) the question is where should you write game code?

Turns out that question has a LOT of answers. You need to be able to choose the fastest solution, the one you are most comfortable with and build your project on that.


## 1) Inline

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

You simly make a static reference variable to the Game class inside the 

## 3) Singleton