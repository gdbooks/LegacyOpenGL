# Matrix relationships

What happened with the legs and feet of Mr.Roboto is super important. We must understand that each matrix manupulation we do, builds on top of previous matrix manipulations. Take the following code

```
GL.Translate(-2, -1, 7);
// Everything below this will be translated
```

So, when we wanted to foot of Mr.Roboto to move with the leg we had to multiply it while the model matrix of the leg was still on the stack.

This made the model matrix of the foot __relative__ to the model matrix of the leg. 

In OpenGL you can use indentation along with Push and Pop matrix to make these relationships very clear to understand. For example

```
void Draw() {
    GL.PushMatrix();
        
        // Transform to where the sun is
        Translate(...)
        Rotate(...)
        Scale(...)
        DrawSphere(); // < draws the sun
        
        // Mars ciecles the sun, so it's model matrix has to be relative to the suns!
        GL.PushMatrix();
            Translate(...)
            Rotate(...)
            Scale(...)
            DrawSphere(); // < draws mars
            
            // Mars has two moons, both circle mars, so their model matrices has to be relative to mars-es
            
            // However the moons are NOT relative to each other, so we have to save and restore matrices between rendering them
            GL.PushMatrix()
                Translate(...)
                Rotate(...)
                Scale(...)
                DrawSphere(); // < draws marses "phobos" moon
            GL.PopMatrix();
            GL.PushMatrix()
                Translate(...)
                Rotate(...)
                Scale(...)
                DrawSphere(); // < draws marses "demios" moon
            GL.PushMatrix();
            
            GL.PopMatrix();
        GL.PopMatrix();
    
    GL.PopMatrix();
}
```

## On your own
Make a new github gist. Copy / paste the above code into it. Add the pseudo code to render earth and it's moon. Send me a skype link when it's finished.