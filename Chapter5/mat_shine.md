## Specular

The ambient and diffuse components of the material define the overall color of the object. The specular component defines how shiny the object is.

Let's make a new demo class we'll call it ```MateiralSpecular```. Get the code in it up to par with the test scene.

It's important to know, when setting the Ambient and Diffuse colors, OpenGL expected a floating point array.





```
// Setup camera
// Render unlit grid

// Set ambient and diffuse
GL.Material(MaterialFace.FrontAndBack, MaterialParameter.Specular, new float[] { 1, 1, 1, 1 } );
GL.Material(MaterialFace.FrontAndBack, MaterialParameter.Shininess, 0.0f);
// Draw sphere 1

// Set ambient and diffuse
GL.Material(MaterialFace.FrontAndBack, MaterialParameter.Specular, new float[] { 1, 1, 1, 1 } );
GL.Material(MaterialFace.FrontAndBack, MaterialParameter.Shininess, 16.0f);
// Draw sphere 2

// Set ambient and diffuse
GL.Material(MaterialFace.FrontAndBack, MaterialParameter.Specular, new float[] { 1, 1, 1, 1 } );
GL.Material(MaterialFace.FrontAndBack, MaterialParameter.Shininess, 128.0f);
// Draw sphere 3
```
