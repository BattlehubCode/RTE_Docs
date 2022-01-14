#Transform Handles Docs
##Overview

__Runtime Transform Handles__ are the __runtime__ 3D controls that are used to manipulate items in the scene. There three built-in transform tools to [position](#position-handle), [rotate](#rotation-handle) and [scale](#scale-handle) object via transform component. 
Another special built-in tool - the [rect tool](#rect-tool) allows you to move and change scale of game objects. Supplementary controls such as [scene gizmo](#scene-gizmo), selection gizmo and [grid](#grid) allows to change viewing angle and projection mode, identify selected objects and orientate in scene space. 
Another important components: [selection component](#runtime-selection-component), [scene component](#runtime-scene-component) and [handles component](#runtime-handles-component) allows to select objects, navigate in scene and change transform handles appearance.
Scripts, prefabs and example scenes for transform handles can be found in Assets/Battlehub/__RTHandles__ folder.

> [!NOTE]
> Runtime Transform Handles are simply referred as transform handles through this text
   

Here is how transform handles and components rendered in the game view:
![Screenshot](~/resources/img/transform-handels/overview/overview.png)

All in one:

![Screenshot](~/resources/img/transform-handels/overview/all-in-one.png)


##Getting Started

Here are four simple steps to get started with transform handles:

  1. Create transform handles using __Tools->Runtime Handles->Create__.
	![Screenshot](~/resources/img/transform-handels/getting-started/step1-create-transform-handles.png)
 &nbsp;
  2. Create and select Game Object.
  	![Screenshot](~/resources/img/transform-handels/getting-started/step2-create-object.png)
	&nbsp;
  3. Enable object editing with __Tools->Runtime Handles->Enable Editing__.
	![Screenshot](~/resources/img/transform-handels/getting-started/step3-enable-editing.png)
	&nbsp;
  4. Hit play. After clicking on the game object in the game view you should see the following:
	![Screenshot](~/resources/img/transform-handels/getting-started/step4-hit-play.png)

Following actions available by default:
  
Action       | Input
------------ | ------------- 
Select   	 | Left-click
Move (Pan)   | Hold middle mouse button, then drag
Flythrough   | Click and hold the right mouse button. Move around using mouse and WSAD keys.
Orbit        | Hold Alt+left-click, then drag 
Zoom         | Use the scroll wheel
Undo		 | Shift + Z (Ctrl + Z in player)  
Redo		 | Shift + Y (Ctrl + Y in player) 
  
> [!NOTE]
> Example scenes can be found in Assets/Battlehub/RTEditorDemo/Content/Runtime/RTHandles
	
	
##Base Handle

This is the base class for position, rotation, scale transform handles. Therefore all transform handles have following settings:

   * `Window` - reference to the [window](infrastructure.md#runtime-window). Interaction with transform handle allowed only in case referenced window is active. 
   * `HighlightOnHover` - bool value. If true then transform handles will be highlighted on pointer over. Set it to false if touch input is used. (default: true)
   * `Appearance` - reference to the [runtime handles component](#runtime-handles-component). Various visual settings.  
   * `Model` - reference to the prefab with BaseHandle model component attached. If Model is set then default rendering procedure will be disabled and prefab instance will be rendered instead.
   * `Targets` - array of transforms to be modified by transform handle.
   * `GridSize` - float value used in unit-snapping mode. By default Unit-snapping mode is activated with 'Shift' key.
   * `BeforeDrag` - event raised before transform handle drag.
   * `Drag` - event raised each frame during drag and drop operation.
   * `Drop` - event raised when drag and drop operation completed.


##Position Handle

To create a position handle, do the following:
	
   1. Create a new Game Object.
   2. Add Assets/Battlehub/RTEditor/Runtime/RTHandles/__PositionHandle__ component.

![Screenshot](~/resources/img/transform-handels/position-handle/create-position-handle.png)

The colors and scale of the transform handles can be adjusted using [Runtime Handles Component](#runtime-handles-component), and the proportions of individual parts such as lines and arrows can be adjusted using the __Position Handle Model__ script attached to PositionHandleModel.prefab. 
To change the proportions of the position handle, modify Assets/Battlehub/RTEditor/Content/Runtime/RTHandles/Prefabs/<br>__PositionHandleModel.prefab__ and add a reference to it using `Model` field of the Position Handle.

![Screenshot](~/resources/img/transform-handels/position-handle/set-model-field.png)

Position Handle Model script has following fields:

   * `Radius` - thickness of the line (default: 0,01).
   * `Length` - lenght of the line (default: 1).
   * `Arrow Radius` - radius of the arrow (default 0,075).
   * `Arrow Length` - lenght of the arrow (default 0,2).
   * `Quad Length`  - size of xy, xz and yz quads (default 0,2).

Adjustments example:
![Screenshot](~/resources/img/transform-handels/position-handle/position-handle-model-adjustment.png)


##Rotation Handle

To create a rotation handle, do the following:

   1. Create a new Game Object.
   2. Add Assets/Battlehub/RTEditor/Runtime/RTHandles/__RotationHandle__ component.

![Screenshot](~/resources/img/transform-handels/rotation-handle/create-rotation-handle.png)

The colors and scale of the transform handles can be adjusted using [Runtime Handles Component](#runtime-handles-component), and the proportions of individual parts can be adjusted using the __Rotation Handle Model__ script attached to RotationHandleModel.prefab. 
To change the proportions of the rotation handle, modify Assets/Battlehub/RTEditor/Content/Runtime/RTHandles/Prefabs/__RotationHandleModel.prefab__ and add a reference to it using `Model` field of the Rotation Handle.

![Screenshot](~/resources/img/transform-handels/rotation-handle/set-model-field.png)

Rotation Handle Model script has following fields:

   * `Minor Radius` - line thickness (default: 0,0075).
   * `Major Radius` - x-axis, y-axis, z-axis, inner circle radius (default: 1).
   * `Outer Radius` - outer circle radius (default: 1,11).
   
Adjustments example:
![Screenshot](~/resources/img/transform-handels/rotation-handle/rotation-handle-model-adjustment.png)
   
##Scale Handle
To create a scale handle, do the following:

   1. Create a new Game Object.
   2. Add Assets/Battlehub/RTEditor/Runtime/RTHandles/__ScaleHandle__ component.
   
![Screenshot](~/resources/img/transform-handels/scale-handle/create-scale-handle.png)

The colors and scale of the transform handles can be adjusted using [Runtime Handles Component](#runtime-handles-component), and the proportions of individual parts can be adjusted using the __Scale Handle Model__ script attached to ScaleHandleModel.prefab. 
To change the proportions of the scale handle, modify Assets/Battlehub/RTEditor/Content/Runtime/RTHandles/Prefabs/__ScaleHandleModel.prefab__ and add a reference to it using `Model` field of the Scale Handle.

![Screenshot](~/resources/img/transform-handels/scale-handle/set-model-field.png)

Scale Handle Model script has following fields:

   * `Radius` - line thickness (default: 0,01).
   * `Length` - line length (default: 1).
   * `Arrow Radius` - cap size (default: 0,05).

Adjustments example:
![Screenshot](~/resources/img/transform-handels/scale-handle/scale-handle-model-adjustment.png)

##Rect Tool

To create a rect tool, do the following:

   1. Create a new Game Object.
   2. Add Assets/Battlehub/RTEditor/Runtime/RTHandles/__RectTool__ component.
   3. Set Targets field.
      
The colors and scale of the rect tool can be adjusted using Runtime Handles Component.

##Locking Axes 
 
The Assets/Battlehub/RTEditor/Runtime/RTCommon/__LockAxes__ component prevents the modification of certain transformations with the [position](#position-handle), [rotation](#rotation-handle) or [scale](#scale-handle) handles. Attach it to [selectable](#runtime-selection-component) Game Object and choose axes you want to lock.

![Screenshot](~/resources/img/transform-handels/lock-axes/position-handle-x-y-locked.png)

##Scene Gizmo

To create a scene gizmo, do the following:

  1. Create a new Game Object.
  2. Add Assets/Battlehub/RTEditor/Runtime/RTHandles/__SceneGizmo__ component.
  
  ![Screenshot](~/resources/img/transform-handels/scene-gizmo/create-scene-gizmo.png)
  
Scene gizmo script has following fields:
  
   * `Window` - reference to the [window](infrastructure.md#runtime-window). Interaction with scene gizmo allowed only in case referenced window is active. 
   * `Btn Projection` - UGUI button to switch between orthographic and perspective projection (default: None).
   * `Pivot` - transformation of the object around which the camera rotates.
   * `Size` - scene gizmo rect size (default: 96,96).
   * `Pivot Point` - scene gizmo rect pivot point (default: 1,0).
   * `Anchor` - scene gizmo rect anchor (default: 1,0).
   * `Appearance` - reference to the [runtime handles component](#runtime-handles-component). Various visual settings.   
   * `Orientation Changing` -  event fires when camera rotation and position are about to be changed.
   * `Orientation Changed` - event fires when camera rotation and position are changed.
   * `Projection Changed` - event fires when camera projection changed.

##Grid

To create a grid, do the following:
  
   1. Create a new Game Object.
   2. Add Assets/Battlehub/RTEditor/Runtime/RTHandles/__SceneGrid__ component.
   
![Screenshot](~/resources/img/transform-handels/grid/scene-grid.png)

To change color of the grid use `Grid Color` field of [runtime handles component](#runtime-handles-component).
   
##Runtime Handles Component

Use this script to change appearance of transform handles:

   1. Create a new Game Object.
   2. Add Assets/Battlehub/RTEditor/Runtime/RTHandles/__RuntimeHandlesComponent__ component

![Screenshot](~/resources/img/transform-handels/runtime-handles-component/runtime-handles-component.png)
   
Runtime Handles Component script has following fields:

   * `Colors` - transform handles, scene gizmo, selection gizmo and grid colors.
   * `Handle Scale` - transform handles scale (default: 1).
   * `Scene Gizmo Scale` - scale of the scene gizmo (default: 1).
   * `Selection Margin` - scale of clickable area around transform handle axes (default: 1).
   * `Invert Z Axis` - invert position handle z-axis (default: false).
   * `Position Handle Arrows Only` - hide xy, yz and xz quads (default: false).
   
   
##Runtime Selection Component

This is the implementation of default selection behavior.
To enable default selection behavior:

  1. Create a new Game Object. 
  2. Add Assets/Battlehub/RTHandles/Scripts/__RuntimeSelectionComponent__.
  3. Choose objects you want to make selectable and click __Tools->Runtime Handles->Enable Editing__.
	   * Alternatively add [Assets/Battlehub/RTEditor/Runtime/RTCommon/__ExposeToEditor__](infrastructure.md#expose-to-editor).
  4. To visualize selection add __OutlineManager__ to object with Runtime Selection Component.
  
	   
![Screenshot](~/resources/img/transform-handels/runtime-selection-component/runtime-selection-component.png)

Following actions defined in Assets/Battlehub/RTHandles/Scripts/Input/__RuntimeSelectionInput__ :
  
Action          | Input
-------------   | ------------- 
Select   	    | Left-click
Select multiple | Hold Shift + left-click (Ctrl + left-click in player)
Select all      | Hold Shift + A (Ctrl + A in player)

<br>
Runtime Selection Component script has following fields:

  * `Window` - [window reference](infrastructure.md#runtime-window).
  * `Position Handle` - [position handle reference](#position-handle).
  * `Rotation Handle` - [rotation handle reference](#rotation-handle).
  * `Scale Handle` - [scale handle reference](#scale-handle).
  * `Rect Tool` - [rect tool reference](#rect-tool).
  * `Box Selection` - [box selection reference](#box-selection).
  * `Grid` - [scene grid reference](#grid).
    
To switch between transform handles using Q W E R T keys, do the following : 

   1. Add Assets/Battlehub/RTEditor/Runtime/RTHandles/Input/__RuntimeToolsInput__ component.
   
> [!NOTE]
> If you wish to switch between transform handles programmatically proceed to [-> this <-](infrastructure.md#runtime-tools) section.

> [!NOTE]
> If you wish to change selection programmatically proceed to [-> this <-](infrastructure.md#runtime-selection) section.

##Box Selection

To create a Box Selection, do the following:
  
   1. Create a new Game Object. 
   2. Add Assets/Battlehub/RTEditor/Runtime/RTHandles/__BoxSelection__ component.
	
![Screenshot](~/resources/img/transform-handels/box-selection/create-box-selection.png)

> [!NOTE]
> Game Objects without [__ExposeToEditor__](infrastructure.md#expose-to-editor) component are invisible to Box Selection.
	
> [!NOTE]
> Box Selection use [Runtime Selection API](infrastructure.md#runtime-selection) to store selected objects.
	

Box Selection has following fields:

  * `Window` - reference to the [window](infrastructure.md#runtime-window).
  * `Graphics` - sprite which is used to render box selection.
  * `Use Camera Space` - use camera space for rendering (true) or screen space (false) (default:true)
  * `Box Selection Method` - box selection method:
	* _Vertex_ - use MeshFilter.sharedMesh.vertices (default);
	* _Loose Fitting_ - use renderer bounds and collider (default);
	* _Bounds Center_ - use bounds center;
	* _Transform Center_ - use transform center;

Use following code to filter objects selected using Box Selection:
	
``` C#
using Battlehub.RTHandles;
using UnityEngine;

//In this example objects with name "Capsule" are filtered out

public class FilteringBehaviour : MonoBehaviour
{
    [SerializeField]
    private BoxSelection m_boxSelection;

    private void Start()
    {
        m_boxSelection.Filtering += OnFiltering;
    }

    private void OnDestroy()
    {
        if (m_boxSelection != null)
        {
            m_boxSelection.Filtering -= OnFiltering;
        }
    }

    private void OnFiltering(object sender, FilteringArgs e)
    {
        if (e.Object.name == "Capsule")
        {
            e.Cancel = true;
        }
    }
}
```

##Runtime Scene Component

Runtime Scene Component extends [Runtime Selection Component](#runtime-selection-component) and enables mouse orbiting, movement and zoom.

To create a Runtime Scene Component, do the following:

  1. Create a new Game Object
  2. Add Assets/Battlehub/RTEditor/Runtime/RTHandles/__RuntimeSelectionComponent__.
  
![Screenshot](~/resources/img/transform-handels/runtime-scene-component/create-runtime-scene-component.png)

Runtime Scene Component has following fields:
  
  * `View Texture` - cursor which is visible during mouse orbiting.
  * `Move Texture` - cursor which is visible during movement.
  * `Free Move Texture` - cursor which is visibile in flythrough mode.
  * `Scene Gizmo` - reference to the [Scene Gizmo](#scene-gizmo).
  
Actions defined in Battlehub/RTEditor/Runtime/RTHandles/Input/__RuntimeSceneInput__ :
  
Action       | Input
-------------| ------------- 
Focus   	 | Press F
Snap To Grid | Press Shift + G (Ctrl + G in player)
Move (Pan)   | Hold middle or right mouse button, then drag
Flythrough   | Click and hold the right mouse button. Move around using mouse and WSAD keys.
Orbit        | Hold Alt+left-click, then drag 
Zoom         | Use the scroll wheel

<br>
