#Rendering Notes
##IRTECamera

All transform handles and gizmos use IRTECamera interface in order to be rendered.

``` C#

Camera camera = Window.Camera;
IRTEGraphics graphics = IOC.Resolve<IRTEGraphics>();
if(graphics != null)
{
	IRTECamera rteCamera = graphics.GetOrCreateCamera(camera CameraEvent.AfterForwardAlpha);
	
	m_rteCamera.CommandBufferRefresh += OnCommandBufferRefresh;
	m_rteCamera.RefreshCommandBuffer();
}

protected virtual void OnCommandBufferRefresh(IRTECamera camera)
{
	CommandBuffer cmd = camera.CommandBuffer;
	
	//drawing method calls 
}
	
```

##Graphics Layer Camera
This is camera created automatically to render gizmos and transform handles.
`Clear Flags` of this camera is set to the _Depth only_ and `Culling Mask` to __Nothing__. At the same time _RuntimeGraphicsLayers 0-3_ will be excluded
from `Culling Mask` of scene camera.

![Screenshot](~/resources/img/rendering/runtime-graphics-layer.png)

> [!NOTE]
> [Runtime Editor](runtime-editor.md) use 9 layers [16-24]: for ui, for asset previews and for runtime graphics. 

![Screenshot](~/resources/img/rendering/layers-used-by-runtime-editor.png)