# Custom Matrices
Using OpenGL matrix functions is pretty easy. And the stack makes working with matrices even easyer. But you already have a Matrix class built. Which already offers everything OpenGL matrices offer, and more! 

Tour matrices are also a bit easyer to work with than OpenGL's because you have full control and know they are mathematically accurate. Wouldn't it be great if you could use your own matrices to work with OpenGL?

Good news, you can!

## OpenGL Matrix
The OpenGL matrix has been confusing programmers since 1992. This next bit might get a bit confusing. So, bear with me; re-read the section if you need to and call me on Skype if you have questions.

__Mathematically__ OpenGL uses __column major__ matrices. Because the matrices are _column major_, OpenGL uses __column vectors__. This means OpenGL works __post multiplication__ because a vector is a 4x1 matrix, so in order for inner dimensions to match it has to go on the right hand side of a matrix. __matrix * vector__. The matrices are __right handed__, that is __-Z is forward__, it goes _into_ the monitor.

Everything bolded in the above paragraph is the theoretical law of the land in OpenGL world. Write them on a notecard, memorize them! That is how the OpenGL specification says OpenGL matrices will work.

A column major matrix has it's basis vectors in the first 3 columns and it's translation vector in the 4th column, like so:

```
Xx  Yx  Zx  Tx
Xy  Yy  Zy  Ty
Xz  Yz  Zz  Tz
0   0   0   1
```

## Topology
Topology refers to how the matrix is stored in memory, THIS is where OpenGL gets confusing. Consider the following column major matrix:

```
Xx  Yx  Zx  Tx
Xy  Yy  Zy  Ty
Xz  Yz  Zz  Tz
0   0   0   1
```

Given that a matrix is stored as an array of floats (size 16) you would expect it to be laid out in memory like so:

```
[0] [1] [2] [3] [4] [5] [6] [7] [8] [9] [10] [11] [12] [13] [14] [15]
 Xx  Yx  Zx  Tx  Xy  Yy  Zy  Ty  Xz  Yz  Zz   Tz   0    0    0    1
```

Which makes sense, the matrix is stored 1 row at a time in a linear array.