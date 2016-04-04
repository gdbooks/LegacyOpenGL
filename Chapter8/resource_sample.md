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
}
```