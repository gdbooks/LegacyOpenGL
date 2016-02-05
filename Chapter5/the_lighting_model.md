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