#Point Lights  
The code for point lights is very similar to the code for directional lights. You can think of a directional light as the sun, a point light is more like a light-bulb. It's a point in space that emits light, the further the light gets from the point, the less it's influance is. Unlike directional lights, point lights do have a position, which means the matrix stack and the model-view matrix become incredibly important.

## Single Static Light
Lets start by adding a single, static point light to our test scene. We will have the light be near origin, and large enough to illuminate all objects in the scene. For this, the light will be yellow. 

In the Initialize function enable lighting and light 0. Set the color of the light to yellow RBG(1, 1, 0). Remember, to set the color of a light you need to set it's ambient, diffuse and specular components. Also, the lgiht color is a 4 component array, the last component being alpha!

Still in initialize, after the color of the light is set up, we need to set the lights position. The position is a 4 component vector (float array),with the W component being 1.

```
// Having a W of 1 makes the lgiht a point light with a position
float[] position = new float[] { 0f, 1f, 0f, 1f };
GL.Light(LightName.Light0, LightParameter.Position, position);
```

```
GL.Light(LightName.Light0, LightParameter.ConstantAttenuation, 0.25f);
GL.Light(LightName.Light0, LightParameter.LinearAttenuation, 0.25f);
GL.Light(LightName.Light0, LightParameter.QuadraticAttenuation, 0.0f);
```