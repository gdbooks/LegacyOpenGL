#OpenTK
What is OpenTK? The Open Toolkit is an advanced, low-level C# library that wraps OpenGL, OpenCL and OpenAL. It is suitable for games, scientific applications and any other project that requires 3d graphics, audio or compute functionality.

What this means is OpenTK takes several low level C libraries (OpenGL, OpenCL and OpenAL) and exposes them to as a higher level C# Interface.

#Other options
OpenTK is not the only C# binding of OpenGL. There are a lot of others to choose from. The most notable one is the TAO framework.

With other options, why use OpenTK? First, it's the most activeley supported binding. More importantly, OpenTK attempts to leverage the language design of C# to solve issues that OpenGL has inherited from C. This is something no other binding is doing.

# Problem with C
OpenGL is nativly implemented as a C-API. C does not have Objects, only functions and memory. As such the API can be hard to navidate. For example, every state switch is one big enumeration. This means the following states (GL\_TRIANGLE, GL\_QUAD, GL\_COLOR\_BIT) are all really integers. So given the following function:

```
void glBegin(int primitiveState);
```

Which of the above states is valid here? GL\_TRIANGLE and GL\_QUAD are. The only way to know that is to go trough the docs. What happens if you pass GL\_COLOR\_BIT to the function? Nothing renders!

As you can imagine intellisense is of no help either. The second you type GL\_ about 500 options pop up. There is no indication what is appropriate and what is not.

#Solution with C-Sharp
To solve this problem OpenTK defines it's own enumerations. Many of them, that map to the huge list of C enumerations. For Instance, in OpenTK havs the following enum:

```
enum RenderPrimitives { GL_TRIANGLE, GL_QUAD, GL_FAN };
```

Now when you look at this function

```
void glBegin(RenderPrimitives primitiveState);
```

Visual studios intellisense will tell you that the argument is of type ```RenderPrimitives``` and when you type in ```RenderPrimitives.``` intellisense will pop up only the relevant items in the correct enumeration

# Other improvements
OpenTK is filled with other similar improvements that make writing OpenGL programs in C# much faster and less error prone than writing them in C. The OpenTK framework is available in Visual Studio's package manager NuGet.

Take some time and check out the [official website](http://www.opentk.com/) which has some cool guides and nifty code samples.