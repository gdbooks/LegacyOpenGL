# Implementation

Now that we have talked a bit about how particle systems work, let's go ahead and actually implement one. I'll get you started by walking you trough the base classes needed to create particles. After the base classes are made i'll also walk you trough how to extend the system to create a simple snowing particle effect. 

We're going to use this snow texture:

![SN](snow.png)

The final scene we will have at the end of the implementation will look like this:

![SNOWIN](snowin.png)

Notice that the snow falls above the world, but below the UI. This is because of where we will position the snow's draw call.