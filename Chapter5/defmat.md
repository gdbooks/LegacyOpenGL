# Defining materials

Now that you have a general understanding of what materials are, lets look at how to use them. Materials do not need to be enabled or anything, the truth is you have been using them all along, just with whatever the default values where. Now, we're going to set some non-default values.


Actually, setting a material is fairly similar to creating a light source. The main difference is the name of the functions being used:

```
void GL.Material(MaterialFace face, MaterialParameter param, float value);
void GL.Material(MaterialFace face, MaterialParameter param, float[] value);
```

The face paramater in these functions specifies how the material will be applied to the objects polygons, implying that materials can affect the front and back faces of polygons differently. It can be one of three values:

* Front
* Back
* FrontAndBack

Only the faces you specify will be modified by the call to ```GL.Material```. Most often you will use the same values for front and back faces (and 90% of the time back faces will be culled, so the back face value won't matter). The second paramater _param_ tells OpenGL which material property we are assiging a value to. This can be any of the following:

* __Ambient__ Ambient color of the material
* __Diffuse__ Diffuse color of the material
* __AmbientAndDiffuse__ Ambient AND diffuse color of material
* __Specular__ Specular color of material
* __Shininess__ Specular exponent (power)
* __Emission__ Emissive color

## Material Colors
The ambient, diffuse and specular components specify how a material interacts with a light source and, thus determine he color of the material. These values are set by passing ```Ambient```, ```Diffuse``` or ```AmbientAnddiffuse``` to ```GL.Material``` as the ```MaterialParamater```. Most often the same values are used for both the ambient and diffuse term, so much so that OpenGL provided a conveniance paramater ```AmbientAndDiffuse``` so you can specify both with 1 function call.

For example, if you wanted to set the ambient material color to red for the front and back of polygons, you would call this function:

```
float[] red = { 1f, 0f, 0f, 1f };
GL.Material(MaterialFace.FrontAndBack, MaterialParameter.Ambient, red);
```

Similarly, to set both the ambient and diffuse paramaters to blue would be:

```
float[] blue = { 0f, 0f, 1f, 1f };
GL.Material(MaterialFace.FrontAndBack, MaterialParameter.Ambient, blue);
```

Keep in mind that any polygons you draw after calling GL.Material will be affected by the material settings until the next call to GL.Material
