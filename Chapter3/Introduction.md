#OpenGL States and Primitives
Now it's time to finally get into the meat of OpenGL! To begin to unlock the power of OpenGL, you need to start with the basics and that means understanding primitives. Before we start, we need to discuss something that is going to come up during our discussion of primitives and pretty much everything else from this point on. The OpenGL state machine.

The OpenGL state machine consists of hundreds of settings that affect various aspects of rendering. Because the state machine will play a role in everything you do, it's important to understand what the default settings are, how you can get information about the current settings and how to change those settings. Several generic functions are used to control the state machine, so we will look at those here.

## GL
Within OpenTK, there is a class called GL. All OpenGL functions are implemented as static function of the GL class. For example, to begin a primitive you would:

```
GL.Begin(BeginMode.Points);
```

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

The cap parameter represents the OpenGL capibility you want to turn on or off. ```GL.Enable``` turns it on, conversly ```GL.Disable``` turns it off