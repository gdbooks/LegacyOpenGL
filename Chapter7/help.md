# Loading an OpenGL image

Loading an OpenGL image can be done in 5 lines of code. 

```cs
Bitmap bmp = new Bitmap("SomeImageFile.png");
BitmapData bmp_data = bmp.LockBits(new Rectangle(0, 0, bmp.Width, bmp.Height), ImageLockMode.ReadOnly, System.Drawing.Imaging.PixelFormat.Format32bppArgb);
GL.TexImage2D(TextureTarget.Texture2D, 0, PixelInternalFormat.Rgba, bmp_data.Width, bmp_data.Height, 0, OpenTK.Graphics.OpenGL.PixelFormat.Bgra, PixelType.UnsignedByte, bmp_data.Scan0);
bmp.UnlockBits(bmp_data);
bmp.Dispose();
```

Three of these 5 lines are just fillers. This is the only line that matters:

```cs
GL.TexImage2D(TextureTarget.Texture2D, 0, PixelInternalFormat.Rgba, bmp_data.Width, bmp_data.Height, 0, OpenTK.Graphics.OpenGL.PixelFormat.Bgra, PixelType.Short, bmp_data.Scan0);
```

Make sure you understand the paramaters of the ```GL.TexImage2D``` function, as it is what uploads the large array of pixel data to the GPU