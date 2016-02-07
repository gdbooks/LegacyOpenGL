# Blend Practice
We're going to real quick try to render a scene which uses blending. This exercise will also re-inforce lessons we've learned in the materials section.

First, set up a test scene. In this scene your camera will not be moving!

### Initialize
* Enable lighting, we're going to use only one light
* Set the lights position to 0, 1, 1, it's a direcitonal light
  * REMEMBER TO SET A W 
* Set a uniform low ambient color (dark gray)
  * I suggest (.2, .2, .2, 1) 
* Set a uniform high diffuse color (light gray)
  * I suggest (.8, .8, .8, 1) 
* Set a specular color of white
* Set the specular property of the front and back of the material to white
* Set the mateiral specular exponent to 20
* Enable color tracking for the ambient AND diffuse components