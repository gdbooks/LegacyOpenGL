# Directional lights
Directional lights are perhaps the easyest to program. They are simply a direction that the light comes from, like sunlight. Let's give it a go.

Follow along, make a new scene for each sub-section here.

## Adding one light

The first thing we need to do is enable lighting and light 0. This isn't necesarley something that needs to happen every frame, so lets handle it in the initialize function.

```
public override void Initialize() {
    grid = new Grid(true);
    GL.Enable(EnableCap.DepthTest);
    GL.Enable(EnableCap.CullFace);
    Resize(MainGameWindow.Window.Width, MainGameWindow.Window.Height);

    GL.Enable(EnableCap.Lighting);
    GL.Enable(EnableCap.Light0);
}
```

Now we are ready to do some lighting! I'm going to set light 0 to be a blue light comming from the upper front of the scene. For now, the light is static (it doesn't change from frame to frame) so it's safe to configure in the Init function as well.

```
public override void Initialize() {
    grid = new Grid(true);
    GL.Enable(EnableCap.DepthTest);
    GL.Enable(EnableCap.CullFace);
    Resize(MainGameWindow.Window.Width, MainGameWindow.Window.Height);

    GL.Enable(EnableCap.Lighting);
    GL.Enable(EnableCap.Light0);

    float[] white = new float[] { 0f, 0f, 0f, 1f };
    float[] blue = new float[] { 0f, 0f, 1f, 1f };

    GL.Light(LightName.Light0, LightParameter.Position, new float[] { 0.0f, 0.5f, 0.5f, 0.0f } );
    GL.Light(LightName.Light0, LightParameter.Ambient, blue );
    GL.Light(LightName.Light0, LightParameter.Diffuse, blue );
    GL.Light(LightName.Light0, LightParameter.Specular, white );
}
```

That's all there is to it. If you run your code, it should look like this:

![DIR1](directional3.png)

## Light direction

It may not be obvious from the previous example, but OpenGL shades (lights) objects independently! That is, no object casts a shadow on another object. When the light is comming from the top, like in the above scene it's easy to miss that. So, let's change the light direction to come from the negative Y axis!

```
    GL.Light(LightName.Light0, LightParameter.Position, new float[] { 0.0f, -0.5f, 0.5f, 0.0f } );
```

Let's take a look at what that looks like:

![DIR2](directional2.png)

As you can see the shading on the primitives has changed, the floor is the most noticable. In the first example, the floor got a LOT of light, in the second example, the floor is unlit. This happens because in the second example the floor is facing away from the light.

In the real world, the floor would stop light from reaching the objects. In OpenGL each object is shaded and lit indevidually, like if no other object was in the scene. It is possible to add code to support shadows, but it's a complicated process that we will talk about later.

## Two Lights