# Half Space Culling

So far we've rendered everything, no matter what. And that's ok, for small demos. But even the simplest game could not get away from that. Think of a modern game, like Assasins Creed. They have MILLIONS of objects per scene. Rendering them all would make the game unplayable.

So what's the solution? Don't render what you can't see! This is called __view culling__. We cull out all invisible objects, and NEVER actually render them. Over the years view culling has gotten increasingly more complex, but we're going to start off with the simplest implementation, __half space culling__.

The theory behind half space culling is simple. Construct a plane at the position of the camera, the plane will span the cameras X and Y axis, and will face in the direction of it's Z axis. Then, test all models we are rendering against this plane. Only render models that are in front of the plane. This way, we don't render anything behind the camera!

How do we actually test what side of the plane a point is on? Using the game programmers bread and butter, the __dot product__! If you want to google how to do this, you should be looking for [Half Space Test](https://www.google.com/#q=half+space+test).

## Plane

A _plane_ in 3D space can be thought of as a flat surface extending indefinitely in all directions. It can be described in several different ways. For examply by:

* Three points not on a straight line
* A normal, and a point on the plane
* A normal, and a distance from origin

Let's define a plane class in code:

```cs
class Plane {
  public Vector3 n; // Plane normal. Points x on the plane satisfy Dot(n, x) = d
  public float d; // Distance from origin, d = Dot(n, p), for a given point p on the plane
}
```

So we've chosen to represent a plane as a normal and a distance from origin. We can calulate this plane, from 3 (3D) points

```cs
static Plane ComputePlane(Vector3 a, Vector3 b, Vector3 c) {
  Plane p = new Plane();
  p.n = Vector3.Normalize(Vector3.Cross(b - a, c - a));
  p.d = Dot(p.n, a);
  return p;
}
```

When two planes are not parallel to each other, they intersect in a line. Similarly, three planes (two not parallel to each other) intersect at a 3D point.

##Theory

The theory of a half space test is simple. Given a plane, with a normal. What is the angle between the plane and the point!

Remember how the dot product works! If we take the planes right vector, and a vector from the plane to the point we are testing we can do a dot product between the two. 

The result of this dot product is the half space test, remember if the result of the dot product is:

* 0, the vectors are parallel
* positive, the vectors are less than 90 degrees apart
* negative, the vectors are more than 90 degrees apart

So, if the dot product of the planes vector and the vector from the plane to the point is:

* 0, the point is on the plane
* positive, the point is in front of the plane
* negative, the point is behind the plane

## Half Space Test

Before we talk about the half space test, let's take a quick look at the plane equasion (Equasion of a plane).

```
a * x + b * y + c * z + d = 0
```

Where, ABC is the normal of the plane, D is the distance of the plane from origin and XYZ is some point. This equasion states that if point XYZ is on the plane, the result of the above equasion is 0. If a point is in front of the plane the result will be a positive number (distance from the plane) and if the point is behind the plane the result will be a negative number.

Because you already know what ABC and D are, you could just plug int XYZ into the above equasion and return the result. Try implementing this function in code

```cs
static int HalfSpace(Plane p, Vector3 v) {
  // TODO: Return result of plane equasion
}
```

That code lets you perform a half space test between a plane and a point. But earlyer i mentioned that the half space test would be done using a dot product, what gives? Look at the plane equasion, it's the expanded form of a 4D dot product. The above function COULD be expressed as:

```cs
static int HalfSpaceDotProduct(Plane p, Vector3 v) {
  Vector4 v1 = new Vector4(p.n.X, p.n.Y, p.n.Z, p.d);
  Vector4 v2 = new Vector4(v.X, v.Y, v.X, 1);
  return Vector4.Dot(v1, v2);
}
```

Note that the W component of the plane vector is the d term of the plane, while the W component of the point vector is 1. This is because if the W component of the point vector was 0 it would cancel out the planes W term, breaking the equasion.

Whichever method (dot product, or plane equasion) you want to use is up to you. But the straight plane equasion method is simpler.

## Alternate representation

There are other ways of representing planes, and doing the half space test. For example, a plane can be represented by a normal and any point on the plane. 

If this was the case, the half space test would in involve subtracting the plane point from the point you are testing to get a direction vector, then doing a dot product with the plane normal and direction vector.

Because the dot product returns a number that is relative to an angle this representation might be easyer to understand. 

* If the dot of the plane normal and direction vector is negative, then the angle is greater than 90 degrees
* if it is 0 then the angle is exactly 90 degrees (on the plane)
* If it is positive, then the angle is less than 90 degrees

![figure16-20.jpeg](figure16-20.jpeg)

## Implementation

Cool, now that we understand the half space test (If you don't call me on skype) let's go ahead and implement it in our test scene. I'm going to be working in the camera scene we just implemented in the last section.

Implement a ```Plane``` class, and the ```HalfSpace``` function. For ease of use, make the ```HalfSpace``` function a static member of the ```Plane``` class. Also, make the ```ComputePlane``` function a static member of the plane class.

Inside the example application, after all the camera variables, add a new ```Plane``` variable:

```cs
... old code
protected Vector3 CameraPosition = new Vector3(0, 0, 10);
protected Vector2 LastMousePosition = new Vector2();
protected Matrix4 viewMatrix;
// NEW
Plane cameraPlane = null;
```

We're going to construct this new plane inside the ```Move3DCamera``` function. We know we can construct a plane using 3 points, so what 3 points of the camera can we use to make a plane?

Well we know the forward and right and up of the camera. We can use the right and up to construct a camera plane whose normal will be the camera forward. But that's only two vectors, what about the third one? We can invert the right vector to get a left vector. 

This means we can create the camera plane using the camera world matrix left, right and up planes. Let's see how this would look in code:

```cs

```