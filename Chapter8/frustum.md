# Frustum Culling

Halfspace culling was a good start, but it's far from optimal. For one, you don't have a 180 degree field of view, the camera can usually only see aout 60 degrees wide. Also, it does not take any of the other planes into account. Things might be out of view to the left, right, top, bottom, they may be too far or behind the camera.

This is where Frustum Culling comes in. We don't use frustum culling in conjunction with halfspace culling, INSTEAD OF halfspace culling we do frustum culling. 

This method revolves around the cameras frusutm:

![frustum.jpg](frustum.jpg)

Basically if an object is not inside the green frustum it will not be rendered. The math behind frustum culling isn't much different than the math behind Half Space Culling. It's the same thing actually!

To do Furstum culling, we first extract the 6 planes taht make up the Frustum from the modelview matrix. Then, we do a half space test with ALL 6 planes. If the point is in front of all 6 planes then we render the object. If any one fails, we reject the object from being rendered.

## Extracting planes

Recall the plane equasion

```
a * x + b * y + c * z + d = 0
```

To extract the planes of a frustum, you need the __viewProjection__ matrix, which you get by multiplying the projection and view matrices together. The plane values can be found by adding or subtracting one of the first three rows of the __viewProjection__ matrix from the fourth row.

* __Left Plane__
* __Right Plane__
* __Bottom Plane__
* __Top Plane__
* __Near Plane__
* __Far Plane__