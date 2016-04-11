# Frustum Culling

Halfspace culling was a good start, but it's far from optimal. For one, you don't have a 180 degree field of view, the camera can usually only see about 60 degrees wide. Also, it does not take any of the other planes into account. Things might be out of view to the left, right, top, bottom, they may be too far or behind the camera.

This is where Frustum Culling comes in. We don't use frustum culling in conjunction with halfspace culling, INSTEAD OF halfspace culling we do frustum culling. 

This method revolves around the cameras frusutm:

![frustum.jpg](frustum.jpg)

Basically if an object is not inside the green frustum it will not be rendered. The math behind frustum culling isn't much different than the math behind Half Space Culling. It's the same thing actually!

To do Furstum culling, we first extract the 6 planes that make up the Frustum from the modelview matrix. Then, we do a half space test with ALL 6 planes. If the point is in front of all 6 planes then we render the object. If any one fails, we reject the object from being rendered.

## Extracting planes

Recall the plane equation

```
a * x + b * y + c * z + d = 0
```

To extract the planes of a frustum, you need the __viewProjection__ matrix, which you get by multiplying the projection and view matrices together. The plane values can be found by adding or subtracting one of the first three rows of the __viewProjection__ matrix from the fourth row.

* __Left Plane__ Row 1 (addition)
* __Right Plane__ Row 1 Negated (subtraction)
* __Bottom Plane__ Row 2 (addition)
* __Top Plane__ Row 2 Negated (subtraction)
* __Near Plane__ Row 3 (addition)
* __Far Plane__ Row 3 Negated (subtraction)

So, assuming that matrix ```mp``` is the view-projection matrix, you could get the left and right planes like so:

```cs
Matrix4 mv =  perspective * view;

Vector4 row1 = new Vector4(mv[0, 0], mv[0, 1], mv[0, 2], mv[0, 3]);
Vector4 row4 = new Vector4(mv[3, 0], mv[3, 1], mv[3, 2], mv[3, 3]);

Vector4 p1 = row4 + row1;
Vector4 p2 = row4 - row1;

Plane left = new Plane();
left.n = new Vector3();
left.n.X = p1.X;
left.n.Y = p1.Y;
left.n.Z = p1.Z;
left.d = p1.w;

Plane right = new Plane();
right.n = new Vector3();
right.n.X = p2.X;
right.n.Y = p2.Y;
right.n.Z = p2.Z;
right.d = p2.w;
```

## Frustum Culling

Once you have all the planes of a matrix, the actual Frustum Culling comes down to doing 6 half space tests. Assume that we have our frustum defined like so:

```cs
Plane[] frustum = new Plane[6];

void Init() {
  for (int i = 0; i < 6; ++i) {
    frustum[i] = new Plane();
  }
  
  ExtractPlanes(); // Fill the global frustum variable with planes
}
```

Now that we have a frustum, the culling code becomes very simple, first we use a helper function to test if a point is inside a frustum:

```cs
public bool PointInFrustum(Plane[] frustum, Vector3 point) {
    foreach (Plane plane in frustum) {
        if (Plane.HalfSpace(plane, point) < 0) {
            return false;
        }
    }
    return true;
}
```


## Implementation

As you can see above it's useful to make a Vector4 into a Plane, so lets add a new 