# Putting it all together

Now we know everything we need to know to make a simple textured scene! Nothing left to do except actually make something textured! In this section i'm going to walk you trough setting up a simple textured scene, but the actual texturing work will be up to you.

## Putting together the test scene

## On your Own

## Adding some detail

## On Your Own

## Sorting

So far we've been writing all of this rendering code inline, that is whenever we needed to render anything, we just do! The only thing we really wrapped into an object has been the grid. Real games don't work like this. Real games are component based.

In a real game, you have a game object, the game object has a list of components. When the game object's render function is called, it just calls render on every component. This should sound familiar to you, it's the system you built for the dungeon game.

In the dungeon game objects rendered according to how deep they where in the scene graph. This works for a simple 2D game, but not for a complex 2D game. It would never work on a 3D game. It's pretty advanced, but there is a few [Handmade Hero](https://hero.handmadedev.org/videos/game-architecture/day229.html) episodes on the topic.

I'll walk you trough how it would normally work. This is not something you need to implement or anything, but it is something to be aware of, as you will need to do this at some point.

## Transparent objects
When reading this remember, you can render solid objects in any order. The Z-Buffer will make sure that objects get rendered correctly.

Game Objects only need to be sorted when they are rendered with transperancy!

## The Game Object

For this example, let's assume we have a super simple 3D game object class. The class is going to be super minimal for this example, it's going to have a list of components, a list of children, a potential parent and a 3D transform (a matrix).

```
class GameObject {
    public string Name;
    public List<Component> Components;
    public GameObject Parent;
    public List<GameObject> Children;
    public Matrix4 LocalTransform;
    
    public Matrix4 WorldTransform {
        get {
            // The order of this multiplication might be wrong
            return LocalTransform * Parent.LocalTransform;
        }
    }
    
    public void Update(float deltaTime) {
        foreach (Component component in Components) {
            component.Update(deltaTime);
        }
        foreach(GameObject child in Children) {
            child.Update(deltaTime);
        }
    }
}
```

## The Scene

The scene class, is going to for the most part be what we are used to, a root game object, that in turn has many children. The update function is actually going to be recursive, like we are used to. One thing that's different in this scene is it's going to have a view matrix defined. This is essentially the camera. 

Remember, you can get the world position of the camera (the viewer) by taking the inverse of the view matrix. And you can get the view matrix by taking the inverse of the world position of the camera matrix.

```
class Scene {
    public string Name;
    public GameObject Root;
    public Matrix View;
    
    public void Update(float deltaTime) {
        if (Root != null) {
            Root.Update(deltaTime);
        }
    }
}
```

## Rendering
The thing you have probably noticed is we don't have a Render function in the scene, or the game object! This is because we can't just recursivley render objects. Well, we technically could, and the Z-Buffer would take care of the display. But if we want to render transparent object, they must be sorted!