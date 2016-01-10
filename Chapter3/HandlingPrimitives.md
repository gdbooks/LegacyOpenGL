#Handling Primitives
So, what are primitives? Simply put primitives are basic geometric entities such as points, lines and triangles.

You will be using thousands and thousands of these primitives to make your games, so it is important to know how they work. Before we get into specific primitive types, we need to talk about a few OpenGL functions that you will be using often for working with primitives. The first is ```GL.Begin```. The function has the following prototype:

```
void GL.Begin(BeginMode mode);
```