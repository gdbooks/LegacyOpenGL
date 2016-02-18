#Deleting textures

Now that we know exactly what we have to do in the initialize function of a textured application, let's take a quick look at how to clean up a texture handle. This could not be any easyer.

First, make sure the texture you are deleting isn't bound! Remember, you can always bind 0 as the active texture (effectivley binding null). Next, you need to pass the textures handle to the ``` ``` function. This function has two variations:

```

```

One of them takes a single handle and deletes it, the other takes an array of handles and deletes them all. It should be common sense, but once a texture is deleted you should not use it. If you do, no error will be thrown, but undefined behaviour will happen!