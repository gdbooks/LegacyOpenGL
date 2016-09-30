# Specular Mapping

Specular Mapping is almost like normal mapping, but for the specular component. Becuase the specular component is just a single range value, the spec map is often added as the alpha channel of the normal map.

All specular mapping is is calculating the specular reflection of light per pixel. For example, here is a flat plane with a diffuse texture, normal map and specular map. Notice how it looks a little wet. That's because of the spec map:

![S1](SPEC1.jpeg)
