# Spot Light    
The spot light is probably the least used of the three light types OpenGL provides. Why is that? Well, the attenuation calculation for a spot light is the same as that of a point light, but in addition to that OpenGL needs to do math to only light a cone, not a sphere. Limiting the light to a cone shape makes spot lights considerably more expensive than point lights. So much so, that things a spot light would be perfect for like street lamps are most often implemented with point lights instead.

So, if spot lights are more expensive, and point light generally have the same look to them, is there a reason to know about spot lights? Or even to implement them? Yes there is. Sometimes using a PointLight will not suffice. If the shape of the light matters, a spot light is the only way to go. Like luigis manor:

![LIGHT](luigis_manor.jpg)

We're going to go trough implementing a simple spot light together. All the advanced lighting topics (local lights, moving lights, etc...) have been covered in the Directional Light and Point Light sections. So this section should be pretty short.

## Implementation

Lets start with the unlit test scene we set up in the begenning. Enable lighting and light 0. Configure light 0 to have a diffuse term of yellow, an ambient term of black and a specular term of white.

Set the position of this light to  (0, 3, 3, 1). Unlike the colors, you can't configure the lights position in the Initialize function. Just like a Point Light, a Spot light must be defined in eye space. Therefore you have to set it after the view matrix is set.

Your scene should be lit like this (once you let it rotate a bit):

![S1](spot2.png)

Next, let's set the properties that make up a spot light, these are ```SpotCutoff```, ```SpotExponent``` and ```SpotDirection```. For an review of how these work, check the "Spotlights" section of "Light Sources", because they have already been covered there i'm not going to cover them here. We're just going to use them.

You can set these properties in the ```Initialize``` function, as they don't change frame to frame. We're going to set a cutoff of 15, an exponent of 5 and a direction of  (0, -1, 0):

```
float[] direction = { 0f, -1f, 0f };
GL.Light(LightName.Light0, LightParameter.SpotCutoff, 15f);
GL.Light(LightName.Light0, LightParameter.SpotExponent, 5f);
GL.Light(LightName.Light0, LightParameter.SpotDirection, direction);
```

If you run your game and let it rotate you should see the below image. If your whole scene is black that means your grid's vertices are all outside of the spot lights reach, it needs to be sub-divided more.

![S3](spot3.png)