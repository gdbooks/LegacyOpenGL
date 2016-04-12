# Ambient Occlusion

Ambient Occlusion is the small shadows that ambient lighting casts. When you're in a well lit room look at the corner of a wall. It's usually a little bit darker than the rest of the wall. 

A better example is the wrinkles on a persons face. They cast suddle shadows. Because wrinkles are expressed using normal maps, there is no geometry for them to cast shadows. This is where ambient occlusion maps come into play. We basically render out all the suddle shadows, then apply them with a texture.

![AB1](AB1.png)

Ambient maps look cool, they look like the scene is rendered into some solid dark white material. Modern games apply ambient occlusion maps to entire cities:

![AO_C](AO_C.jpg)