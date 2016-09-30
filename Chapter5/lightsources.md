#Light sources
It's time to turn some lights on and get on with the show! Like most GL.Enable calls, you want to enable lighting in the initialize of your program.

You can enable lighting like so:

```
GL.Enable(EnableCap.Lighting);
```

This call causes the lighting equasion to be applied to every vertex, but it does not turn on any lights. You need to explicitly turn lights on by calling:

```
GL.Enable(EnableCap.LightX);
```

Where X is a number between 0 and 7 (inclusive). The OpenGL specification dictates that OpenGL will support a minimum of 8 lights. Some implementations support more, but 8 is the safe number.

## Only 8?
8 lights might not seem like a lot. That's because 8 lights is not a lot. It's a very conservitive number. This is largly because lighting is expensive. Enabling just 4 lights might cause a noticable drop in framerate.

You've no doubt seen games (Even those built on OpenGL 1X, like Doom3) that use way more than 8 lights! How do they do it? Well, if you are standing in the living room, the living room light has an effect on you. But the bedroom light does not.

Lights are enabled and disabled PER OBJECT. This means object A can be drawn with 2 lights, object B can be drawn with 6 and object c can be drawn with 3. Assuming each light is unique, we just drew a scene with 11 lights. But we didn't break OpenGL, because each indevidual object only enabled the lights that effect it.

## Light properties
Each world light has several properties associated with it that define it's position or direction in the world, the colors of its ambient, diffuse, specular and emissive terms and weather the light radiates in all directions or shines in a specific direction. 

These parameters are configured trough the following functions:

```
void GL.Light(LightName name, LightParameter param, float val);
void GL.Light(LightName name, LightParameter param, float[] val);
```

The first argument is an enumeration of type ```LightName```, it's going to have a value of ```LightName.Light0``` trough ```LightName.Light7```. This argument specifies which lights properties you are modifying.

The second argument is an enumerationof type ```LightParameter``` which tells openGL which value is being modified. These are the valid values:

* __Ambient__ Ambient intensity of the light
* __Diffuse__ Diffuse intensity of the light
* __Specular__ Specular intensity of the light
* __Position__ Position of the light as a vector4 (x, y, z, w)
* __SpotDireciton__ Direciton of the spot light as a vector3 (x, y, z)
* __SpotExponent__ Spotlight exponent
* __SpotCutoff__ Spotlight cutoff
* __ConstantAttenuation__ Constant attenuation
* __LienarAttenuation__ Linear atenuation
* __QuadraticAttenuation__ Quadratic attenuation

Lets discuss each of these settings briefly before actually using them in code.