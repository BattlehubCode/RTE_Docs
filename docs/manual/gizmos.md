#Gizmos Docs
##Overview

__Runtime Gizmos__ are the runtime 3D controls that are used to manipulate items in the scene. Unlike [transform handles](transform-handles.md),
gizmos does not modify the transformation of objects. Instead, they are used to modify colliders, bounding boxes and properties of light and audio sources. All gizmos, their base classes,
rendering classes and shaders can be found in __/Battlehub/RTEditor/Runtime/RTGizmos__ and __/Battlehub/RTEditor/Content/Runtime/RTGizmos__  folders.

> [!NOTE]
> In this section, Runtime Gizmos is simply called "gizmos".

##Getting Started

 Here are several simple steps to get started with gizmos:
 
 1. Create a Cube using __Create->3D Object->Cube__
 2. Select Cube.
 3. Add Assets __BoxColliderGizmo__ component.
 4. Hit Play.
 5. Use mouse left-click + drag to resize Box Collider Gizmo.
 6. Note that `Center` and `Size` properties of Box Collider have been changed.
 
 ![Screenshot](~/resources/img/gizmos/getting-started/getting-started.png)
 
##Base Gizmo
Source code of Base Gizmo can be found in Battlehub/RTEditor/Runtime/RTGizmos/__BaseGizmo.cs__. This is the base class of [Box Gizmo](#box-gizmo), [Sphere Gizmo](#sphere-gizmo), [Capsule Gizmo](#capsule-gizmo) and
[Cone Gizmo](#cone-gizmo). Therefore all these gizmos have following settings:

 * `Grid Size` – step size used in unit snapping mode (default: 1.0).
 * `Line Color` – color of line.
 * `Handles Color` – color of handle (small quad).
 * `Selection Color` – color of selected handle.
 * `Enable Undo` – if set to true then gizmo will write all changes to undo stack (default: true).
 * `Unit Snap Key` – key switching gizmo to unit snapping mode.
 * `Target` – target object reference;

##Box Gizmo

Located in Battlehub/RTEditor/Runtime/RTGizmos/__BoxGizmo.cs__.  
Base class for all gizmos that have box shape:

 * [Box Collider Gizmo](#box-collider-gizmo)
 * [Skinned Mesh Renderer Gizmo](#skinned-mesh-renderer-gizmo)

##Sphere Gizmo

Located in Battlehub/RTEditor/Runtime/RTGizmos/__SphereGizmo.cs__.  
Base class for all gizmos that have sphere shape:

 * [Sphere Collider Gizmo](#sphere-collider-gizmo)
 * [Pointlight Gizmo](#point-light-gizmo)
 * [Audio Source Gizmo](#audio-source-gizmo)
 * [Audio Reverb Zone Gizmo](#audio-reverb-zone-gizmo)

##Capsule Gizmo

Located in Battlehub/RTEditor/Runtime/RTGizmos/__CapsuleGizmo.cs__.    
Base class for all gizmos that have capsule shape:

 * [Capsule Collider Gizmo](#capsule-collider-gizmo)

##Cone Gizmo

Located in Battlehub/RTEditor/Runtime/RTGizmos/__ConeGizmo.cs__.
Base class for all gizmos that have cone shape:

 * [Spotlight Gizmo](#spot-light-gizmo)

##Box Collider Gizmo

Located in Battlehub/RTEditor/Runtime/RTGizmos/__BoxColliderGizmo.cs__.  
Box Collider Gizmo can be added to object with Box Collider:

 1. Create Game Object with Box Collider.
 2. Add Box Collider Gizmo component.
 
 ![Screenshot](~/resources/img/gizmos/box-collider-gizmo.png)
 
##Sphere Collider Gizmo

Located in Battlehub/RTEditor/Runtime/RTGizmos/__SphereColliderGizmo.cs__.  
Sphere Collider Gizmo can be added to object with Sphere Collider:

 1. Create Game Object with Sphere Collider.
 2. Add Sphere Collider Gizmo component.
 
 ![Screenshot](~/resources/img/gizmos/sphere-collider-gizmo.png)
 
##Capsule Collider Gizmo

Located in Battlehub/RTEditor/Runtime/RTGizmos/__CapsuleColliderGizmo.cs__.  
Capsule Collider Gizmo can be added to object with Capsule Collider:

 1. Create Game Object with Capsule Collider.
 2. Add Capsule Collider Gizmo component.
 
 ![Screenshot](~/resources/img/gizmos/capsule-collider-gizmo.png)
 
##Point Light Gizmo

Located in Battlehub/RTEditor/Runtime/RTGizmos/__PointLightGizmo.cs__.  
Point Light Gizmo can be added to Point Light:

 1. Create Point Light.
 2. Add LightGizmo component.

 ![Screenshot](~/resources/img/gizmos/point-light-gizmo.png)

##Spot Light Gizmo

Located in Battlehub/RTEditor/Runtime/RTGizmos/__SpotLightGizmo.cs__.  
Spot Light Gizmo can be added to Spot Light:

  1. Create Spot Light.
  2. Add LightGizmo component.
  
  ![Screenshot](~/resources/img/gizmos/spot-light-gizmo.png)

##Directional Light Gizmo

Located in Battlehub/RTEditor/Runtime/RTGizmos/__DirectionalLightGizmo.cs__. 
Directional Light Gizmo can be added to Directional Light

  1. Create Directional Light.
  2. Add Light Gizmo component.
  
  ![Screenshot](~/resources/img/gizmos/directional-light-gizmo.png)

##Audio Source Gizmo

Located in Battlehub/RTEditor/Runtime/RTGizmos/__AudioSourceGizmo.cs__.  
Audio Source Gizmo can be added to Audio Source

  1. Create Audio Source.
  2. Add Audio Source Gizmo component.
  
  ![Screenshot](~/resources/img/gizmos/audio-source-gizmo.png)

##Audio Reverb Zone Gizmo

Located in Battlehub/RTEditor/Runtime/RTGizmos/__AudioReverbZoneGizmo.cs__.  
Same as [AudioSouce Gizmo](#audio-source-gizmo)

##Skinned Mesh Renderer Gizmo

Located in Battlehub/RTEditor/Runtime/RTGizmos/__SkinnedMeshRendererGizmo.cs__.  
Skinned Mesh Renderer Gizmo can be added to object with SkinnedMesh

  1. Create GameObject with SkinnedMeshRenderer.
  2. Add Skinned Mesh Renderer Gizmo component.
  
  ![Screenshot](~/resources/img/gizmos/skinned-mesh-renderer-gizmo.png)
