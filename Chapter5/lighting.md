# Lighting In OpenGL
You have now arrived at one of the most important aspects of 3D graphics: lighting. It is one of the few elements that can make or break the realism of your 3D game. So far, you've looked at how to build objects, move object, color objects and shade objects. Now let's look at how to make these objects come to life with materials and lights.

## OpenGL Lighting and the Real World
Let's take a quick step back and look at a simple explanation of how light woks in the real worlds. Light sources produce photons of many different wavelengths, colvering the full spectrum of color. Many of these photons strike objects, which absorb some of them and reflect others. The reflected photons might be reflected uniformly (smooth shiny surfaces) or they may get scattered (rough, matte sourfaces). The reflected photons may strike other objects and repeat the process. We are able to see objects because some of these photons eventually enter our eyes

Modelling the complex interaction of light photons with even a small number of objects (one) is computationally expensive. So much so that it is not feasable to do with an interactive framerate. This type of rendering is refered to as ray tracing, not to be confused with ray-casting which we have done.

OpenGL (and other graphics API's) use simplified lighting models that trade accuracy for speed. Altough the results will not match the real world exactly, they are close enough to be believable. 

OpenGL calculates lighting by approximating the light into __R__ed, __G__reen and __B__lue components. This means that the color a light emits is determined by the amount of red,green and blue light it emits.

Light is further broken down into four different terms, which together atempt to simulate the major effects seen in real-world ighting:

* __Ambient Light__ simulates the light bouncing between surfaces so many times that the source of the light is no longer apparent. This components is NOT affected by the position of the light or the position of the viewer.
* __Diffuse Light__ comes from a certain direction, once it strikes a surface it is reflected equally in all directions. The figguse light components IS affected by the position (direction) of the light, but NOT the position of the viewer.
* __Specular Light__ is drirectional and is reflected off a surface in a particular direction. Specularity is often refered to as shininess. the speculer term is effected by BOTH the position (direction) of the light AND the viewer.
* __Emussive Light__ is a cheap way to simulate objects that emit light. OpenGL will not actually use the emissive term to illuminate nearby objects, it simply causes the emissive object to be more intensly lit.

The final results of lighting depend on several major factors, each of which will be discussed in detail ater. the factors are:

* One of more light sources. Each light source will have the ambient, diffuse, specular and emmisive terms listed above. Each specified as RGBA values. In addition they will either have a position or direction, or have terms that affect the attenuation and may have a limited area of affect.
* The orientation of surfaces in the scene. This is determined trough theuse of normals, which are associated with each vertex.
* The material each object is made of. Material properties define what precentages of the RGBA values each ligt term should be reflected. They also define how shiny a surface is.
* The lighting model, which includes a global ambient term (independent of any light source) and wether or not the position of the viewer has an effect on lighting calculations. 

When the light strikes a surface, OpenGL uses the material of the surface to determine how much R, G and B light should be reflected by the surface. Even tough they are approximations, the equations used by OpenGL can be computed very quickly and produce reasonably good results.