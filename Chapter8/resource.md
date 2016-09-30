# Resource Management

Loading meshes and textures one at a time isn't the worst thing you can do, but it does make managing them extremly difficult. 

Let's say you have a scene with 1 texture used 20 times. You obviously don't want to load the same texture 20 times. So do you keep a counter around? How do you manage the lifecycle of those textures? This is where resource management comes in handy.

## Reference counting

The basic idea behind resource management is simple, it's called reference counting. You put all of the code related to loading a specific type of asset into a class. Let's assume we're working with meshes. We could easily make a ```MeshManager``` class. This class would contain all the code needed to load meshes. 

The manager needs to keep a counter for every mesh. This is the reference counter. It increments every time a mesh is loaded and decrements every time a mesh is unloaded. Once this reference counter reaches 0 the mesh in question is deleted, because it's no longer being used.

## Handles

When a manager loads an asset, it owns the resource. You are essentially leasing an instance of that resource, eventually you have to give it back. In the case of a mesh you will need a way to access the array of verts, in the case of a texture you will need the OpenGL texture ID. So, how do you get a reference to this?

The answer here is a handle. Once a manager loads a resource it needs to give you a resource handle. When it gives you the resource handle, the reference count is increased. When you give the resource handle back, it is decreased. So what is a resource handle?

It can be anything! Well, anything that will uniquelly identify an object. The most common things to use as resource handles are integers and strings. For example, the 2DOpenTKFramework we used returned integer handles.

## Previous framework
Speaking of the 2D framework we used, let's take a quick look at how it functioned. Take some time and browse trough the [TextureManager](https://github.com/Mszauer/GameFramework/blob/master/2DFramework/Framework/TextureManager.cs) class. Knowing the above information should help you navigate the class.

In the [TextureManager](https://github.com/Mszauer/GameFramework/blob/master/2DFramework/Framework/TextureManager.cs) i have an internal class that represents each texture instance, this calss is called ```TextureInstance```. The ```TextureManager``` contains a ```List``` of ```TextureInstance```s. 

When a new texture is requested i loop trough this list, and check the path of each intance. If a path identical to the texture being requested is found, it's reference count is incremented and a handle is returned.

If the texture was not found in the list, i loop trough the list, looking for any texture instances with a reference count of 0. 

This manager utilizes lazy unloading, meaning when the reference count reaches 0 the texture is not unloaded. Instead textures are unloaded if a new texture is requested and a slot with a 0 reference is available.

If there was no 0 reference slot available and the texture was not already loaded, a new one is made and added to the list. And a handle to that is returned.

So what are the handles in the framework? The handle of a texture is it's index in the list. Why did i choose that, it is the cheapest way to retrieve a texture. Loading a texture is expensive, but retreaving it is relativley cheap.

## Whats next?

Next up, i want to walk you trough actually creating a simple resource manager. 

There is no one size fits all solution for asset management, and the methods i show you are far from perfect. Each game figures out what they need and build the asset pipeline around their needs. Engines like Unity or Unreal handle this for you.

I like to create a spearate manager for each asset type. That is, i'll have a ```TextureManager``` a ```MeshManager```, an ```AudioManager``` and so on. Some companies just make a single class called ```AssetManager``` that manages every possible asset type. Like i said before, there is no wrong or right way. What you use depends on what makes sense to you and what your game needs.