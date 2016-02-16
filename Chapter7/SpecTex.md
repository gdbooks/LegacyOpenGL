#Specifying Textures
Now that a texture object has been created on the GPU and it's associated handle has been bound as the active texture, it's time to actually fill that texture object with data.

It's important to make the distinction between GPU memory and CPU memory. You have control of CPU memory, but not GPU memory. Rendering is done mostly using GPU memory. Therefore, to render a texture, you must upload it to GPU memory.

The process of getting a texture into GPU memory is fairly straight forward. 

* First, you must have the texture object you want to fill with data bound. 
* Read the texture data into CPU memory.
* Upload to the GPU with ``` ```
* At this point, you can manually delete CPU memory
    * Or let the garbage collector do it, depends on how you loaded the texture

The ``` ``` function is fairly straight forward

```

```

Explain arguments

So, how do we get that array of bytes that represents the texture?