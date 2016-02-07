# Blending

OpenGL allows you to blend incoming framents with pixels already on screen. Which enables you to introduce effects such as transparency into your scenes. With transparency you can simulate water, windows, glass and other objects in the world you can normally see trough.

The term **fragment** might be new to you, but it will come up several times from here on out. This is a good time to discuss what it is.

As OpenGL processes the primitives you pass to it, during the rasterization stage it breaks them down into pixel size chunks called fragments. Sometimes the terms pixel and fragment are used interchangeably, but there is a subtle difference. A pixel is a value that gets written to the color buffer. A fragment is a piece of a primitive that _might_ eventually become a pixel after it is depth-tested, alpha-tested, blended, combined with a texture,  combined with another fragment or it may just become a pixel without being modified.

So, essentially a fragment is data that can become a pixel, but isn't necesarley a pixel yet.

Remember the alpha value we've been ignoring all this time? Well, now that we're talking about blending it's time to learn how to use it. When enabling blending, you are telling OpenGL to combine the color of the incoming primitive with the color that is already in the frame buffer, and store that combined color back into the frame buffer.

Blending operations are typically specified with the RGB values representing color, and the Alpha value representing opacity. But other combinations are possible. From now on,we will refer to the incomming fragment as SOURCE, and the pixel that is already in the frame buffer as DESTINATION.

To enable blending, call the ```GL.Enable``` method with ```EnableCaps.Blend``` as it's argument. Just enabling blending isn't enough to make blending happen. You must also tell OpenGL what formulat to use to blend pixel colors. You do this by defining a blending function for the source and destination fragments.

You can call the function ```GL.BlendFunc``` to define the source and destination blend factors. Blend factors are values in the 0 to 1 range, that are multiplied by the RGBA components of both the source and destination colors. The resulting colors are then combined (usually by adding them) and clamped to the range of 0 to 1. 

```
void GL.BlendFunc(BlendingFactorSrc sourceFactor, BlendingFactorDest destFactor);
```

The first argument is the source blend factor, the second argument is the destination blend factor. Both enums contain the same enumerated values, with the exception of OneMinusSrdColor which is only available as a source blend factor and OneMinusDstColor, which is only available as a destination blend factor. The enumerated values are:

* __Zero__ Each component is multiplied by 0, effectivley setting the color to black
* __One__ Each component is multiplied by 1, leaving the color unchanged
* __SrcColor__ Each component is multiplied by the coresponding component
* __OneMinusSrcColor__ Each component is multiplied by 1 - source color
* __DstColor__ Each component is multiplied by the coresponding component in the destination color
* __OneMinusDstColor__ Each component is multiplied by 1 - destination color
* __SrcAlpha__ Each component is multiplied by the source alpha value
* __OneMinusSrcAlpha__ Each component is multiplied by 1 - source alpha value
* __DstAlpha__ Each component is multiplied by the destination alpha value
* __OneMinusDstAlpha__ Each component is multiplied by 1 - dest alpha value
* __ConstantColor__ Each component is multiplied by a constant color, set using ```GL.BlendColor```
* __OneMinusConstantColor__ Each component is multiplied by 1 - constant color, set using ```GL.BlendColor```
* __ConstantAlpha__ Each component is multiplied by an alpha value set using ```GL.BlendColor```
* __OneMinusConstantAlpha__ Each component is multiplied by 1 - a constant alpha value set using ```GL.BlendColor```
* __SrcAlphaSaturate__ Multiplies the source color by the minimum of source and (1 - destination). The  alpha value is not modified. Only valid as the source blend factor

The default values are One for the source and Zero for the destination. Which produces the same results as not using blending at all.

Many different effects can be created with these blending factors, some o which are more useful in medical imaging than games. To better understand how this voodoo works, let's look at the application that is most often used in games: transparency!