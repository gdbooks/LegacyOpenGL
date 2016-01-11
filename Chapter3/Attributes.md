#Attributes
Earlier in this chapter you saw how to set up and query individual states from OpenGL. Now lets take a look at how to save and restore the values of a set of related state variables with a single command.

An _attribute group_ is a set of related state variables that OpenGL classifies into a group. For example, the line group consists of all the line drawing attributes, such as width, stipple pattern and anti-aliasing. The polygon group consists of the same set of attributes, just for polygons. You can save and restore these values by using the ```GL.PushAttrib``` and ```GL.PopAttrib``` functions. These are the signatures

```
void GL.PushAttrib(AttribMask);
void GL.PopAttrib();
```

```PushAttrib``` saves all of the attributes for an attribute group specified by it's argument (an ```AttribMask``` enum). To be more specific, it pushes all the attributes onto a stack known as the _attribute stack_. ```PopAttrib``` restores the previous attributes by poping the top of the _attribute stack_. 

The argument to PushAttrib is a bit-msak. The arguments you pass in can be combined with a bitwise __OR__ ( __|__ )The following ```AttribMask``` values are valid:

* __AllAttribBits__ All opengl state variables in all attribute groups
* __EnableBit__ Enable state variables
* __FogBit__ Fog state variables
* __LightingBit__ Lighting state variables
* __LineBit__ Line state variables
* __PointBit__ Point state variables
* __PolygonBit__ Polygon state variables
* __TextureBit__ Texturing state variables

It's common to set a default state for everything in your initialize function. Then, in your render function you push and pop attribs in order to set custom states and restore the defaults. For example, something like this

```
void RenderModel() {
    GL.PushAttrib(AttribMask.PolygonBit | AttribMask.TextureBit);
    
    if (renderWireframe) {
        GL.
    }
    
    DoTexturing();
    GL.Begin(GL.Triangles();
    foreach(Traignle t in triangles) {
    
    }
    GL.End();
}
```