# Frustum Culling

Halfspace culling was a good start, but it's far from optimal. For one, you don't have a 180 degree field of view, the camera can usually only see aout 60 degrees wide. Also, it does not take any of the other planes into account. Things might be out of view to the left, right, top, bottom, they may be too far or behind the camera.

This is where Frustum Culling comes in. We don't use frustum culling in conjunction with halfspace culling, INSTEAD OF halfspace culling we do frustum culling. 

This method revolves around the cameras frusutm:

![frustum.jpg](frustum.jpg)

Basically if an object is not inside the green frustum it will not be rendered. As you can imagine, the math behind this method of culling is more intense than the math behind halfspace culling.

