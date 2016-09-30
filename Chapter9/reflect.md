# Reflections

__Planar Reflections__ is the oldest reflection technique used in video games. It's used for things like water, glass and mirrors. Basically any surface that is somewhat of a plane. This technique will not work on complex shapes, like a car.

The technique is simple, you reflect your camera around the reflection plane and render the scene upside down:

![P1](PLANE_1.png)

The plane is just geometry rendered with opacity. This doesn't look great tough, you can see the upside down geometry! Because of this, the Stencyl buffer is used to clip the upside down image to the shape of the plane:


![R2](REFL_2.png)