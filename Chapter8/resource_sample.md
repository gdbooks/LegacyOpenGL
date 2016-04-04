#Example

So now that we have a few managers let's put them to good use. I'm going to do this by making a ```TexturedModel``` class that will let me display textured 3D models.

```cs
class TexturedModel {
  protected int objHandle = -1;
  protected int texHandle = -1;
  
  public TexturedModel(string obj, string texture) {
    texHandle = TextureManager.Instance.LoadTexture(texture);
    objHandle = ModelManager.Instance.LoadModel(obj);
  }
  
  public void Shutdown() {
    TextureManager.Instance.UnloadTexture(texHandle);
    ModelManager.Instance.UnloadModel(objHandle);
    objHandle = texHandle = -1;
  }
  
  public void Render() {
    int texture = TextureManager.Instance.GetGLHandle(texHandle);
    OBJLoader model = ModelManager.Instance.GetModel(objHandle);
    
    GL.BindTexture(TextureTarget.Texture2D, texture);
    model.Render();
    GL.BindTexture(TextureTarget.Texture2D, 0);
  }
}
```

That's it. That's how simple it is to use our new manager classes to display any textured obj model. The constructor takes two arguments, both file paths. Each object is loaded up, a handle to each is kept and released accordingly.