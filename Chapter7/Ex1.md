# Putting it all together

Now we know everything we need to know to make a simple textured scene! Nothing left to do except actually make something textured! In this section i'm going to walk you trough setting up a simple textured scene, but the actual texturing work will be up to you.

## Putting together the test scene

## On your Own

## Adding some detail

## On Your Own

## Sorting

So far we've been writing all of this rendering code inline, that is whenever we needed to render anything, we just do! The only thing we really wrapped into an object has been the grid.

Rendering a 3D game will get complex. A 3D mixes and matches solid and transparent objects sometimes. Altough, if you look at modern 3D games, it doesn't happen a lot. In order to draw transparent objects, you must first draw solid objects, then draw transparent objects based on who is furthest from the camera.

I'll walk you trough how it would normally work. This is not something you need to implement right now, but it is something to be aware of, as you will need to do this at some point.

## Transparent objects
When reading this remember, you can render solid objects in any order. The Z-Buffer will make sure that objects get rendered correctly.

Game Objects only need to be sorted when they are rendered with transperancy! And even then, it's advised to render in two passes. First, render the solid objects, next render the transparent ones.

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
    
    public void RenderSolid() {
        foreach (Component component in Components) {
            if (component is MeshRenderer) {
                MeshRenderer renderer = component as MeshRenderer;
                if (!renderer.UsingAlpha) {
                    component.Render();
                }
            }
        }
        foreach(GameObject child in Children) {
            child.RenderSolid();
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
    
    public void Render() {
        // load the view matrix
        GL.LoadMatrix(Matrix4.Transpose(View).Matrix);

        if (Root != null) {
            // Each object will load it's own model matrix
            Root.RenderSolid();
        }
    }
}
```

This will render all solid objects in the scene. Now, let's see what we need to do to render transparent objects!

## Rendering transparent objects
In order to render transparent objects, we have to do a 2 step process. First, we need to collect all transparent object. Then, we need to sort them based on distance to camera. I'm going to make a new class (a protected helper of scene) and create a method to collect all transparent objects

```
class Scene {
    protected class RenderCommand {
        MeshRenderer component;
        Matrix worldTransform;
        float DistanceToCamera;
        
        public void Execute() {
            // Backup view matrix
            GL.PushMatrix();
            
            // Apply game object transform
            GL.MulMatrix(Matrix4.Transpose(worldTransform).Matrix);
            // Render component
            component.Render();
            
            // Restore view matrix
            GL.PopMatrix();
        }
    }
    
    public string Name;
    public GameObject Root;
    public Matrix View;
    
    public void Update(float deltaTime) {
        if (Root != null) {
            Root.Update(deltaTime);
        }
    }
    
    public void Render() {
        // load the view matrix
        GL.LoadMatrix(Matrix4.Transpose(View).Matrix);

        if (Root != null) {
            // Each object will load it's own model matrix
            Root.RenderSolid();
        }
        
        // Collect transparent objects
        List<RenderCommand> transparentObjects = CollectTransparent(Root);
        
        // Sort the transparent objects back to front
        SortList(transparentObjects);
        
        // Finally, render all transparent objects, back to front
        foreach (RenderCommand command in transparentObjects) {
            command.Execute();
        }
    }
    
    void SortList(List<RenderCommand> list) {
        // Implement bubble sort, 
        // sort the list using the DistanceToCamera field
        // of each RenderCommand
    }
    
    List<RenderCommand> CollectTransparent(GameObject object) {
        List<RenderCommand> result = new List<RenderCommand>();
        
        foreach(Component component in object.Components) {
            if (component is MeshRenderer) {
                MeshRenderer renderer = component as MeshRenderer;
                
                if (renderer.UsingAlpha) {
                    RenderCommand command = new RenderCommand();
                    command.component = renderer;
                    command.worldTransform = object.WorldTransform;
                    
                    // Find the distance to camera. We do this by taking two points at 0, 0, 0
                    // Next transform one point to where the camera is
                    // and the other point to where the game obejct is
                    // With the transformed vectors, check their difference
                    
                    Vector3 cam = Matrix4.Inverse(View) * new Vector3(0, 0, 0);
                    Vector3 obj = object.WorldTransform * Vector3(0, 0, 0);
                    command.DistanceToCamera = Vector3.Length(cam - obj);
                    
                    result.Add(command);
                }
            }
        }
        
        foreach(GameObject child in object) {
            List<RenderCommand> childRenderers = CollectTransparent(child);
            if (childRenderers != null && childRenderers.Count > 0) {
                result.AddRange(childRenderers);
            }
        }
        
        return result;
    }
}
```

It's a lot of code, but our renderer finally has a list of transparent objects. Instead of storing GameObjects i made a helper calss called RenderCommand. We could have just stored objects, then found the render component on those objects later when it's time to draw them, but this is slightly more efficient.

Render commands are also pretty standard in 3D games. Usually even solid objects get grouped into a set of render commands, they are not sorted; but commands only get created for objects that the camera can see. We will talk about this more in detail later.

All that's 