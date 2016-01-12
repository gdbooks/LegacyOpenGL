# Understanding Coordinate Transformations
The OpenGL transformation pipeline transforms application vertices into window coordinates, where they can be rasterized. Like all 3D graphics systems, OpenGL uses linear algebra; it treats vertices as vectors and transforms them by using vector-matrix multiplication. The transformation process is called a __pipeline__ because geometry passes trough several coordinate systems on the way to window space. Each coordinate system serves a purpose for one or more OpenGL features.

For each stage of the transformation pipeline, this section describes the characteristics of that coordinate system, what OpenGL operations are performed there, and how to construct and control transformations to the next coordinate system in the pipeline.

 When rendering 3D scenes, vertices pass trough 4 types of transformations before they are rendered onto the screen:

* __Modeling Transform__ The modeling transformation moves objects around the scene and moves objects from local coordinates into world coordinates
* __Viewing Tranasform__ The viewing transformation specifies the location of the camera and moves objects from world coordinates into _eye coordinates_ (camera coordinates)
* __Projection Transform__ The projection transformation defines the viewing volume and clipping planes, it maps objects from eye coordinates to clip coordinates (-1 to +1 on all axis, clip coordinates are also called NDC)
* __Viewport transform__ The viewport transformation maps the clip coordinates (NDC) into the two-dimensional view port (the window on your screen)

A summary of the transformation pipeline:

![TRANSFORM](transform.png)

While these four transformations are standard in 3D graphics, OpenGL combines the model and view transforms into a single __modelview__ transformation. The viewport transform (also known as w divide) is done automatrically by OpenGL.

If yo look at the above diagram, there are only two matrices in the pipeline. These are the matrices that the state machine let's you specify, the __ModelView__ matrix and the __Projection__ matrix.

## A closer look at the pipeline

![PIPE](pipe.jpg)

This is the entire pipeline in a bit easyer to follow fashion. Let's take a closer look at what each section foes.

### Object Coordinates
Applications specify vertices in object coordinates. The definition of object coordinates is entireley up to the application. Some applications render scenes that are composed of many models, each created and specified in it's own individual coordinate system.

What this means is that each model is centered around it's own origin. When you export a 3D model from blender, that model was created around (0, 0, 0) and is said to be in model space.