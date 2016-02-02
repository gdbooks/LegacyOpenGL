# Where render color comes from
This page is a straight up rip of [Steve Baker's OpenGL Lighting](http://www.sjbaker.org/steve/omniv/opengl_lighting.html) tutorial. Some of this stuff we've already covered and will serve as a review, other information might be brand new. Think of this as a one page cheat-sheet for lighting.

## Introduction

Many people starting out with OpenGL are confused by the way that OpenGL's built-in lighting works - and consequently how color functions. I hope to be able to clear up some of the confusion. What is needed to explain this clearly is a flow chart:

![SRC](color_source.png)

## Lighting ENABLED or DISABLED?

The first - and most basic - decision is whether to enable lighting or not.

```
GL.Enable(EnableCaps.Lighting);
```

...or...

```
GL.Disable (EnableCaps.Lighting);
```

If it's disabled then all polygons, lines and points will be colored according to the setting of the various forms of the ```GL.Color``` command. Those colors will be carried forward without any change other than is imparted by texture or fog if those are also enabled. Hence:

```
GL.Color3(1.0f, 0.0f, 0.0f) ;
```

...gets you a pure red triangle no matter how it is positioned relative to the light source(s).

With ```EnableCaps.Lighting``` enabled, we need to specify more about the surface than just it's color - we also need to know how shiny it is, whether it glows in the dark and whether it scatters light uniformly or in a more directional manner.

The idea is that OpenGL switches over to using the current settings of the current _"material"_ instead of the simplistic idea of a polygon _"color"_. This is an over-simplistic explanation - but keep it firmly in mind.

##glMaterial and glLight

The OpenGL light model presumes that the light that reaches your eye from the polygon surface arrives by four different mechanisms:

* __AMBIENT__ - light that comes from all directions equally and is scattered in all directions equally by the polygons in your scene. This isn't quite true of the real world - but it's a good first approximation for light that comes pretty much uniformly from the sky and arrives onto a surface by bouncing off so many other surfaces that it might as well be uniform.
* __DIFFUSE__ - light that comes from a particular point source (like the Sun) and hits surfaces with an intensity that depends on whether they face towards the light or away from it. However, once the light radiates from the surface, it does so equally in all directions. It is diffuse lighting that best defines the shape of 3D objects.
* __SPECULAR__ - as with diffuse lighting, the light comes from a point souce, but with specular lighting, it is reflected more in the manner of a mirror where most of the light bounces off in a particular direction defined by the surface shape. Specular lighting is what produces the shiny highlights and helps us to distinguish between flat, dull surfaces such as plaster and shiny surfaces like polished plastics and metals.
* __EMISSION__ - in this case, the light is actually emitted by the polygon - equally in all directions.
 

So, there are three light colors for each light - Ambient, Diffuse and Specular (set with ```GL.Light```) and four for each surface (set with ```GL.Material```). All OpenGL implementations support at least eight light sources - and the ```GL.Material``` can be changed at will for each polygon.

The final polygon color is the sum of all four light components, each of which is formed by multiplying the ```GL.Mateiral``` color by the ```GL.Light``` color (modified by the directionality in the case of Diffuse and Specular). Since there is no Emission color for the ```GL.Light```, that is added to the final color without modification.

A good set of settings for a light source would be to set the Diffuse and Specular components to the color of the light source, and the Ambient to the same color - but at MUCH reduced intensity, 10% to 40% seems reasonable in most cases. So, a blue light might be configured like so:

```
float[] blue = new float[] { 0f, 0f, 1f, 1f };
float[] dimBlue = new float[] { 0f, 0f, 0.1f, 1f);

GL.Light(LightName.Light0, LightParameter.Ambient, dimBlue );
GL.Light(LightName.Light0, LightParameter.Diffuse, blue );
GL.Light(LightName.Light0, LightParameter.Specular, blue );
```

For the ```GL.Material```, it's usual to set the Ambient and Diffuse colors to the natural color of the object and to put the Specular color to white. The emission color is generally black for objects that do not shine by their own light. So, a yellow object might have the following material:

```
float[] red = { 1f, 0f, 0f, 1f };
float[] white = { 1f, 1f, 1f, 1f }
float[] black = { 0f, 0f, 0f, 1f }

GL.Material(MaterialFace.FrontAndBack, MaterialParameter.Ambient, red);
GL.Material(MaterialFace.FrontAndBack, MaterialParameter.Diffuse, red);
GL.Material(MaterialFace.FrontAndBack, MaterialParameter.Specular, white);
GL.Material(MaterialFace.FrontAndBack, MaterialParameter.Emission, black);
```

Before you can use an OpenGL light source, it must be positioned using the ```GL.Light``` command and enabled using ```GL.Enable(EnableCaps.LightN)``` where 'N' is 0 through 7. There are additional commands to make light sources directional (like a spotlight or a flashlight) and to have it attenuate as a function of range from the light source.

## glColorMaterial

This is without doubt the most confusing thing about OpenGL lighting - and the biggest cause of problems for beginners.
The problem with using ```GL.Material``` to change polygon colors is two-fold:

* You frequently need to change ```GL.Mateiral``` properties for both Ambient and Diffuse to identical values - this takes two OpenGL function calls which is annoying.
* You cannot change ```GL.Mateiral``` settings with many of the more advanced polygon rendering techniques such as Vertex arrays and ```GL.DrawElements```.

For these reasons, OpenGL has a feature that allows you do drive the ```GL.Material``` colors using the more flexible ```GL.Color``` command (which is not otherwise useful when lighting is enabled).

To drive (say) the Emission component of the ```GL.Material``` using ```GL.Color```, you must say:

```
GL.Enable(EnableCap.ColorMaterial);
GL.ColorMaterial(MaterialFace.FrontAndBack, ColorMaterialParameter.Emission);
```

From this point performing a ```GL.Color``` command has the exact same effect as calling:

```
GL.Material(MaterialFace.FrontAndBack, MaterialParameter.Emission, ...colors...);
```

One especially useful option is:

```
GL.ColorMaterial(MaterialFace.FrontAndBack, ColorMaterialParameter.AmbientAndDiffuse);
```

This causes ```GL.Color``` commands to change both Ambient and Diffuse colors at the same time. That's a very common thing to want to do for real-world lighting models.


## Light Sources.

OpenGL's lights are turned on and off with ```GL.Enable(EnableCaps.LightN)``` and ```GL.Disable(EnableCaps.LightN)``` where 'N' is a number in the range zero to the maximum number of lights that this implementation supports (typically eight). 

The ```GL.Light``` call allows you to specify the color (ambient, diffuse and specular), position, direction, beam width and attenuation rate for each light. 

By default, it is assumed that both the light and the viewer are effectively infinitely far from the object being lit. You can change that with the ```GL.LightModel``` call - but doing so is likely to slow down your program - so don't do it unless you have to. ```GL.LightModel``` also allows you to set a global ambient lighting level that's independent of the other OpenGL light sources.

There is also an option to light the front and back faces of your polygons differently. That is also likely to slow your program down - so don't do it.

