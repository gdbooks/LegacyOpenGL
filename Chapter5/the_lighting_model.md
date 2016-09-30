# The lighting model

In addition to individual lights and materials, and normals there are additional global components of the lighting model that determine the final color of rendered objects. These are:

* A global ambient term
* Weather the location of the viewer is local or infinite
  * This only effects the specular highlight
* Weather the lighting is one or two sided
* Weather the calulated specular color is stored seperately from other color values
   * Meaning it's passed as it's own color to the final rendering stage

You control these elements of the lighting model with the ```GL.LightModel``` function, which is defined as:

```
void GL.LightModel(LightModelParameter, float);
void GL.LightModel(LightModelParameter, float[]);
```

The first paramater specifies which property you are modifying, the second is the value you are setting it to. The argument will either be a single float, or a 4 component array. The values for the first argument are:

* __LightModelAmbient__ Ambient intensity of the scene (RGBA)
  * The default value is (0.2, 0.2, 0.2, 1.0) 
* __LightModelLovalViewer__ Is the viewpoint local or infinite. 
  * The default value is 0 (INFINITE).
  * This takes either 1 (LOCAL) or 0 (INFINITE) as arguments 
* __LightModelTwoSide__ One or two sided lighting. 
  * Default is 0 (ONE-SIDED)
  * This can be either 0 (ONE-SIDED) or 1 (TWO-SIDED) 
* __LightModelColorControl__ Is the specular color stored seperate or together with ambient and diffuse
  * Default value is 0 (Single color, not sperate)
  * 0 is single, 1 is seperate

We will discuss each of these properties below, however they are not important enough to each get their own coding section. So pay attention, this is the only page they will be mentioned on. 

Of course if you want to play around with them and make some practice scenes on your own, you can certainly do so.

## Global ambient light
In addition to the ambient light contributed by individual light sources, there is a __global ambient light__ that is present wheater or not any light sources are enabled. This is used to model lights for which the source can not be determined. 

This value is controlled with ```LightModelAmbient```. The following code sets the global ambient light to a blue-green color. Kind of like what you might expect to see under water.

```
float[] ambient = new float[] { 0f, 0.2f, 0.3f, 1f };
GL.LightModel(LightModelParamater.LightModelAmbient, ambient);
```

__Note:__ I don't really like a magical light source that i can't see, more often than not i will straight up set this to black to avoid it from contributing to my scene.

## Local or Infinite View
When calculating the specular term, the direction from the vertex being calculated to the viewpoint effects the intensity of the specular highlight. The ```LightModelLocalViewer``` paramater lets you specify weather a viewpoint is local (based on the viewers actual world position) or an infinite distance away.

Having a local viewpoint will look more realistic, but will be significantly more expensive calculation wise, as OpenGL will now need to calculate the direction for each vertex. An infinite view is used by default, it looks good enough 90% of the time.

If the default isn't realistic enough for your application, you can change it any time with:

```
// This takes an int (or float), 
// 1 is ON 
// 0 is OFF
GL.LightModel(LightModelParamater.LightModelLocalViewer, 1); 
```

## Two Sided or One Sided Lighting
The next paramater you can specify is ```LightModelTwoSide```. This paramater deals with weather you want to calculate the ighting for the back of polygons correctly. For example, if you where to take an enclosed object, like a cube and cut it in half the inside would not be lit correctly. To light the inside correctly, you must enable two sided lighting like so:

```
// 0 or 1
GL.LightModel(LightModelParamater.LightModelTwoSide, 1); 
```

Because most of the time you will not see inside closed meshes, and you will not see the back side of visible meshes, keeping this off is a good idea. It's off by default to save on performance.

## Seperate Specular Color
The final light model property you can set is the ```LightModelColorControl``` property. This control was added because when you do lighting with texturing the specular highlight tends to get washed out once the texture is applied. 

When you enable this property, OpenGL stores two colors per vertex. One that is a combination of the ambient, diffuse, and emissive light combined with the texture of the object. And one that is it's specular value. At render time it applies the specular value, this keeps the specular highlight from getting washed out by the texture colors.

Enabling this property is pretty straight forward:

```
// 0 or 1
GL.LightModel(LightModelParamater.LightModelColorControl, 1); 
```

Because i've never rendered anything where the specular highlight __REALLY__ mattered, i've never had a need to turn this component on