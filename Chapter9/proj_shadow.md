# Planar Shadows

Planar shadows are the cheapest way to achieve "nice" looking shadows in games. They look OK, but they have a lot of artifacts. First, an object can not shadow it's-self. Second, the shadow is literally a plane, so if the player stands on an elevated surface, like a stump, the shadow will float.

This method was popular on the PS2 because of how cheap it is to implement performance wise. The method is simple, construct a special matrix that collapses your 3D geometry down onto a 2D plane. Render this collapsed version of the model black. This becomes the shadow. Then, render the model normally.

The extra matrix multiplication has minimal impact. The only challenging part of this method is finding the matrix that will project your 3D model onto a 2D plane in 3D space!

I don't currently have this book with me, but [OpenGL Game Programming](http://www.amazon.com/OpenGL-Programming-Prima-Techs-Development/dp/0761533303) covers how to derive the matrix.

We may or may not implement this shadowing method, i'm not sure yet. It's kind of useless because of the artifacts it produces, and the next two shadowing methods we discuss both are capable of self-shadowing.