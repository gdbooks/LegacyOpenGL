# Color Tracking

We call it color tracking, but more often than not people simply refer to this technique as __Changing material paramaters with GL.Color__, sometimes it's also refered to as __color material mode__

Earlyer we saw that OpenGL uses the current material color (set with ```GL.Material```) when lighting is enabled, but uses the current primary color (set with ```GL.Color```) when lighting is disabled. This inconsistency can often get confusing. It's also slightly more expensive to call ```GL.Material``` than it is to call ```GL.Color``` because of the safety checks the functions do. Whenever you change a materials color, most often you only change the diffuse and ambient components.

color mateiral mode adresses these issues. When you enable color material mode, certain current material colors track the current primary color instead. To use color materials, you must first enable them:

```
GL.Enable(EnableCaps.ColorMaterial);
```

This will cause calls to ```GL.Color``` to change not only the primary color, but also the current ambient and diffuse material colors. Of course you can change which properties color tracking affects by calling ```GL.ColorMaterial```.

the 