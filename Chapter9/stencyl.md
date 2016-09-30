# Stencyl Shadows

This is perhaps the crispest way to implement shadows, it was made famous by DOOM3. Take a look at the detail of their shadow tech:

![SHADOW](SHADOW.jpg)

Stencyl shadows are also called volume shadows. To get a stencyl shadow you first find the outline of an object:

![S0](S0.png)

Once you have the outline, you must extrude it in the direction of the light. In theory this extrusion should be to infinity, but really it's just a really big number (50,000)

![S1](S1_.png)

This extruded geometry is never rendered. What you do is test for intersections. Where this geometry instersects other geometry, you draw a shadow.

![S2](SO_2.png)

Above i peeled back the color to show where the shadow volume intersects the room geometry. Below is the final image. Notice how the shadow volume also covered half of the object casting the shadow, so the object is __self shadowed__

![S3](SO_3.png)

## Stencyl?

So, why is this method called stencyl shadows? Well, you don't actually do any intersection testing. You "Render" the scene into the stencyl buffer (Not color buffer). Then you "Render" the shadow volume into the stencyl buffer too. 

If you render the shadow volume and it draws on-top of other geometry we know an intersection happened.

It's a pretty complex topic, it took me about 5 years to actually implement. The best part is, no games actually use this method of shadowing anymore!

Anyway, i wrote an article about it, with some sample c++ code: [https://github.com/gszauer/GLExperiments/tree/master/ZFail](https://github.com/gszauer/GLExperiments/tree/master/ZFail)


