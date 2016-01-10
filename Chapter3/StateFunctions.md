## State functions 
OpenGL provides a number of multipurpose functions that allow you to query the OpenGL state machine. Most of these functions begin with ```GL.Get...```. The most generic of these functions will be covered here, the rest will be covered as we use them in later chapters.

###Querying numeric states
There are four general purpose functions that allow you to retrieve numeric (or Boolean) values in the OpenGL state machine:

* ```bool GL.GetBoolean(GetPName pname)```
* ```double GL.GetDouble(GetPName pname)```
* ```float GL.GetFloat(GetPName pname)```
* ```int GL.GetInteger(GetPName pname)```

[GetPName](http://www.opentk.com/files/doc/namespace_open_t_k_1_1_graphics_1_1_open_g_l.html#a4a17062512d656f51b8bc8d880372689) is an enumeration containing all of the states we may want to query. In each of the function, we query the state represented by it's name.

Of course determining the current state machine settings is interesting, but not nearly as interesting as being able to change those settings. Contrary to what is expected, there is no ```GL.Set...``` or similar function for setting state machine values. Instead there is a variety of more specific functions, which we will discus as they become relevant.

###Enabling and disabling states
We know how to find out the states in the OpenGL state machine, so how do we turn states on and off? Enter the ```GL.Enable``` and ```GL.Disable``` functions.

```
void GL.Enable(EnableCap enableCap);
void GL.Disable(EnableCap enableCap);
```

The cap parameter represents the OpenGL capibility you want to turn on or off. ```GL.Enable``` turns it on, conversly ```GL.Disable``` turns it off. Some of the more common caps are ```EnableCap.Blend``` (for texture blending), ```EnableCap.Texture2D``` (for texturing) and ```EnableCap.Lighting``` for lighting.

###GL.IsEnabled
Often times you just want to find out weather a particular OpenGL capacity is on or off. Altough this can be done with ```GL.GetBoolean``` it's easyer to do with ```GL.IsEnabled```.

```
bool GL.IsEnabled(EnableCap enableCap);
```

It's easyer to use this function because it takes an ```EnableCap``` enum as it's argument, instead of the ```GetPName``` enum that ```GL.GetBoolean``` takes.

###Querying string values
You can find out details of the OpenGL implementation being used at runtime with the following function

```
string GL.GetString(StringName name)
```

The ```StringName``` enum only has the following five properties:

* __Vendor__: The returned string indicates the name of the company whose OpenGL implementation you are using. For example, the vendor string for API drivers is _API Technologies Inc._
* __Renderer__ The string contains information that usually reflects the hardware being used. For example _RADEON 9800 Pro X86/MMX/3DNow!/SSE_.
* __Version__ This string contains the version number formatted either as: ```major.minor``` or ```major.minor.patch```, possibly followed by additional information. 
* __Extensions__ The string returned contains a space-delimited list of all of the OpenGL extensions your driver supports. This is not very useful for OpenTK.
* __ShadingLanguageVersion__ this is the version of GLSL your graphics card supports. We will not be dealing with shaders for a while, for now this is safe to ignore.

It's good practice to print out the __Vendor__, __Renderer__ and __Version__ strings on seperate lines in your Initialize function. This will give you a feel for what hardware you are running on.