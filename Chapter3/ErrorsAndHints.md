## Finding errors
When you make an error OpenGL will usually not throw an exception, instead nothing will render. Finding errors in OpenGL is a hard task.

Passing incorrect values to OpenGL functions causes an error flag to be set. When this happens the functions will return without actually doing anything. To track down the problem you have to query the state machine error flag, you can do so with this function:

```
ErrorCode GL.GetError();
```

The return values of the error are documented on OpenTK's doxygen page.

# Giving OpenGL a Hint
Some operations in OpenGL will vary slightly from implementation to implementation (or driver to driver). An attempt was made to allow developers some form of control over these variations.

Not all implementations follow the commend, the ```GL.Hint``` function allows you to specify you desired level of trade off between image quality and speed for several different behaviours.

```
void GL.Hint(HintTarget target, HintMode mode)
```

The target paramater specifies the bahaviour you want to contro. The hint paramater is one of three options

* ```HintMode.Fastest``` is used to hint that the fastest and most efficient implementation should be used, possibly sacrificing image quality.
* ```HintMode.Nicest``` is used to indicate that the highest quality implementation should be used, even at the cost of execution speed / performance.
* ```HintMode.DontCare``` indicates that you don't have a preference. Let your OpenGL implementation decide what to use.

Lets take drawing a line for example.If you are drawing a few lines and you want them to be as smooth as possible, not pixelated at all you could set the following hint:

```
GL.Hint(HintTarget.LineSmoothHint, HintMode.Nicest);
```

If you are rendering millions of lines for whatever reason, anti-aliasing them all will give you a huge drop in performance. You could tell OpenGL to not anti-alias them and just draw the lines jagged / pixelated with:

```
GL.Hint(HintTarget.LineSmoothHint, HintMode.Fastest);
```