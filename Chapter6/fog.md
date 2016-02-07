# Fog

Adding fog to your world has more than one purpose. Besides trying to give the player the impression of actual fog, it can be used to obscure objects in the distance and have them gradually become clearer as the player gets closer. This is beneficial both in allowing you to reduce the number of geometry on the screen at one time (thus improving performance) and in preventing objects from suddenly popping into the view frustum.

There is more than one way to impement fog, but because OpenGL has a built in solution we will simply use that.

## OpenGL Fog

OpenGL's built-in fog support works by blending each pixel with the color of the fog, using a blend factor dependent on the distance from the viewer, the density of the god and the currently selected fog mode. This is the blend factor:

