#Shading
So far we've talked about and used GL.Color to set the color of each vertex. But how does OpenGL decide what color to use in the middle of a triangle (not at a vertex)? If all 3 vertices are the same color, then obviously the middle will be that color. But what happens if you use a different color for each vertex? List so:

```
GL.Begin(PrimitiveType.Triangles);
    GL.Color3(1.0f, 0.0f, 0.0f);
    GL.Vertex3(0.0f, 0.5f, 0.0f);
    
    GL.Color3(0.0f, 1.0f, 0.0f);
    GL.Vertex3(-0.5f, 0.0f, 0.0f);
    
    GL.Color3(0.0f, 0.0f, 1.0f);
    GL.Vertex3(0.5f, 0.0f, 0.0f);
GL.End();
```