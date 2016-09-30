# Outlines

Outlines are a pretty classic effect in video games. Sometimes they are core to a games style, just look at borderlands:

![B](BLDS.png)

The way borderlands does outlines is interesting, it's an old hack. First, you turn off Z-Write. Then you scale your model to 1.1 on all axis. Turn off texures and lighting, and render the model all black. Next, re-enable all the stuff that was disabled, and render the model normally at a scale of 1. The result is an outline!

To this day the above method is probably the fastest way to render an outline, but there are more precise ways to do it too. Sometimes you need precision! 

If you need precise outlines you must have access to a models triangles. You have to have a list of edge objects, an edge is the line that connects two triangles. And you need a light direction. If the dot product of one of the triangles of an edge is < 0, but the other is > 0 then you know that edge is a part of the models outline.
