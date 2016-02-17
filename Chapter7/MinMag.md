# Min and Mag Filters
We're almost done! We've managed to load some memory onto the GPU at this point, but if we tried to render that memory as a texutre, nothing would show up. This is because our texture loading code neglected to specify min and mag filters. There is a TODO comment section in the function.

## What is a Min filter

## What is a Mag filter

Mag filter is the magnification filter. It is applied when an image is zoomed in so close that one pixel (texel) on the source image takes up multiple pixels (fragments) on the display screen. There are two common settings for mag filtering:

* __Nearest__ 
  * Nearest neightbor filtering means no scale is applied to the texture
  * The resulting image will be sharp, but very pixelated
* __Bilinear__
  * Use bilinear filtering to resize (enlarge) the source image
  * The result will be blured, but the image will look smooth

![MAG](mag_filter.png)