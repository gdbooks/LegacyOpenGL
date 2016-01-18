#Using colors in OpenGL

When you pass primitives to OpenGL, it assigns colors to them by one of two methods: either by using lighting calculation or using the current color. When lighting is used the color for each vertex is computed based on a number of factors, including the position and color of one or more lights, the current materia, the vertex normal and so on. If lighting is disabled, then the current color is used instead. 

In RGBA mode, (this is the only mode we are going to be using), OpenGL keeps track of a primary and secondary color consisting of __R__ed, __B__lue, __G__reen and __A__lpha components. The alternate to RGBA mode is Color Index mode, which has not been used since the mid 90's.

## Setting the Color
In RGBA mode, you specify colors by indicating the intensity of the RGBA components. Just because you specify an alpha component doesn't mean OpenGL will draw the shape you are drawing transparent. You need to enable blending for that; something we will discuss later in this chapter.

The color components are usually expressed as a floating point number from 0 to 1, 0 being minimum, 1 being maximum. That means black is (0, 0, 0, 0) and white is (1, 1, 1, 1).

To specify the primary color in OpenGL you will use one of the many variations of:

```
void GL.Color3(...);
void GL.Color4(...);
```

The one we've been using takes 3 floats as an argument, i suggest you keep using that one. But the function does have many overrides. When using Color3, the alpha value is automatically set to 1. When using Color4, you must specify the alpha value.