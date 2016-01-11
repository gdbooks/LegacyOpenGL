#Drawing polygons in 3D
Altough you can (and will) do some interesting things with points and lines, polygons will give you the most power to create immersive 3D worlds. With that being said, the rest of Chapter 3 will be spend discussing polygons. Before we get into the specific polygon types supported by OpenGL (Triangles, Quadralatirals, Generic Poligons) we need to discuss a few things that pretain to all polygon types.

You draw all polygons by specifying several points in 3D space. These points specify a region that is then filled with color. At least, thats the default behaviour. However as you probably expect by now, the state machine controls the way in which a polygon is drawn. Trough the state machine you can change the drawing behaviour. Use the following function to change the way polygons are drawn:

```
void GL.PolygonMode(MaterialFace, PolygonMode);
```

OpenGL handles the front and back faces of polygons seperateley. A front face is any face of a polygon that is facing the camera. A back face is any face of a polygon that is facing away from the camera. 

As a result, when you call ```GL.PolygonMode``` you need to change the face to which the change will apply. You do this by passing either ```MaterialFace.Front```, ```MaterialFace.Back``` or ```MaterialFace.FrontAndBack``` as the first paramater.

The following values are valid for the second paramater:

* __PolygonMode.Point__ Each vertex is rendered as a single point. This basicaly produces the same effect as calling ```GL.Begin``` with ```Lines``` as the argument.
* __PolygonMode.Line__ This will draw the edges of the polygon as a set of lines. This is _similar_ to calling ```GL.Begin``` with ```LineLoop```
* __PolygonMode.Fill__ This is the default state, which renders the polygon with the interior filled.  This is the only state in which polygon smoothing takes effect.

If you want to see the back facing polygons of a model, you could render the front facing polygons as lines, and the back facing polygons solid. You can achieve this like so:

```
GL.PolygonMode(MaterialFace.Front, PolygonMode.Line);
GL.PolygonMode(MaterialFace.Back, PolygonMode.Fill);
```

If you have not changed the polygon mode for back facing poly's, the second line is redundant. This is because the default fill mode is ```Fill```. But better safe than sorry.

