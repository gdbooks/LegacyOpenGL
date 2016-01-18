#Using colors in OpenGL

When you pass primitives to OpenGL, it assigns colors to them by one of two methods: either by using lighting calculation or using the current color. When lighting is used the color for each vertex is computed based on a number of factors, including the position and color of one or more lights, the current materia, the vertex normal and so on. If lighting is disabled, then the current color is used instead. 

In RGBA mode, (this is the only mode we are going to be using), OpenGL keeps track of a primary and secondary color consisting of __R__ed, __B__lue, __G__reen and __A__lpha components. The alternate to RGBA mode is Color Index mode, which has not been used since the mid 90's.

## Setting the Color
In RGBA mode, you specify colors by indicating the intensity of the RGBA components. Just because you specify an alpha component doesn't mean OpenGL will draw the shape you are drawing transparent. You need to enable blending for that; something we will discuss later in this chapter.