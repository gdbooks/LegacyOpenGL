# Texture Paramaters

You can tell OpenGL how to treat different properties of textures using the ```GL.TexParameter``` function. We've already used the function when telling OpenGL how to handle the min and mag filters for the textures being loaded. What other texture paramaters can we control?

## Texture Wrapping
There are a few paramaters we can control using ```GL.TexParameter```, but the only ones we care about are mip-mapping and texture wrapping. So what is texture wrapping?