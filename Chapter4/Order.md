## Function order
Boy, we covered a LOT in the last section! We saw how we could translate, rotate and scale an object to create the world matrix. We also saw how we can use lookAt to create a view matrix.

Now, i always say matrix multiplication is assisiative, but not cumulative. This means:

* A \* B \* C = A \* (B \* C)
* A \* B != B \* A

So, whats the right order to multiply matrices in? Well, with the __modelView__ matrix you would expect to multiply the model by the view matrix, but that is NOT the case. That's right, we're about to make matrices a LOT more confusing. But first, let's take a look at the full multiplication that's happening.

Remember, OpenGL is column major, so vectors are column matrices (1 x 4). Because matrix multiplication inner products must match, this means the vector goes on the right side. The entire transform pipeline looks like this:

```
In theory
translated vertex = vertex * model * view * projection
How OpenGL really does it
renderVertex = vertex * modelView * projection
```

We want to first transform into model space, then view space, then projection space. Column major matrices have a slight gotcha to them, the matrices take effect from left to right, not right to left. 

This means that the above code, would first move into projection space, then view space then model space. The exact Opposite of what we want!

This is confusing, but in order to do this:

```
translated vertex = vertex -> model -> view -> projection
```

You have to multiply in reverse order, like so:

```
translated vertex = projection * view * model * vertex
```

This is not the case for row major matrices! Only column major ones.

## Scaling, Translating and Rotating
The other order we need to worry about is scale-translate-rotate order. 

What order should you scale/translate/rotate in?

* Scale First
* Rotate second
* Translate last

So the transformation pipe looks like this:

```
transformed = vertex -> scale -> rotate -> translate
```

Remember, multiply right to leftThis means your multiplication order will look like this:

```
Result = Translate * Rotate * Scale * Vertex
```
Fun times.

## OpenGL functions
Hopefully that all made sense, so how does that apply to OpenGL functions? Well if you recall, openGL functions just multiply matrices, so we have to call them in the right order, like so

```cs
void Render() {
    // Pipeline: translated vertex = vertex -> model -> view -> projection
    // Model matrix: transformed = vertex -> scale -> rotate -> translate
    // Remember, read right to left when implementing multiplication!
    
    // TODO: Set up projection here
    // Multiply projection
    
    // Set the modelView matrix to identity
    GL.MatrixMode(MatrixMode.ModelView);
    GL.LoadIdentity();
    
    // Multiply view
    LookAt(0.5f, 0.5f, 0.5f, 0.0f, 0.0f, 0.0f, 0.0f, 1.0f, 0.0f);

    // Draw an object
    // Multiply model (scale, rotate, translate)
    // Multiply translate
    GL.Translate(-3.0f, 0.0f, 1.0f);
    // Multiply rotate
    GL.Rotate(90.0f, 1.0f, 0.0f, 0.0f);
    // Multiply scale
    GL.Scale(1.0f, 1.0f, 1.0f); // No scale
    
    // The actual model has the input vertices
    DrawCube();
}
```

If this doesn't make sense right now, go ahead and give me a call on skype. I know it's a bit of a confusing topic.

# Box Demo
Make a new demo scene and follow along

```
using System;
using OpenTK.Graphics.OpenGL;

namespace GameApplication {
    class BoxDemo : Game {
        Grid grid = null;

        // Add Look at function, it's in this gist
        // https://gist.github.com/gszauer/91038dbb010755d719de

        public override void Initialize() {
            grid = new Grid();
        }

        public override void Update(float dTime) {

        }
        public override void Render() {
            GL.MatrixMode(MatrixMode.Modelview);
            GL.LoadIdentity(); // Reset modelview matrix
            LookAt(0.5f, 0.5f, 0.5f, 0.0f, 0.0f, 0.0f, 0.0f, 1.0f, 0.0f);

            // Render grid at the origin of the world
            grid.Render();

            
        }
        
        public override void Shutdown() {

        }
    }
}
```