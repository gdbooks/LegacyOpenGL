# Particle System

The ```ParticleSystem``` class is a base class. It will need to be subclassed for each type of particle effect that your game might need. I included a minimum number of common attributes that you might need, more might be added as necesary.


We start off by declaring some member variables. We're going to need a list to store all the particles in the system. We're going to need to know the maximum number of particles the system will be able to hold as well as how many are currently alive. It also helps to know where in 3D space the system is. Becuse of how the particles are going to be spawned, let's keep track of how long the system has been alive. And finaly, i'll include a conveniance variable for random numbers.
```cs
using System;
using System.Collections.Generic;
using Math_Implementation;

namespace GameApplication {
    class ParticleSystem {
        protected List<Particle> particleList;
        protected int maxParticles;
        protected  int numParticles;
        protected  Vector3 systemOrigin;
        protected float accumulatedTime;
        protected Random random;
```

Next, let's make the constructor. It takes two arguments and sets all the class member variables accordingly. This constructor will also allocate all the particles we need. 

```cs
        public ParticleSystem(int maxParticles, Vector3 origin) {
            particleList = new List<Particle>();
            this.maxParticles = maxParticles;
            systemOrigin = new Vector3(origin.X, origin.Y, origin.Z);
            numParticles = 0;
            accumulatedTime = 0f;
            random = new Random();

            for (int i = 0; i < maxParticles; ++i) {
                particleList.Add(new Particle());
            }
        }
```

Next, let's define some strictly virtual functions. Because each particle effect might behave differently, they will all need to override these functions. There is the standard update and render functions. The Shutdown function should be used in case the constructor loads a texture or something.

The interesting function here is ```InitParticle```. This function is responsible for taking an unused particle and initalizing it with some semi random values. This will of course need to be overridden.

```cs
        public virtual void Update(float deltaTime) { }
        public virtual void Render() { }
        public virtual void Shutdown() { }

        public virtual void InitParticle(int index) {
            Console.WriteLine("ParticleSystem.InitParticle should never be called directly");
            Console.WriteLine("Only subclasses should call this function!");
        }
```

We're going to use a function called Emit to actually spawn particles. When you call emit you pass in a number, the number of particles that you would like spawned. Emit will return an integer, representing how many particles it failed to spawn. With any luck the return number should be 0.

Particles might or might not start out with some randomized values. For this reason when a particle is created we call the ```InitParticle``` function on it. That function will ensure that the particle starts out right.
```cs
        public int Emit(int request) {
            // If we can make particles, make them!
            while (resuest > 0 && (numParticles < maxParticles)) {
                // Initialize a particle, and increase the number of particles 
                InitParticle(numParticles++);
                // Decrease particles left to spawn
                --request;
            }

            // Return how many particles we could not create. Ideally, should be 0
            return request;
        }
    }
}
```

That's all there is to the particle system. It's a base class, so a lot of the effect related heavy lifting is going to be done by child classes. 