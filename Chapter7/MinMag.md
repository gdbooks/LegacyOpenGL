# Min and Mag Filters
We're almost done! We've managed to load some memory onto the GPU at this point, but if we tried to render that memory as a texutre, nothing would show up. This is because our texture loading code neglected to specify min and mag filters. There is a TODO comment section in the function.

Min and mag filters describe to OpenGL what to do when we are trying to render a 256x256 texture on a 512x512 surface, or even a 128x128 surface! There is no clear way to map the pixels of a smaller image to a larger surface or a larger image to a smaller surface. Each approach in handling the problem has it's own ups and downs, so OpenGL lets you pick what to do.

## What is a Min filter

Min filter is the minification filter. It is applied when an image is zoomed out so far that multiple pixels (texels) on the source image make up a single pixel (fragment) on the display screen. There are two common settings

* __Nearest__
  * Nearest neigbor filtering, does not attempt to scale the image
  * Result is sharp / pixelated
* __Bilinear__
  * Bilinear filtering, attempts to resize the image
  * Result is soft / blurred

Either way you set the min filter, the resulting image will be less than ideal because certain pixels have to be skipped. Usually, min filtering only needs to effect far away objects, so this isn't too big of an issue.

![MIN](min_filter.png)

## What is a Mag filter

Mag filter is the magnification filter. It is applied when an image is zoomed in so close that one pixel (texel) on the source image takes up multiple pixels (fragments) on the display screen. There are two common settings for mag filtering:

* __Nearest__ 
  * Nearest neightbor filtering means no scale is applied to the texture
  * The resulting image will be sharp, but very pixelated
* __Bilinear__
  * Use bilinear filtering to resize (enlarge) the source image
  * The result will be blured, but the image will look smooth

![MAG](mag_filter.png)