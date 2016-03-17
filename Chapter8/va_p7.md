```
using OpenTK.Graphics.OpenGL;
using Math_Implementation;
using System.Drawing;
using System.Drawing.Imaging;

namespace GameApplication {
    class DrawElementsExample : Game {
        Grid grid = null;
        float[] cubeVertices = null;
        float[] cubeColors = null;
        uint[] cubeIndices = null;

        public override void Resize(int width, int height) {
            GL.Viewport(0, 0, width, height);
            GL.MatrixMode(MatrixMode.Projection);
            float aspect = (float)width / (float)height;
            Matrix4 perspective = Matrix4.Perspective(60.0f, aspect, 0.01f, 1000.0f);
            GL.LoadMatrix(Matrix4.Transpose(perspective).Matrix);
            GL.MatrixMode(MatrixMode.Modelview);
        }

        public override void Initialize() {
            grid = new Grid(true);

            cubeVertices = new float[] {
                -1.0f, -1.0f,  1.0f, // Vertex 1
                 1.0f, -1.0f,  1.0f, // Vertex 2
                 1.0f,  1.0f,  1.0f, // Vertex 3
                -1.0f,  1.0f,  1.0f, // Vertex 4
                -1.0f, -1.0f, -1.0f, // Vertex 5
                 1.0f, -1.0f, -1.0f, // Vertex 6
                 1.0f,  1.0f, -1.0f, // Vertex 7
                -1.0f,  1.0f, -1.0f  // Vertex 8
            };

            cubeColors = new float[] {
                1.0f, 0.0f, 0.0f, // Vertex 1
                0.0f, 1.0f, 0.0f, // Vertex 2
                0.0f, 0.0f, 1.0f, // Vertex 3
                1.0f, 1.0f, 1.0f, // Vertex 4
                1.0f, 0.0f, 0.0f, // Vertex 5
                0.0f, 1.0f, 0.0f, // Vertex 6
                0.0f, 0.0f, 1.0f, // Vertex 7
                1.0f, 1.0f, 1.0f  // Vertex 8
            };

            cubeIndices = new uint[] {
                // Front
		        0, 1, 2, // Front triangle 1
                2, 3, 0, // Front triangle 2
		        // Top
		        1, 5, 6, // Top triangle 1
                6, 2, 1, // Top triangle 2
		        // Back
		        7, 6, 5, // Back traignle 1
                5, 4, 7, // Back triangle 2
		        // Bottom
		        4, 0, 3, // Bottom triangle 1
                3, 7, 4, // Bottom triangle 2
		        // Left
		        4, 5, 1, // Left triangle 1
                1, 0, 4, // Left triangle 2
		        // Right
		        3, 2, 6, // Right triangle 1
                6, 7, 3  // Right triangle 2
            };
        }

        public override void Render() {
            Matrix4 lookAt = Matrix4.LookAt(new Vector3(-7.0f, 5.0f, -7.0f), new Vector3(0.0f, 0.0f, 0.0f), new Vector3(0.0f, 1.0f, 0.0f));
            GL.LoadMatrix(Matrix4.Transpose(lookAt).Matrix);

            GL.Disable(EnableCap.DepthTest);
            grid.Render();
            GL.Enable(EnableCap.DepthTest);

            GL.EnableClientState(ArrayCap.VertexArray);
            GL.EnableClientState(ArrayCap.ColorArray);

            GL.VertexPointer(3, VertexPointerType.Float, 0, cubeVertices);
            GL.ColorPointer(3, ColorPointerType.Float, 0, cubeColors);
            GL.DrawElements(PrimitiveType.Triangles, cubeIndices.Length, DrawElementsType.UnsignedInt, cubeIndices);

            GL.DisableClientState(ArrayCap.VertexArray);
            GL.DisableClientState(ArrayCap.ColorArray);
        }
    }
}
```