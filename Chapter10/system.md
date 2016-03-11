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