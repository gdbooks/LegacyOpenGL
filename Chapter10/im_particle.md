# Particle Class

Let's start by making a particle class. We're going to include a small number of attributes in this class. We're also going to include two unused attributes. The idea behind the unused atributes is to demonstrate that not all systems use the same attributes.

For example, if we have a snow particle system and a fire particle system they will both utilize the same particle class. The fire system might use attributes that the snow system doesn't and visa versa. If there are extra, unused variables in this class, it's not a problem.

The other thing to note about this class is that it has no methods. It's a class strictly to hold data. I've decided that my particle systems are going to know how to render each particle, so i gave the responsibility to render and update the particles to the system. This makes the partcile class super simple.

```cs
using Math_Implementation;

namespace GameApplication {
    class Particle {
        // These are used in our demo
        public Vector3 position = new Vector3();
        public Vector3 velocity = new Vector3();
        public float size = 0f;

        // Unused in our demo, might be nice for other systems
        public float[] color = new float[4];
    }
}
```

That's all there is to it. The __position__ variable is used to determine where to render the particle. The __velocity__ variable determines how the particle will travel. The __size__ variable sets the size of the particle. 

The __color__ variable is not going to be used in our demo, but other systems might need it. You should add attributes to the particle class every time a specific system needs them!