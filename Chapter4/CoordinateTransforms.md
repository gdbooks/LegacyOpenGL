# Understanding Coordinate Transformations
Look around for a moment, imagine that you have a camera in your hands and you are taking photographs of your surroundings. You probably have a desk, a computer, maybe some books around you. Each of these objects has a shape in a __local coordinate system__ which is unique for every object, is centered on the object and doesn't depend on any other objects. They also have some position and orientation is world space. You have a position and orientation in world space as well. The relationship between the positions of these objects around you and your position and orientation determines weather or not objects are in front of you or behind you.

If you are taking pictures of these objects, the lense of the camera also has some effects on the final outcome of the pictures. A zoom lense might make objects appear closer to your position. You am, click and the picture is "rendered" onto the camera film. Your camera also has a size and resolution which define how the final picture is rendered. 

The final image you see in a picture is a product of how each objects position, your position, your camera lenses position and its settings interact to map your surrounding objects three-dimensional features onto a two-dimensional picutre.

Transformations work the same way. They allow you to move, rotate and manipulate objects in a 3D world, while also allowing you to project 3D coordinates onto a 2D screen. Altough transformations seem to modify an object directly, in reality, they are merely transforming the objects local coordinate system into another coordinate system. When rendering 3D scenes, vertices pass trough 4 types of transformations before they are rendered onto the screen:

* __Modeling Transform__ The modeling transformation moves objects around the scene and moves objects from local coordinates into world coordinates
* __Viewing Tranasform__ The viewing transformation specifies the location of the camera and moves objects from world coordinates into _eye coordinates_ (camera coordinates)
* __Projection Transform__ The projection transformation defines the viewing volume and clipping planes, it maps objects from eye coordinates to clip coordinates (-1 to +1 on all axis, clip coordinates are also called NDC)
* __Viewport transform__ The viewport transformation maps the clip coordinates (NDC) into the two-dimensional view port (the window on your screen)

while these four transformations are standard in 3D graphics, OpenGL combines the model and view transforms into a single __modelview__ transformation. We will discuss this soon.

A summary of the transformation pipeline:

![TRANSFORM](transform.png)