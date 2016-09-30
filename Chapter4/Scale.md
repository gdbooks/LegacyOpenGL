## Scaling
Scaling in its most simple definition increases or decreases the size of an object or coordinate system. Scaling is pretty self explanatory, i'm not going to spend too much time on it.

The function looks like this

```
void GL.Scale(float x, float y, float z); 
```

Scaling an object large is just using numbers bigger than 1 into the function. To shrink an object, use numbers smaller than 1 but greater than 0 (decimals). Using negative numbers will flip the object on the specific axis.

Go ahead and make a sample like we did with Translation and rotation. Try scaling big (2.0, 2.0, 2.0), also try scaling small (0.25, 0.25, 0.25)