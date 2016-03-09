# Resource Management

Loading meshes and textures one at a time isn't the worst thing you can do, but it does make managing them extremly difficult. 

Let's say you have a scene with 1 texture used 20 times. You obviously don't want to load the same texture 20 times. So do you keep a counter around? How do you manage the lifecycle of those textures? This is where resource management comes in handy.

## Reference counting
The basic idea behind resource management is simple, it's called reference counting. You put all of the code related to loading a specific type of asset into a class. Let's assume we're working with meshes. We could easily make a ```MeshManager``` class. This class would contain all the code needed to load meshes. 

The manager needs to keep a counter for every mesh. This is the reference counter. It increments every time a mesh is loaded and decrements every time a mesh is unloaded.