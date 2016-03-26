# Meshes

Up to this point we've used a lot of placeholder geometry for spheres, cubes and planes. None of it was very impressive, and most of them render in a very sub-optimal kind of way. Let's go ahead and fix that.

In this section i'm going to show you how to load meshes from possibly the most common format out there, OBJ files. We're going to build a mesh class that can load these OBJ models and render them using vertex buffer objects.