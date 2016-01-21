# Spotlights
Normally positional lights radiate in all directions. However you can limit the effect of the line to a cone. This would look like a flash light, the effect is called a _spotlight_.

To create a spotlight, you set up a positional light like you normally would and then set a few spotlight-specific paramaters: cutoff, direction and focus.

Lets think about what a spotlight looks like for a moment. If you where looking at a spot light in pure darkness, you would see that the light creates a cone of light in the direction that the spotight is pointing.

![LIGHTS](lights.png)

With OpenGL, you can define how wide this cone of light should be by specifying the angle between the edge of teh cone and its axis by passing ```LightParameter.SpotCutoff``` to ```GL.Light```.

![CUTOFF](spot_cutoff.gif)

A cutoff value of 10 for example results with a spotlight whose cone spreads out 20 degrees. OpenGL accepts only values between 0 and 90 for this paramater, with the special exception of 180. 180 is the default value and turns the spotlight into a regular point light.

This is the code you would use to create a spotlight with a 30 degree cone
```
GL.Light(LightName.Light0, LightParameter.SpotCutoff, 15.0f);
```