# Implementing a Manager

Let's go trough and create a texture manager together. What functions will this manager need?

* __Initialize__ All managers should have some kind of initialize function (that's not the constructor). Even if the function does nothing, it helps keep code readable
* __Destroy__ Whatever is initialized must be destroyed! This is also a good place to warn the rest of the code if any resources where not unloaded
* __LoadTexture__ This function is the powerhouse, it does the actual work of loading a texture, OR incrementing the reference on an already loaded texture
* __UnloadTexture__ ```LoadTexture``` requests memory, this function will release memory
* __GetTexture__ We need a way to access the raw OpenGL texture handle. This is it.

Are there any other considerations to designing this system? Tons! Do we want insertion of new data to be fast and retrieval to be slow? Or do we want insertion to be slow and retrieval to be fast? 

This is an important question because it affects the data structures we will use internally. If we want to retrieve the data fast, we must store it in a linear array, but every time we insert that array must be searched; making retrieval slow.

Because most games use a loading screen when loading assets, we're going to design this system for fast retrieval, slow insertion. This means the underlying data structure is going to be an array, so our handles will be integers (Indices into the array)

## Helper classes