# Putting it all together

Now we know everything we need to know to make a simple textured scene! Nothing left to do except actually make something textured! In this section i'm going to walk you trough setting up a simple textured scene, but the actual texturing work will be up to you.

## Putting together the test scene

## On your Own

## Adding some detail

## On Your Own

## Sorting

So far we've been writing all of this rendering code inline, that is whenever we needed to render anything, we just do! The only thing we really wrapped into an object has been the grid. Real games don't work like this. Real games are component based.

In a real game, you have a game object, the game object has a list of components. When the game object's render function is called, it just calls render on every component. This should sound familiar to you, it's the system you built for the dungeon game.

In the dungeon game objects rendered according to how deep they where in the scene graph. This works for a simple 2D game, but not for a complex 2D game. It would never work on a 3D game. It's pretty advanced, but there is a few [Handmade Hero](https://hero.handmadedev.org/videos/game-architecture/day229.html) episodes on the topic.

I'll walk you trough how it would normally work. This is not something you need to implement or anything, but it is something to be aware of, as you will need to do this at some point.