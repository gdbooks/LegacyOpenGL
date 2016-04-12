# Terrain

Terrain's are created using height maps. Height maps are very popular in games. I've never had to implement a height map in my career, and i don't see that changing. Engines like Unreal or Unity have built in support for height-maps, and unless you are planning to make a game outside an engine that happens to be set out in the wilderness i don't think you'll need to implement this method.

So, how does height mapping work? Well you have a black and white image. White areas represent low height and black areas represent tall height. Then you have a large, subdivided plane. You map each vertex of the plane to a pixel on the height-map. You set the Y position of each vertex to whatever the heightmap wants you to set it to.

![H1](HM_1.gif)

![H2](HM_2.jpg)

As you can imagine, outdoor games like Skyrim or Far Cry use this method a lot. Indoor games have 0 use for the method.