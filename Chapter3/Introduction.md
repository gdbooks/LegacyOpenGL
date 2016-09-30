#OpenGL States and Primitives
Now it's time to finally get into the meat of OpenGL! To begin to unlock the power of OpenGL, you need to start with the basics and that means understanding primitives. Before we start, we need to discuss something that is going to come up during our discussion of primitives and pretty much everything else from this point on. The OpenGL state machine.

The OpenGL state machine consists of hundreds of settings that affect various aspects of rendering. Because the state machine will play a role in everything you do, it's important to understand what the default settings are, how you can get information about the current settings and how to change those settings. Several generic functions are used to control the state machine, so we will look at those here.

## GL
Within OpenTK, there is a class called GL. All OpenGL functions are implemented as static function of the GL class. For example, to begin a primitive you would:

```
GL.Begin(PrimitiveType.Points);
```
