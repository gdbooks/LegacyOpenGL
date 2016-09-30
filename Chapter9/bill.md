# Billboarding

Billboarding is a technique of making polygons always face the viewer. Why is this useful? Suppose we're making an outdoor scene. There are trees, LOTS of trees. They have LOTS of polygons. But far away trees have little detail to them. You could replace far away trees with textured quads that always face the camera.

Changes are the player wouldn't even notice this. I can't, here is an example:

![BILL1.jpg](BILL1.jpg)

In some games all foliage may be billboarded:

![BILL2.png](BILL2.png)

Flashes, Special FX, grenades and other objects are also good candidates for billboarding.

## Making a billboard

If every object has a matrix, making a billboard is easy. When we did frustum culling we looked at how to extract the forward, right and up vectors from a matrix. Billboarding is mainly that.

Take the cameras forward vector and invert it. Set the billboarded objects forward vector to this. We don't want the up axis of the billboard to change, but we do want to make sure that the right vector stays orthogonal. So take the billboards up and forward vectors and do a cross product. Set the billboards right vector to the result of the cross product.