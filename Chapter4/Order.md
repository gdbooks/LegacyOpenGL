## Function order
Boy, we covered a LOT in the last section! We saw how we could translate, rotate and scale an object to create the world matrix. We also saw how we can use lookAt to create a view matrix.

Now, i always say matrix multiplication is assisiative, but not cumulative. This means:

* A \* B \* C = A \* (B \* C)
* A \* B != B \* A

So, whats the right order to multiply matrices in? Well, with the __modelView__ matrix you would expect to multiply the model by the view matrix, but that is NOT the case. That's right, we're about to make matrices a LOT more confusing. But first, let's take a look at the full multiplication that's happening.

Remember, OpenGL is column major, so vectors are column matrices (1 x 4). Because matrix multiplication inner products must match, this means the vector goes on the right side. The entire transform pipeline looks like this:

```
In theory
translated vector = vector * model * view * projection
How OpenGL really does it
renderVector = vector * modelView * projection
```

We want to first transform into model space, then view space, then projection space. Column major matrices have a slight gotcha to them,