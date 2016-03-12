# Particle Class

Let's start by making a particle class. We're going to include a small number of attributes in this class. We're also going to include two unused attributes. The idea behind the unused atributes is to demonstrate that not all systems use the same attributes.

For example, if we have a snow particle system and a fire particle system they will both utilize the same particle class. The fire system might use attributes that the snow system doesn't and visa versa. If there are extra, unused variables in this class, it's not a problem.

The other thing to note about this class is that it has no methods. It's a class strictly to hold data. I've decided that my particle systems are going to know how to render each particle, so i gave the responsibility to render and update the particles to the system. This makes the partcile class super simple.

```cs
using Math_Implementation;

namespace GameApplication {
    class Particle {
        // These are used in our demo
        public Vector3 m_pos = new Vector3();
        public Vector3 m_velocity = new Vector3();
        public float m_size = 0f;

        // Unused in our demo, might be nice for other systems
        public float[] m_color = new float[4];
    }
}

```