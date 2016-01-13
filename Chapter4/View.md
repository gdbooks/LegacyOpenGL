## The view matrix 
Combining translation, rotation and scale creates a transformation matrix. Sometimes also called the model matrix, or model to world matrix. But we're editing the mode-view matrix! So what is the view matrix?

As discussed earlyer, the view matrix is the inverse of the cameras world position! There are two things you need to know about your camera in world space, where it is (it's position) and which what it's looking (it's orientation).