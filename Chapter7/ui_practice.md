# Implementing a simple UI

We're going to modify the demo scene we've been working with to have a simple UI. I suggest reading this whole page over at least one time before implementing any code, just so you have an idea of everything that will be happening.

By the time we're done, the demo scene will look like this:

# TODO

First, download the following texture into your assets directory:

![UI](ui_atlas.png)

The first thing you want to do is make a ```RenderUI``` function. You can leave it blank for now, just have it stubbed in. This is where to code to render your UI will go.

* Add a new integer variable to the class to hold the UI
* In initialize, load the UI texture
  * Remember to unload it in shutdown
* Refactor your render function into a ```RenderWorld``` function
  * That is, render should set the modelview matrix
  * Then call ```RenderWorld```
* After the world is rendered:
  * Set a pixel perfect Orthographic projection matrix
    * Remember, back up the old one
  * Load identity for the view matrix
    * Remember, back up the old one
  * Call your ```RenderUI``` function
  * Restore the projection and view matrices

Running your game at this point, everything should render as it did before. It's a really good place to check and make sure nothing is broken yet. After you've confirmed that nothing is broken, it's time to start working on the actual UI.

* First, clear your depth buffer!
* Render the health-bar on screen.
  * Screen coordinates:
    * X: 
    * Y: 
    * W: 
    * H: 
  * UV Coordinates:
    * X:
    * Y:
    * W:
    * H:
  * You can either figure out the normalized UV position by hand
  * Or normalize it in code, all you need is the texture width / height
* The 3 buttons need to be rendered independently,
  * Because they are in a different order on screen than in the atlas 
  * All 3 buttons should be **Relative** to the bottom right corner
    * When the window is resized, these stay the same distance from the corner 
* Render facebook button on screen
* Render help button on screen
* Render home button on screen

When resizing the screen, the health bar is relative to the top left. This is why you could just do screen coordinates. But when you render the buttons in the bottom right, they need to move with the screen size. So you need to adjust their X-Y coordinates accordingly.

For example:

```cs
int buttonSpace = 10;
int homeButtonX = screen.Width - buttonSpace - home.Width;
int helpButtonX = homeButtonX - buttonSpace - help.Width;
int fbButtonX = helpButtonX - buttonSpace - fb.Width;
```