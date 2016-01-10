# OpenGL States & Primitives
Now its time to finally get into the meat of OpenGL! To begin to unlock the power of OpenGL, you need to start with the basics, and that means understanding states and primitives. Before we start, we need to discuss something that is going to come up during the discussion of primitives and pretty much everything else from here on out. The OpenGL State Machine.

The OpenGL state machine consists of hundreds of settings that affect varous aspects of rendering. Because the state machine will play a role in everything you do, it's important to understand what the default settings are, how you can get information about the current settings and how to change those settings. Several generic functions are used to control the state machine, we will look at these first!

##State functions
OpenGL provides a number of multipurpose function that allow you to query the OpenGL state machine. 