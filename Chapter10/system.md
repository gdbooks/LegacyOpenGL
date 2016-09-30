# Particle Systems

Particles are owned by a particle system. Each particle system will control a set of particles, each of which acts atonomously, but share some common attributes. It's the job of the particle system to assign these attributes in such a way that, collectivley, the particles create a desired effect. Some of the things a particle system might handle include:

* Partcle list
* Position
* Emission Rate
* Forces
* Default particle attributes and ranges
* Current state
* Blending
* Representation

Just like with the discussion of particles, not everything we discuss about particle systems here will be implemented in our demo. And when you design your own particle systems, you might opt to include more attributes than the ones listed here. With that being said, let's go trough each attribute:

### Particle List

First and foremost, the system needs to be able to access the particles it is managing. So it needs a list of them, often stored as an array. The particle system should also have some maximum number of particles that it is allowed to create, effectivley defining the size of the array.

### Position

The particle system must be located somewhere to determine where particles start. Altough this is usually modelled as a single point in space (For example, a campfire), but they don't have to be. You could represent the position of a system as a rectangle, so particles can be emitted randomly within that space (For example, rain), which would make the rectangle the part of the sky that is raining.

### Emission Rate

The emission rate determines how often a new particle is created. To maintain a regular emission rate, the system will also need to keep track of how long it's been since the last particle was emitted.

### Forces

Adding one or more forces to a particle system can create a greater degree of realism, or lack thereof. Forces can range anywhere from graivty, gravity wells to exploding forces and torque.

### Default particle attributes and ranges

When a system creates a new particle, it should do so with some default attribtues. For certain systems, having each particle spawn the same size will work perfectly. Other systems might need a default range. That is any particle might spawn between a range of 2 and 4 in terms of size. Wether your system uses default attribtues or ranges depends on the effect you want to create.

### Current State

You might want your particle system to change its behaviour over time; this may simply involve turning a system on or off. For example, a snow particle system should be off during summer.This is essentially enabling / disabling the system.

Sometimes you want to manually turn the system on or off, but other times you might want to have the system turn off on it's own after some time. An explosion would be a great example of this.

### Blending

Most particle systems use some sort of blending. For this reason you might want to include source and destination blend paramaters as attirbutes of the particle system. Then again, most systems will make due with the default alpha blending, in which case this attribute is useless.

### Representation

In most cases all particles within a system are going to be represented the same way. Because of this you might want to store a particles representation as a system attribute, instead of as an indevidual particle attribtue.