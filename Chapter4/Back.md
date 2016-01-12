#And we're back
After taking a little deep-dive to explore the transformation pipeline in depth, let's finish our discussion of coordinate systems.

When you are writing your 3D programs, remember that these transformations execute in a specific order. The modelview transformation execute before the projection transformations; However the viewport can be specified any time and OpenGL will automatically apply it at the right time.

Let's re-visit this figure:

![TRANSFORM](transform.png)

## Eye coordinates
One of the most critical conepts to transformations and viewing in OpenGL is the concept of the __camera__, or eye coordinates. In 3D graphics, the currrent viewing transformation matrix, which converts world coordinates to eye coordinates defines the cameras position and orientation. In contrast, OpenGL converts world coordinates to eye coordinates with the __modelview__ matrix. 

When an object is in eye coordinates, the geometric relationship between the object and the camera is known, which means our ojects are positioned relative to the camera. This is important to render correctly. 

To get an object into eye coordinates, multiply it with the inverse of the cameras world transform (the view matrix)

This is the camera's world position, after it is multiplied with the view matrix (inverse of the cameras model matrix)

![CAM](cam.png)

## Viewing Transformations
We know that the view matrix is the inverse of the cameras world transformation. But how do we position the camera? How to we get the initial world transform for a camera? There are two general ways of doing this

* By using a function such as ```LookAt```, this will specify a position and a direction to look and return the inverse of a matrix created from that.
* By treating the camera as any other object and giving it a model matrix. This model matrix can then be inverted to get the view matrix.

Both of these methods actually use the same math, so it's going to be pretty easy to work with them. We will walk trough how to construct a ```LookAt``` function later.

## Modeling transformation