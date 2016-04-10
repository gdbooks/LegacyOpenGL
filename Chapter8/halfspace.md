# Half Space Culling

So far we've rendered everything, no matter what. And that's ok, for small demos. But even the simplest game could not get away from that. Think of a modern game, like Assasins Creed. They have MILLIONS of objects per scene. Rendering them all would make the game unplayable.

So what's the solution? Don't render what you can't see! This is called __view culling__. We cull out all invisible objects, and NEVER actually render them. Over the years view culling has gotten increasingly more complex, but we're going to start off with the simplest implementation, __half space culling__.

The theory behind half space culling is simple. Construct a plane at the position of the camera, the plane will span the cameras X and Y axis, and will face in the direction of it's Z axis. Then, test all models we are rendering against this plane. Only render models that are in front of the plane. This way, we don't render anything behind the camera!

How do we actually test what side of the plane a point is on? Using the game programmers bread and butter, the __dot product__! If you want to google how to do this, you should be looking for [Half Space Test](https://www.google.com/#q=half+space+test).

## Plane

## Test

## Implementation