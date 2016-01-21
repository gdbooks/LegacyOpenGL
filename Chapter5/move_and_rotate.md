# Moving and rotating
What do you need to do to make a light move around? Think about how you would make any other object in the world move around. One way is to set the position of the object after you translate or rotate it. You can do the same thing with lights. When you call ```GL.Light``` to define the position or direction of a light, the information you specify is in local space. It will be transformed by the current modelview matrix.

### Static Lights
For static lights, lights that never move it makes sense to position the light after you set up the camera (after calling LookAt for example) but before applying any other transformations to the modelview matrix. We rendered our reference grid like this. No matter where the objects in the scene or the camera moved, the reference grid is static at origin. Same thing would happen for lights.

### Perspective Lights
How about perspective lights? What's a perspective light? It's a light that stays fixed relative to the eye (camera) position. For instance, when you sit in a car, the headlights are perspective lights. They stay in more or less the same position relative to your eyes. In this case you would set the modelview matrix to indentity, then define your light at the origin, and then continue transformations as you normally would

```
GL.MatrixMode(MatrixMode.ModelView);
GL.LoadIdentity();

// Position the cars lights at origin, this can be anywhere,
// it doesn't have to be at origin. In the real world it would
// be up a few units on the z axis, and down a few on the y axis
float[] pos = {0,0,0,1}
GL.Light(LightName.Light0, LightParamater.Position, pos);

// Continue rendering as normal
Matrix4 lookAt = Matrix4.LookAt(eyePosition, lookTarget, new Vector3(0.0f, 1.0f, 0.0f));
GL.MulMatrix(lookAt.OpenGL);
```

### Dynamic Lights
But what if you had a moving object, like a flash-light. It would need to be a child of a characters arm if it's being held (thik of Mr.Roboto and how we did his feet).


```
GL.MatrixMode(MatrixMode.ModelView);
GL.LoadIdentity();

Matrix4 lookAt = Matrix4.LookAt(eyePosition, lookTarget, new Vector3(0.0f, 1.0f, 0.0f));
GL.MulMatrix(lookAt.OpenGL);


```