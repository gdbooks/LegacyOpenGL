#Transformation Pipeline Deep Dive
The problem with most books is that they glaze over the transformation pipeline. This is bad! The Transformation Pipeline is the heart of 3D rendering! I really, really need you to understand how it works!

[This article](http://www.codinglabs.net/article_world_view_projection_matrix.aspx) does a much better job of explaining how the transformation pipeline works than i can. Bookmark it, print it if you have to; but read it and take it to heart. If you are still confused at the end of this chapter, reread that article and talk to me!

## More Visualization
To kind of sum up what the above article is saying, this is what's going on:

![PIPELINE](trans_pipe.png)

* Local coordinates are the coordinates of your object relative to its local origin; they're the coordinates your object begins in.
* The next step is to transform the local coordinates to world-space coordinates which are coordinates in respect of a larger world. These coordinates are relative to a global origin of the world, together with many other objects also placed relative to the world's origin.
* Next we transform the world coordinates to view-space coordinates in such a way that each coordinate is as seen from the camera or viewer's point of view.
* After the coordinates are in view space we want to project them to clip coordinates. Clip coordinates are processed to the -1.0 and 1.0 range and determine which vertices will end up on the screen.
* And lastly we transform the clip coordinates to screen coordinates in a process we call viewport transform that transforms the coordinates from -1.0 and 1.0 to the coordinate range defined by glViewport. The resulting coordinates are then sent to the rasterizer to turn them into fragments.

## What moves
The view matrix is interesting, it basically frames the world. The view matrix is what the camera sees. So, what is the view matrix? It's the INVERSE (this is why inverting matrices is so important!) of the cameras world position.

If you remember, multiplying anything by it's inverse will result in the identity vector. Which means, if we multiply the camera world position by the view matrix it puts the cameras world position at (0, 0, 0) or origin.

The model-view matrix is simply combining an objects world transform with the view matrix. Effectivley moving the object into a space where the camera is at origin looking down the z axis.

That's right, when you move trough a 3D world, as far as rendering is concerned the camera is not moving trough the world, the world is moving around the camera.