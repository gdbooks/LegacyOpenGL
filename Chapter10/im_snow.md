# Snow particle example

Lets start the class out with some constants. There are two parts to each constant. The default value that the constant will have, along with some random range that affects that value.
```cs
using OpenTK.Graphics.OpenGL;
using Math_Implementation;
using System.Drawing;
using System.Drawing.Imaging;

namespace GameApplication {
    class SnowstormParticleSystem : ParticleSystem {
        public static Vector3 SNOWFLAKE_VELOCITY = new Vector3(0f, -2f, 0f);
        public static Vector3 VELOCITY_VARIATION = new Vector3(0.2f, 0.5f, 0.2f);
        public static float SNOWFLAKE_SIZE = 0.25f;
        public static float SIZE_DELTA = 0.25f;
        public static float SNOWFLAKE_PER_SEC = 500;
```

Next, let's define some member values. The snow storm is a volumetric effect, so we need to define a cube for it. We're going to leverage the fact that the ```ParticleSystem``` base class already has an origin variable. We will define a cube by augmenting origin with width, height and depth.
```cs
        protected float height;
        protected float width;
        protected float depth;
        protected int texture;
```

The constructor takes 1 more variable than the base constructor, which is the size of the snow volume. Becuase of this, the default constructor is called using the : convention with the first two arguments only.

After we store the dimensions of the snow volume, we go ahead and load a snow texture. You can download the texture from [here]

```cs
        public SnowstormParticleSystem(int maxParticles, Vector3 origin, Vector3 size)
            : base(maxParticles, origin) {
            height = size.Y;
            width = size.X;
            depth = size.Z;
            

            texture = GL.GenTexture();
            GL.BindTexture(TextureTarget.Texture2D, texture);
            GL.TexParameter(TextureTarget.Texture2D, TextureParameterName.TextureMinFilter, (int)TextureMagFilter.Linear);
            GL.TexParameter(TextureTarget.Texture2D, TextureParameterName.TextureMagFilter, (int)TextureMagFilter.Linear);
            Bitmap bmp = new Bitmap("Assets/snow.png");
            BitmapData bmp_data = bmp.LockBits(new Rectangle(0, 0, bmp.Width, bmp.Height), ImageLockMode.ReadOnly, System.Drawing.Imaging.PixelFormat.Format32bppArgb);
            GL.TexImage2D(TextureTarget.Texture2D, 0, PixelInternalFormat.Rgba, bmp_data.Width, bmp_data.Height, 0, OpenTK.Graphics.OpenGL.PixelFormat.Bgra, PixelType.UnsignedByte, bmp_data.Scan0);
            bmp.UnlockBits(bmp_data);
            bmp.Dispose();
        }

        public override void Shutdown() {
            GL.DeleteTexture(texture);
        }

        public override void Update(float deltaTime) {
            for (int i = 0; i < numParticles;) {
                particleList[i].position = particleList[i].position + particleList[i].velocity * deltaTime;

                if (particleList[i].position.Y <= systemOrigin.Y) {
                    particleList[i] = particleList[--numParticles];
                    particleList[numParticles] = new Particle();
                }
                else {
                    i++;
                }
            }

            accumulatedTime += deltaTime;
            int newParticles = (int)(SNOWFLAKE_PER_SEC * accumulatedTime);
            accumulatedTime -= 1.0f / SNOWFLAKE_PER_SEC * newParticles;
            Emit(newParticles);
        }

        public override void Render() {
            GL.BindTexture(TextureTarget.Texture2D, texture);

            GL.Begin(PrimitiveType.Quads);
            for (int i = 0; i < numParticles; ++i) {
                Vector3 startPos = particleList[i].position;
                float size = particleList[i].size;

                GL.TexCoord2(0f, 1f);
                GL.Vertex3(startPos.X, startPos.Y, startPos.Z);

                GL.TexCoord2(1f, 1f);
                GL.Vertex3(startPos.X + size, startPos.Y, startPos.Z);

                GL.TexCoord2(1f, 0f);
                GL.Vertex3(startPos.X + size, startPos.Y - size, startPos.Z);

                GL.TexCoord2(0f, 0f);
                GL.Vertex3(startPos.X, startPos.Y - size, startPos.Z);
            }
            GL.End();
        }

        public override void InitParticle(int index) {
            particleList[index].position.X = systemOrigin.X + (float)random.NextDouble() * width;
            particleList[index].position.Y = height;
            particleList[index].position.Z = systemOrigin.Z + (float)random.NextDouble() * depth;

            particleList[index].size = SNOWFLAKE_SIZE + (float)random.NextDouble() * SIZE_DELTA;

            particleList[index].velocity.X = SNOWFLAKE_VELOCITY.X + (float)random.NextDouble() * VELOCITY_VARIATION.X;
            particleList[index].velocity.Y = SNOWFLAKE_VELOCITY.Y + (float)random.NextDouble() * VELOCITY_VARIATION.Y;
            particleList[index].velocity.Z = SNOWFLAKE_VELOCITY.Z + (float)random.NextDouble() * VELOCITY_VARIATION.Z;
        }
    }
}
```