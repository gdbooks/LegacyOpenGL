# Your own stack
At this point in our look at the transformation libary, we are no longer dependant on the built in OpenGL matrices! Go us! But we're still using the OpenGL stack. Which as we know has a set size. And we don't really know what it's doing under the hood.

Let's remedy this by making our own matrix stack. I'm going to lay out a skeleton here, you fill in the functions

```
class MatrixStack {
    protected List<Matrix4> stack = null;
    
    public MatrixStack() {
        // TODO: make new stack list
        // TODO: Add identity onto the stack
    }
    
    public void Push() {
        // Take the top of the stack, make a copy of it
        // add the new copy onto the stack so it becomes
        // the new top
    }
    
    public void Pop() {
        // Remove the top of the stack
    }
    
    public void Load(Matrix mat) {
        // Replace the top of the stack with whatever was passed in
    }
    
    public void Mul(Matrix mat) {
        // Multiply the top of the stack with whatever was pased in
    }
    
    float[] OpenGL
```