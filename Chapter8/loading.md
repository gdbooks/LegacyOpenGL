# Meshes

Up to this point we've used a lot of placeholder geometry for spheres, cubes and planes. None of it was very impressive, and most of them render in a very sub-optimal kind of way. Let's go ahead and fix that.

In this section i'm going to show you how to load meshes from possibly the most common format out there, OBJ files. We're going to build a mesh class that can load these OBJ models and render them using vertex buffer objects.

## The Format

An OBJ file is just a text file. It's a standard format that supports a LARGE standard with many features. We're only going to implement loading a small specific subset of obj files.

Every OBJ we load we're going to assume has a set vertex format containing a position, a texture coordinate and a normal. Not all OBJ files have all of this information, but the subset we load we assume will.

Let's take a look at what the inside of an OBJ file looks like:

```
# Blender3D v249 OBJ File: untitled.blend
# www.blender3d.org
mtllib cube.mtl
v 1.000000 -1.000000 -1.000000
v 1.000000 -1.000000 1.000000
v -1.000000 -1.000000 1.000000
v -1.000000 -1.000000 -1.000000
v 1.000000 1.000000 -1.000000
v 0.999999 1.000000 1.000001
v -1.000000 1.000000 1.000000
v -1.000000 1.000000 -1.000000
vt 0.748573 0.750412
vt 0.749279 0.501284
vt 0.999110 0.501077
vt 0.999455 0.750380
vt 0.250471 0.500702
vt 0.249682 0.749677
vt 0.001085 0.750380
vt 0.001517 0.499994
vt 0.499422 0.500239
vt 0.500149 0.750166
vt 0.748355 0.998230
vt 0.500193 0.998728
vt 0.498993 0.250415
vt 0.748953 0.250920
vn 0.000000 0.000000 -1.000000
vn -1.000000 -0.000000 -0.000000
vn -0.000000 -0.000000 1.000000
vn -0.000001 0.000000 1.000000
vn 1.000000 -0.000000 0.000000
vn 1.000000 0.000000 0.000001
vn 0.000000 1.000000 -0.000000
vn -0.000000 -1.000000 0.000000
usemtl Material_ray.png
s off
f 5/1/1 1/2/1 4/3/1
f 5/1/1 4/3/1 8/4/1
f 3/5/2 7/6/2 8/7/2
f 3/5/2 8/7/2 4/8/2
f 2/9/3 6/10/3 3/5/3
f 6/10/4 7/6/4 3/5/4
f 1/2/5 5/1/5 2/9/5
f 5/1/6 6/10/6 2/9/6
f 5/1/7 8/11/7 6/10/7
f 8/11/7 7/12/7 6/10/7
f 1/2/8 2/9/8 3/13/8
f 1/2/8 3/13/8 4/14/8
```

Every line begins with some kind of a letter or phrase to describe the information the line will hold. Then, the information appears in a specific format. There are more line markers than what is listed above / what we will be using. This means its important for your parser to ignore unknown line starts.

Here are the lines we will care about:

* v is a vertex
* vt is the texture coordinate of one vertex
* vn is the normal of one vertex
* f is a face

And these are the things we don't care about:

* Any line that begins with # is a comment, just like // in C#
* usemtl and mtllib describe the look of the model. We won’t use this in this tutorial.
* s stands for specular, it can be off or it can be some number

A material describes how to shade a model. OBJ files often come with .mtl files which then link to textures. We're not going to care about mtl files. We just want to get vertex data into our game!

