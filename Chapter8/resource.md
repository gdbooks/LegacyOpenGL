# Resource Management

Loading meshes and textures one at a time isn't the worst thing you can do, but it does make managing them extremly difficult. 

Let's say you have a scene with 1 texture used 20 times. You obviously don't want to load the same texture 20 times. So do you keep a counter around? How do you manage the lifecycle of those textures? This is where resource management comes in handy.

## Reference counting
The basic idea behind resource management is simple, it's called reference counting. You put all of the code related to loading a specific type of asset into a class. Let's assume we're working with meshes. We could easily make a ```MeshManager``` class. This class would contain all the code needed to load meshes. 

The manager needs to keep a counter for every mesh. This is the reference counter. It increments every time a mesh is loaded and decrements every time a mesh is unloaded. Once this reference counter reaches 0 the mesh in question is deleted, because it's no longer being used.

## Handles
When a manager loads an asset, it owns the resource. You are essentially leasing an instance of that resource, eventually you have to give it back. In the case of a mesh you will need a way to access the array of verts, in the case of a texture you will need the OpenGL texture ID. So, how do you get a reference to this?

The answer here is a handle. Once a manager loads a resource it needs to give you a resource handle. When it gives you the resource handle, the reference count is increased. When you give the resource handle back, it is decreased. So what is a resource handle?

It can be anything! Well, anything that will uniquelly identify an object. The most common things to use as resource handles are integers and strings. For example, the OpenTK Framework we created