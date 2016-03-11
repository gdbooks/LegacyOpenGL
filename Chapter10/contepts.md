# Design
Let's design a particle system. In general there are two ways to go about this. You can either design a base "ParticleSystem" class and then extend it into specific sub classes such as "FireParticles" or "SnowParticles" OR you can make a single mega particle system that exposes all sorts of properties such as absolute and relative velocity, then tweek these until your system is the way you want it.

As always, there is no right or wrong answer, what you use depends on what your needs are. Unity for example goes with the second route, the mega particle system. This is because they want the particles to be editable easily from the editor without code.

We on the other hand are not going to be shy about getting our hands dirty. We are going to design a particle system using the first approach. Where each new effect will take a little bit of code to create.

Lets take a bit of time and talk about things a particle system might need. Not everything we talk about on this page will make it into our implementation. Some of the stuff will be just for the purpose of discussion

## Particles

Lets start at the most basic element, the indevidual particle. To begin, you ned to decide what attributes a particle is going to need. Possible attributes might include:

* Position
* Velocity
* Life Span
* Size
* Weight
* Representation
* Color
* Owner

There are of course many other attributes you may want to try out, but these are the basics. When designing a particle system it's important to know to assign attributes that affect indevidual particles to the particle class, but attributes that effect all particles to the particle system class. Let's take a look at each of the above attributes in a little bit more detail. 

### Position

You need to know where the particle is in 3D space so that you can render it correctly. This is an attribute that will almost certainly belong to the particle, not the partcle system. You may also want to track the particles last position to achieve effects such as trails. Note that the particles position will be affected by velocity.