# An overview of texture mapping

In a nutshell _texture mapping_ allows you to attach images to polygons in order to provide more realistic graphics. As an example, you could apply an image of the front of a book to a rectangular polygon; the polygon would appear as a visual representation of the front of the book. Another example would be to take a map of earth and texture map it to a sphere. You then have a 3D visual representation of earth. Nowdays, texture maps are used everywhere in 3D graphics, you're gonna have a hard time finding a game that doest use textures.

Texture maps are composed of rectengular arrays of date, each element of these arrays is called a __texel__. Altough they are rectangular arrays, textures can be mapped to non-rectangular objects. 

In a picture, each pixel has 4 components (RGBA). Each of these components would be 1 entry in the rectangular texture array. Each component is represented by a value of 0 to 255. So the size of a texture is: ```width * height * 4 * sizeof(char)``` Each pixel of that texture is called a __texel__. 

Why __texel__, why not pixel? As we map a texture to a 3D object, and the 3D object to screen, a single pixel in the source texture could occupy multiple pixels on screen (Because of wrapping around a 3D object that could be scaled, rotates, skewed, etc...). Because the mapping is not 1 to 1, the source of the image comes from texel coordinates and the destination on screen is pixel coordinates.

Usually developers use two-dimensional textures for graphics, however using one dimensional or even 3 dimensional textures is not unheard of. 1D textures have a width. 2D textures have a width and a height (these are the images we are used to as png files). 3D textures have a width, a height and a depth. 3D textures are sometimes called _volume textures_. We generally only use 1D and 2D textures. 1D is used almost exclusivley for "toon shading"

When you map a texture to a polygon, it will deform with the polygon. In other words, if you rotate the earth, it's texture will rotate along with the sphere. This applies to all deformations and transformations. 