# Triangles
Triangles are generally the preffered polygon to be used in video games. There are several reasons for this:

* Triangles are always coplanar.
  * Because 3 points define a plane
* A triangle is always convex
  * That means it does not fold in on its-self 
  * Look up what a convex & concave polygon looks like if this confuses you
* A triangle will never cross over its-self
* A triangle will never have a hole in it

If you try to render a polygon that violates any of the above bullet points, the results will be unpredictable. Because any polygon can be broken down into a set of triangles (a process called triangulation), and rendering triangles is guaranteed to not have undefined behaviour, everything renderd in a video game is a triangle.

![TRIANGULATED](triangles.jpg)

Most 3D artists prefer to model with quads. Because of this every 3D modelling program has an option to triangulate models and export only triangles. I wasn't kidding, every 3D model that gets rendered on screen is broken down into triangles!

Drawing a 3D triangle isn't much more difficult than drawing a point, or a line. You just need to pass ```PrimitiveType.Triangles``` to ```GL.Begin``` and provide 3 vertices. Every 3 vertices will make one triangle!

```
// Draw a full screen triangle
Gl.Begin(PrimitiveType.Triangles);
    GL.Vertex3(-1, -1, 0);
    GL.Vertex3(1, -1, 0);
    GL.Vertex3(0, 1, 0);
GL.End();
```