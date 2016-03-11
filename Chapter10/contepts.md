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

### Velocity

Your particles are probably going to be moving, so you need to store their velocity. It's most convenient to store this as a vector representing both speed and direction so you can use the vector to update the particles position.

Velocity will likely be affected by such factors as wind and gravity which we'll discuss later in this chapter, in the "Forces" subsection under "Particle Systems". If the particle is capable of accelerating itself, that could affect the velocity as well, and you'd want to create an additional vector to store the acceleration. More often tough, the factors that affect the velocity of a particle are external.

### Life Span

For most effects, particles are going to be emitted from their source, and after some period of time are going to disappear. For this reason, you need to either keep track of how long a particle has been alive, or how long it has left to live. The life span may affect other attributes as a particle might grow, shrink or fade over time.

### Size

Size in an attribute that may not need to be handled by indevidual particles. In fact, unless the size changes based on something, it's a useless attribute! However you might want to increase the size of particles, for example to represent a large fire, then decrease them as the fire dwindles. 

### Weight

Weight is a lot like size in terms of weather it's needed as an attribute or not. If a particle might accelerate differently based on weight, or if it is affected by gravity differently you should include weight. Otherwise it's useless.

### Representation

To have particles produce some kind of effect, you're going to have to see them. The real question is, how are you going to represent them on screen? There are three commonly used methods, tough you may find other ways to represent them depending on your needs. The most common representations are:

* __Points__ Points can be used for a number of effects, especially those that arn't viewed closely. Each particle might be a 3D point.
* __Lines__ These can be used to create a trailing effect, which is often useful. The line connects the particles current position and it's last one. Often used for rain
* __Textured Quads__ This offers the most flexibility, and is the most widley used method. The particles its self is a quad or a triangle, and has a (possibly alpha blended) texture on it. Fire, sparks, smoke all use this method.

Note, if you decide to use quads, it's a good idea to use billboarding to amke them always face the camera. Billboarding will be covered two chapters from now, under the "Advanced Topics" section.