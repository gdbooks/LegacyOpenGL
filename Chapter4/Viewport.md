#Viewport

Some projection functions we just discussed are closely related to the viewport size. Take Perspective for example, we had to divide the viewport width and height. We know that the viewport transformation happens after the projection transformation, so now is as good a time as any to discuss it.

In essence the viewport specifies the dimensions and orientation of the 2D window on which you will be rendering. You can set it with:

```
void GL.Viewport(int x, int y, int width, int height);
```

x and y specify the coordinates of the __lower left__ corner of the viewprot. Width and height specify window size in pixels. 

The reson our windows didn't look the same up until this point is because we have had different default viewport values set. Once you are setting your own values for this our windows will look the same.

Let's take the scene we rendered in the Projections section, and manually specify a viewport for it

```
```