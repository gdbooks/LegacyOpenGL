#Input
For the sake of completeness i'm going to show you how to read raw input here. While this is usefull, the ```InputManager``` from the ```GameFramework``` still works just fine (Assuming you set it up correctly). I highly suggest using the ```InputManager``` or rolling your own version of it.

After reading this, go ahead and browse trough the ```InputManager``` class to get a better understanding of what it's doing and how it's doing it. The ```InputManager``` is just a convenient abstraction that provides buffered input instead of raw input.

## OpenTK.Input
All input related function reside within the ```OpenTK.Input``` namespace. This namespace has several enumerations and classes we care about:

* OpenTK.Input.Key
  * Enumeration of key values
* KeyboardState
  * Structure containing keyboard information 

For a compleate overview of the namespace, check out [the doxygen](http://www.opentk.com/files/doc/namespace_open_t_k_1_1_input.html) pages for OpenTK.

## Keyborad Input
In order to read the keyboard, all you need is access to a ```KeyboardState``` instance.
Read [this article](http://www.opentk.com/doc/input/keyboard) before proceeding.

There above article get a ```KeyboardState``` reference using this code:

```cs
KeyboardState state = OpenTK.Input.Keyboard.GetState();
```

However this functionality is deprecated. It might get compleatly removed from future versions of OpenTK. Instead, to get a ```KeyboardState``` instance, you need access to an instance of ```OpenTK.GameWindow``` class. 

Luckly with our windowing code:
```cs

```