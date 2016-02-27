#Mip Mapping

Mip mapping is a technique used to prevent texel swimming. Before we can talk about how mip mapping works, we need to discuss what texel swimming is, and why it's a problem. consider the following image:

![MIP1](mip1.png)

The left side shows a scene without mip mapping, the right side shows the same scene with linear mip mapping. Notice how even tough the image is a straight grid, off in the distance a wavy distortion startes to happen. This phenomenon is called texel swimming. 

Texel happens because the further away a textured pixel is, the more super sampled it becomes. That is, the span of 3 pixels on the source image are compressed into perhaps a single pixel on screen. When applying a perspective distortion to an image, the swimming artifacts appear.

How does mip mapping solve this? By using several resolutions of an image in memory, and sampling the correct one depending on how far away screen pixel is. This way you are more likeley to get a 1 to 1 texture mapping from source to screen and less likeley to see texel swimming. Consider the following image:

![MIP2](mip2.jpg)

The further away you view the wall segment, the smaller the version of the image you need to sample. That is, the closer a wall the more detail it needs (sample a 256x256 version of the wall texture) but the further away the wall is, the less textures it is on screen. That means when the wall is far away a 32 x 32 version of the wall texture has enough detail.

Of course because you are sampling an image at different resolutions the resulting image will look blurred based on what resolution the specific image is sampled at. Take a look:

![MIP3](mip3.png)

This may seem unreasonable, i mean doesn't blurring the image make the world look worse? In simple cases yes. But look at this example scene, it's not a complex scene, but adding mip-mapping makes a LOT of difference in the overall quality:

![MIP4](mip4.png)

## Texture piramid

Like i said mip mapping is based on the pixel being rendered's distance to the camera. This image demonstrated the different mip levels being used

![MIP5](mip5.png)

The way to visualize what pixel uses what mip is by thinking of the pixel as a texture pyramid:

![MIP6](mip6.png)

The closer a pixel is to the camera, the closer it is the mip level 0. The further it is from the camera, the closer it is to a deeper mip level (like 2). So what does a mip mapped image look like in memory? Like this:

![MIP7](mip7.gif)

It's still a single texture, but that single texture is now larger, and holds multiple copies of the image at different scales. For example, if a base texture is 1024x1024, the mipmapped version of that texture will have a resolution of 1536x1024 and will contain resized versions of the image. (1536 = 1024 + 512)

## Where do mip maps come from?