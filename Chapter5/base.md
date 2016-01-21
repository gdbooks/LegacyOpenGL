# Exercises

The last section covered a LOT of ground about lighting, what it is and how it works. But i don't feel like it demonstrated clearly how to actually use lights. That's what this chapter is all about. In this chapter we're going to create several demo scenes, some together, some on your own to create some fully lit scenes

## Project setup
Before we get started, there are a few things we need to change within our code to get well lit objects. First, in order to actually view lighting, it helps to have a solid ground plane. Let's modify the grid class to optionally draw a solid plane.

First, let's add a public boolean that is going to control the render mode. By default drawing a filled in grid is going to be off (So that we don't break any of the example scenes we've already written)

```
namespace GameApplication {
    class Grid {
        // Control if the grid is rendered solid or not
        public bool RenderSolid = false;

        public Grid() {
            RenderSolid = false;
        }

        public Grid(bool solid) {
            RenderSolid = solid;
        }
```