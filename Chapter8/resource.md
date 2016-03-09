# Resource Management

Loading meshes and textures one at a time isn't the worst thing you can do, but it does make managing them extremly difficult. 

Let's say you have a scene with 1 texture used 20 times. You obviously don't want to load the same texture 20 times. So do you keep a counter around? How do you manage the lifecycle of those textures? This is where resource management comes in handy.