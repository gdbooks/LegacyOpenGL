## The view matrix 
Combining translation, rotation and scale creates a transformation matrix. Sometimes also called the model matrix, or model to world matrix. But we're editing the mode-view matrix! So what is the view matrix?

As discussed earlyer, the view matrix is the inverse of the cameras world position! There are two things you need to know about your camera in world space, where it is (it's position) and which what it's looking (it's orientation).

Setting the view matrix with the fixed function pipeline is challenging at best, this is mainly because there is no fixed function equivalent of interting a matrix! 

This means if your camera is located at (90, 20, 30) and rotated to (7, 1, 0, 0) you would have to do the following commands:

```
// Negate the translation of the camera
GL.Translate(-90, -20, -30);
// Negate the rotation of the camera
GL.Rotate(-7, 1, 0, 0);
```

Well, that wasn't so bad... Was it? No, but your world position for the camera is NEVER going to be that simple. But there is an easy way to create the world matrix of the camera, we just won't cover it until later (we cover it in the custom matrices section).

Until then, copy/paste and use [this](https://gist.github.com/gszauer/91038dbb010755d719de) function. It is defined as follows:

```
void LookAt(float eyeX, float eyeY, float eyeZ, float targetX, float targetY, float targetZ, float upX, float upY, float upZ);
```

LookAt is actually a pretty standard OpenGL function. It's not included in the core of OpenGL, but it is so useful every programmer knows how it works!

The functions first three arguments are the position of the camera. So, if the camra is at (5, 3, 1) then the first 3 argumetns would be 5, 2, 1.

The second 3 arguments are the target that the camra is looking at. So if the camera is looking at world coordinates (0, 0, 0) then those would be the second three arguments.

The last three arguments are up. Which direction is up? Since the camera is in world space, and in world space positive Y is up, the last 3 arguments are almost always going to be 0, 1 0.

## Example