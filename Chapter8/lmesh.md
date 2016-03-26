# Implementation

It may not be immediateley obvious, but because an OBJ file has 3 indices per vertex attribute (Look at the f paramater in the last section) it does not suit its-self well for indexed rendering. This is because a vertex position and normal might not appear at the same index.