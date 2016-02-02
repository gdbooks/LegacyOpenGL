# Where render color comes from
This page is a straight up rip of [Steve Baker's OpenGL Lighting](http://www.sjbaker.org/steve/omniv/opengl_lighting.html) tutorial. Some of this stuff we've already covered and will serve as a review, other information might be brand new. 

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

For the glMaterial, it's usual to set the Ambient and Diffuse colours to the natural colour of the object and to put the Specular colour to white. The emission colour is generally black for objects that do not shine by their own light.

Before you can use an OpenGL light source, it must be positioned using the glLight command and enabled using glEnable(GL_LIGHTn) where 'n' is 0 through 7. There are additional commands to make light sources directional (like a spotlight or a flashlight) and to have it attenuate as a function of range from the light source.

