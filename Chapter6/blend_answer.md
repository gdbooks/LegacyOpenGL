## During the initial setup function
This is the code we used to get the demo scene set up

### Initialize

```
public override void Initialize() {
    base.Initialize();
    GL.Enable(EnableCap.Lighting);
    GL.Enable(EnableCap.Light0);

    GL.Light(LightName.Light0, LightParameter.Position, new float[] { 0f, 1f, 1f, 0f });
    GL.Light(LightName.Light0, LightParameter.Ambient, new float[] { 0.2f, 0.2f, 0.2f, 1.0f });
    GL.Light(LightName.Light0, LightParameter.Diffuse, new float[] { 0.8f, 0.8f, 0.8f, 1.0f });
    GL.Light(LightName.Light0, LightParameter.Specular, new float[] { 1f, 1f, 1f, 1f });

    GL.Material(MaterialFace.FrontAndBack, MaterialParameter.Specular, new float[] { 1f, 1f, 1f, 1f });
    GL.Material(MaterialFace.FrontAndBack, MaterialParameter.Shininess, 20f);
    
    GL.Enable(EnableCap.ColorMaterial);
    GL.ColorMaterial(MaterialFace.FrontAndBack, ColorMaterialParameter.AmbientAndDiffuse);

    GL.LightModel(LightModelParameter.LightModelAmbient, new float[] { 0f, 0f, 0f, 1f });
}
```