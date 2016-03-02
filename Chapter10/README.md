# Particles

Everything is better with particles! Case and point, watch [this video](https://www.youtube.com/watch?v=VfDyFucRbw8). 

Particles in effect are a number of entities that behave according to some pre-defined rules. Take rain for example. Each rain drop is a particle, all drops behave according to gravity. That is, a cloud spawns rain particles, they fall down with gravity. When a rain drop entity hits the ground it dies (is recycled as a different drop).

A more formal definition: _A particle system is a collection of any number of entities, either related or unrelated, that behave according to a set of logical rules._

See, the concept of a particle system is rather abstract. Any number of things will fit into that description, the roads of GTA, the speckles of a screensaver, rain... This is what makes particles so hard for most programmers. Two games can both have particle systems, and the implementation can be compelatly different. 

When it comes to particles all we care about is what the final image looks like. Do these entitites acting under some system look the way we expect / want? If so, the particle system works. If not, it just needs some additional work.

_"We don't try to model the outcome of the system, we try to model the system and see its outcome"_ - Andre LaMothe

Explosions, sparks, underwater bubbles, other special fx, particle systems in modern games are used to add eye-candy to a game. Usually indevidual particles have no collision reaction, there are just too many of them. But some sort of generic sphere collider could encapsulate the entire particle system.