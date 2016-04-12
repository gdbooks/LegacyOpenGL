#Cubemap reflection

Cube map reflections solve the major issue of planar reflections, somplex shapes. Any racing game you play where the cars have some sort of real reflection on the paint, or any time you see a complex shape reflect something, it's a cubemap. 

A cubemapped reflection will look like this:

![C](C_MAP.png)

So, how to we achieve this effect? First the scene has to be rendered 6 times from the point of view of the object, into a cube map. If you remember, when we bind textures, one of the texture targets to bind to is __CubeMap__. This is what a rendered map will look like:

![C2](C_MAP2.png)

Now comes the magic, when you render the object, OpenGL will take pixel color from the cube map. Imagine a cube that's just big enough to surroung the object being rendered (The green outline around the tea pot). The cube map is wrapped around this cube:

![ENV](ENV.jpg)

Now when a pixel is rendered, OpenGL figures out what direction the light on that pixel reflects from, and casts a ray out. Wherever that ray hits the enclosing cube, that pixel is used for the color of the object.

This is a super advanced graphics method, that i've never had to implement. Mainly because engines like Unity do the hard work for you, you just have to tag an object as cube-map refected. But at some point in time i will try to implement this for fun.