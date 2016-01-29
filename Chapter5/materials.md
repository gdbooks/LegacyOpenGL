#Materials
Lights account for only half of the properties of what we see. The other half is materials. So far every object we've lit has been treated as a mostly white object. Reflecting light almost perfectly. But the real world doesn't work like that. Red light on a blue object might make that object look blue. Some objects are shiny, while other objects are matte. Objects can be assigned these properties trough materials.

OpenGL approximates material properties based on the way the material reflects red, green and blue light. For instance, if you have a surface that is green, it will reflect all incomming green light, absorbing all blue and red light. If you where to place this surface under a purple light, it would appear black. This is because the surface only reflects green light. It absorbs the red and blue components of the purple light, there is no green light left to reflect. 

If you where to place a green surface under a white light, it would still look green. This is because white is a combination of all colors. The surface will absorb the red and blue components of the light, and reflect the green.

Finally, placing this surface under a green light, it would still look green. There is no color for the surface to absorb, and all incomming green is reflected back at the viewer. It would look exactly the same as if we had placed the surface under a white light.

Materials have the same color terms as lights: __ambient__, __diffuse__ and __specular__. These three properties combined will determine how much light a material reflects.