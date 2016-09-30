## Position and Direction
Each light can have either a position or a direction. Lights with a position are often called __positional lights__ or __point lights__. __directional lights__ on the other hand are infinitley far away, they have no position. There are no true direcitonal lights in nature, since nothing is infinitley far away. But some light sources are so far away they can be treated as directional lights (like the sun).

So why use a direcitonal light instead of a really big point light (like how the sun actually works)? Performance. Directional lights are (marginally) cheaper than spot lights. Admitedly, on modern hardware this is no longer a problem.

You can set a lights position by passing ```LightParameter.Position``` to the ```GL.Light``` function. The function will then take a 4 element array of floats (x, y, z, w) representing either the lights position or direction. The __w__ component is used to indicate if the vector passed in is a position or a direction vector. A w of 0 is directional, a w of 1 is positional.

Here is how you could set the direciton of a light

```
// direction must be normalized
float[] lightDir = { 0.5f, 0.5f, 0.0f, 0.0f};
// We pass in a direciton vector, this is going to be a directional light
GL.Light(LightName.Light0, LightParamater.Position, lightDir);
```

If i wanted to set up a positional or point light

```
float[] lightDir = { 2.5f, 3.5f, 7.0f, 1.0f};
// We pass in a position vector, this is going to be a point light
GL.Light(LightName.Light0, LightParamater.Position, lightDir);
```

The default position for all lights is (0, 0, 1, 0), it is directional, pointing down the negtive Z axis.

__IMPORTANT:__ Whenever you make a call to ```GL.Light``` with ```LightParamater.Position``` the vector you specify is transformed by the current __modelview__ matrix, just as vertices are, and stored in eye coordinates. We will discuss this in more detail later.