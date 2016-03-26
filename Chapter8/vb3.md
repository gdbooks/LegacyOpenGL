# Rendering

This is where things get interesting! OpenGL tries to re-use the same functons it did for vertex array rendering, but with the arguments interpreted differently. This in my opinion is a VERY bad idea, but it's how they decided to do it. 

So if the functions are the same then what tells OpenGL that we want to use vertex buffers instead of arrays? The bound buffer. If a buffer is bound, OpenGL assumes you are rendering VBO's, if not it assumes you are rendering vertex arrays. 

Let's take a look at some code that we used to render vertex arrays: