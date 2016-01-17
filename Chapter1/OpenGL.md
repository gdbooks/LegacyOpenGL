#OpenGL History

The current OpenGL version is 4.5. OpenGL 4.5 will continue to be developed but the API is branching off into a more modern API called Vulkan. The two will both be maintained for some time, until the old OpenGL api is phased out.

We are going to cover OpenGL 1.5. It is an older version of the API, but it's the perfect introduction into 3D graphics. It's easy to use and understand. We will delve into more modern versions of the API later.

The designers of OpenGL knew that hardware vendors would want to add features that may not be exposed by default to the OpenGL interface. To address this they included a method for extending OpenGL. Hardware vendors can create OpenGL extensions. When other vendors start implementing the functionality as well and the extension gains enough support it is submitted for review to the ARB. It is up to the ARB to promote the extension into the core specification of OpenGL.

By default the framework we will use to access OpenGL, OpenTK exposes extensions to us. We do not need to worry about having to manualy load them.

#Software V Hardware
Initially OpenGL 1.0 was a software renderer. It would crunch numbers, do math and "render" a 3D scene all on the CPU. Right around 1998 graphics cards started getting more and more popular. Graphics cards have specialized chips on them that can perform simple matrix math MUCH faster than the CPU can.

OpenGL 1.0 was able to use the GPU to render using Extensions. Hardware transform & lighting was promoted to core in OpenGL 1.1. By OpenGL 1.5 all graphics calulations happen on the GPU

#OpenGL Architecture
OpenGL is a collection of several hundred functions providing access to all the features offered by your graphics card. Internally OpenGL functions as a state machine, a collection of states that tell the OpenGL what to do and are changed in a very well-defined manner. Using the OpenGL API you can set various aspects of the state machine, including things such as the current color, lighting and blending.

When rendering, everything drawn is drawn according to the rules set inside the OpenGL statemachine. It's important to be aware of what the various states are and how they effect what gets rendered. It is not uncommon at all to get a black screen with nothing being rendered simply because you forgot to set a single state. Graphics Programming is pretty finicky, and you kind of need to know what you are doing.