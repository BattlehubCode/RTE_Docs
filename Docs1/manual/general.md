#Introduction

![Screenshot](~/resources/img/rteditor/overview/overview.png)

This documentation covers most important parts of Runtime Editor. Unlike previous versions of the documentation, 
I tried to concentrate more on examples rather then documenting each property of each script. [Let me know](mailto:Battlehub@outlook.com) what examples you would like to see.

Start with following sections:

*  [Getting Started with Runtime Editor](runtime-editor.md#getting-started)
*  [Getting Started with Transform Handles](transform-handles.md#getting-started)
*  [Getting Started with Save & Load](save-load.md#getting-started)
*  [Expose To Editor](infrastructure.md#expose-to-editor)
*  [Event methods](infrastructure.md#event-methods)
*  [IOC](infrastructure.md#ioc)
*  [Runtime Selection](infrastructure.md#runtime-selection)
*  [Runtime Undo](infrastructure.md#runtime-undo)

##List of Features

  * Position Handle, Rotation Handle, Scale Handle, Rect Tool.
  * Grid, Box Selection, Scene Gizmo.
  * Global & Local coordinates, Local & Center pivot point modes, Vertex & Grid snapping.
  * Gizmos for Colliders, Lights and Audio Sources.
  * Scene navigation, Orthographic & Perspective mode.
  * Undo & Redo API.
  * Object selection API.
  * Object life-cycle Events.
  * Play & Edit mode.
  * __Configurable Inspector__.
  * Component & Material editors.
  * 20+ Built-in property editors.
  * __Ability to edit components of multiple selected objects.__
  * Add Component control.
  * __Multiple scenes__ and cameras support.
  * __Dock Panels__ & Windows Management.
  * Dialogs, Message Boxes, Confirmations.
  * Easy to extend with new windows.
  * __Configurable Main & Context menu__.
  * Lightweight Virtualizing Tree View for Hierarchy and Project windows.
  * __Configurable Save & Load subsystem__ (almost no coding is required).
  * __Easy to use Project API__.
  * __Static Assets, Asset Bundles and Runtime Assets support__.
  * Load assets on demand.
  * Multiple Projects support.
  * __Animation Editor__.
  * __Terrain Editor__.
  * __ProBuilder Integration__.
  * __Runtime Scripting__.
  * __Universal Render Pipeline__ support.
  * __HD Renderer Pipeline__ support (Beta).
  * __Mobile Support__.


###Breaking Changes

Old Views derived from RuntimeWindow are replaced with Views and ViewModels and [MVVM-style data-binding system for Unity](https://github.com/Real-Serious-Games/Unity-Weld). 
  * Old views and dialogs have been moved to the __Battlehub/RTEditor/Runtime/RTEditor/Legacy__ folder. 
  * Old prefabs can be found in the __Battlehub/RTEditor/Content/Runtime/RTEditor/Prefabs/Legacy__ folder.
  * New Views and ViewModels are located in __Battlehub/RTEditor/Runtime/RTEditor/Views__ and __Assets/Battlehub/RTEditor/Runtime/RTEditor/ViewsModels__ respectively.

If you want to use legacy built-in windows, select the __"Use Legacy Built-In Windows"__ toggle in the WindowManager component of the RuntimEditor prefab.

![Screenshot](~/resources/img/rteditor/overview/use-legacy-builtin-windows.png)

##Changelog

####RTE 3.5.0 - Dec 24, 2021

* RTE: SceneViewImpl, SceneViewModel ExposePrefabInstance modified to not expose full hierarchy.
* UI: Set VirtualizingListBoxItem.m_toggle.interactable to true by default
* UI: Added VirtualizingScrollRect.ItemsChanged event
* UI: Added VirtualizingListBoxBinding.CanRemove property 
* UI: Added VirtualizingListBoxBinding.CanReorder property 
* UI: Added VirtualizingListBoxBinding.CanSelect property 
* UI: Added VirtualizingListBoxBinding.Target property 
* UI: Changed VirtualizingListBoxBinding base class from AbstractMemberBinding to ControlBinding
* RTE: Added code to activate Dialog windows WindowManager.OnInputModuleUpdate()
* RTE: Added WindowOverlay to RuntimeEditor.prefab \UI\LayoutRoot\Middle\DockPanels\Mask\Modal
* RTE PeekMode: added Commands Panel
* RTE PeekMode: added Group, Ungroup, CreatePrefab buttons to GameObjectView
* RTE PeekMode: added "Begin Create" button
* RTE: Added Tools/Runtime Editor/Create Custom Window menu command
* RTE: Added ability to Create CustomView and CustomViewModel from template
* RTE: UnityWeld.dll Auto Reference flag set to True
* RTH: Added code to update ExposeToEditor.LocalEulerAngles from RotationHandle 
* RTE: Added support for Rotations > 360 degrees in Animation Editor
* RTE: Fixed AnimationViewModel undo & redo
* RTE: Fixed Legacy AnimationView undo & redo
* RTE: Added InspectorModel
* RTC: Deprecated RuntimeTools.ShowGizmos, ShowGizmosChanged, ShowSelectionGizmos, ShowSelectionGizmosChanged
* RTC: IOC.IsRegistered and IOC.IsFallbackRegistered methods added
* RTE: Models\Placement\PlacementModel added to RuntimeEditor.prefab
* RTE: Models\Grouping\GroupingModel added to RuntimeEditor.prefab
* RTE: Deprecated IRuntimEditorExt 
* RTE: Added IPlacementModel, PlacementModel
* RTE: Added IGroupingModel, GroupingModel
* RTE PeekMode: Added PeekModeCommandsHandler
* RTE PeekMode: Added CreatorWindow
* RTE:  Added Transform IWindowManager.Prompt(string header, string text, DialogAction<PromptDialogArgs> ok, DialogAction<PromptDialogArgs> cancel, string okText = "OK", string cancelText = "Cancel")
* RTE:  Added Transform IWindowManager.Prompt(Sprite icon, string header, string text, DialogAction<PromptDialogArgs> ok, DialogAction<PromptDialogArgs> cancel, string okText = "OK", string cancelText = "Cancel")
* RTE: Added EmptyView and EmptyDialog
* RTE: Added AutoUI
* RTE: added IProjectTreeViewModel, IProjectFolderViewModel as replacement for IProjectTree and IProjectFolder
* RTC: Added IRuntimeObjects.ComponentDestroyed event 
* RTC: Added ExposeToEditor._ComponentDestroyed event 
* RTE: IWindowManager.CreatePopup, .CreateDropdown methods added
* UI: Added RaiseTransformChanged() call to Start method of RectTransformChangeListener (in order to correctly initialize VirtualizingScrollRect)
* UI: Added Region.Destroy method
* UI: Added DockPanel.AddDropdownRegion and .AddPopupRegion methods with various overloads.
* RTE: Added IRuntimeEditor.StopCoroutine
* RTE: Added IWindowManager Confirmation overloads
* UI: Added DialogManager .ShowComplexDialog
* UI: Added Dialog Alt button, Alt event and Alt action 
* RTH: Changed RuntimeHitTester DefaultExecutionOrder to -89
* RTB: Added support for Probuilder 4.4.0, 5.0.3
* RTC: Added IRuntimeUndo.AddComponentWithRequirements 
* RTH: Added MinOrthoSize, MaxOrthoSize properties to RuntimeSceneComponent
* RTE PeekMode: Dropdowns, add component panel, add component viewmodel
* RTE PeekMode: GameObjectName toggle + styling
* RTE PeekMode: ContextPanel using minimum distance = 50 when Empty Game Object selected
* UI: Fixed NullReferenceException when opening a menu control with more than 2 levels of depth
* RTE PeekMode: Added ComponentViewModel
* RTE PeekMode: Added ContextPanelViewModel
* RTE: Component Editor Icons added
* RTE: Added Default Component Editor Icon
* RTE: Added ComponentEditorSettings.ShowIcon and ComponentEditorSettings.Icon fields
* RTE: Added HeaderDescriptor.ShowIcon and HeaderDescriptor.Icon fields
* RTE: Added Component Editor Icon 
* RTE: Implemented PeekMode context panel visibility and positioning
* RTB: Highlight AutoUV with different color in Manual UV Editor window
* RTB: Hidden position/rotation/scale handle axes if no faces selected in uv editing mode
* RTB: Fixed bug where ManualUVRenderer was not refreshed correctly on mesh editing to uv editing mode change
* RTB: Fixed bug where ManualUVRenderer was not refreshed correctly on vertex/edge/face mode change
* UI: Added SceneName parameter to  MenuDefinitionAttribute (defines scene which menu applies to)
* RTE: Added PeekModeScene and PeekModeLayout
* RTE: Added BuiltInWindowNames
* RTE: Added LayoutExtension
* UI:  Added LayoutInfo  LayoutInfo.Horizontal(LayoutInfo top, LayoutInfo bottom, float ratio = 0.5f);
* UI:  Added LayoutInfo  LayoutInfo.Vertical(LayoutInfo left, LayoutInfo right, float ratio = 0.5f);
* UI:  Added LayoutInfo  LayoutInfo.Group(params LayoutInfo[] tabGroup);
* RTE: Added Transform IWindowManager.CreateWindow(string windowTypeName, out WindowDescriptor wd);
* RTE: Added Transform IWindowManager.CreateWindow(string windowTypeName, out WindowDescriptor wd, out bool isDialog);
* RTE: Added LayoutInfo IWindowManager.CreateLayoutInfo(Transform content, WindowDescriptor desc);
* RTH: Added SelectionPicker (corresponding code removed from BoxSelection and BoxSelectionRenderer)
* RTE: Replaced UnityEvent HideInInspector with NonSerialized attribute 
* RTE: Added ReplaceSceneViewAndViewModelExample
* RTE: Added ability to overrided views
* RTE:  Added SceneViewModelOverride example
* RTE:  Added ability to override view models
* RTSL: Added support for objects with subassets imported from asset bundles.
* RTB: Material palette view tooltips added
* RTE: Added LocalizedTooltip
* RTC: SetCursor if m_currentCursorType != cursorType. CursorHelper.SetCursor return value changed from void to bool.
* RTE: ImportFileDialog replaced
* RTE: Fixed TimelineControl key already exists exceptions when adding row with ColumnCount = 1
* RTE: AnimationSelectPropertiesDialog replaced
* RTH: Fixed SceneGizmo.DoSceneGizmo
* UI:  DockPanel.RootRegion.FrameImage.enabled = false
* RTE: AnimationView replaced
* RTE: add BubbleUpPointerExit only if IsTouchSupported == true
* RTT: TerrainEditor instantiated by TerrainView OnEditorSelectionChanged
* RTT: store selected layer index. 
* RTSL: Fixed compatibility with 2.11 - 2.26 files
* RTB:  ManualUVEditorView, ManualUVSceneComponent, ManualUVRenderer fixed to work with Universal and HD RP
* RTSL: Updated PersistentSprite class to save textures which are not mapped.
* RTE: Replaced SaveAssetsDialog
* RTE: Replaced SaveFileDialog
* RTE: Replaced OpenFileDialog
* RTE: ProjectTreeViewModel select assets folder when selected folder removed
* RTE: ProjectTreeViewModel.SelectedItem accepts items for which CanDisplay returns true
* RTH: Added SelectionMode { UseColliders - default, UseRenderers, UseRenderersBeforeColliders }. 
* RTT: Removed ProjectItemView from LayerListBoxItem.prefab
* UI: VirtualizingTreeViewItem.ItemPresenter.Label Overflow set to Ellipsis
* RTE: Added bool ShowLayers = true 
* RTE: Added GameObjectEditorSettings 
* RTE: ZipConstants.DefaultCodePage = System.Text.Encoding.UTF8.CodePage;
* RTE: Added 'select' parameter to public static void AddGameObjectToScene(this IRTE editor, GameObject go, bool select = true) method
* RTC: Added ImputLowForRDP.cs
* RTH: #pragma target 3.5 removed from OutlineBlur, OutlinePrepass, OutlineCompomosite
* RTE: private string[] m_hiddenValues added to EnumEditor
* RTE: virtual modifer added to ComponentEditor.BuildEditor and .OnExpanded methods 
* RTE: bool? IsExpandedByDefault added to ComponentEditorSettings (used to calculate defaultValue in IsComponentExpanded getter)
* RTE: Replaced ImportAssetsDialog
* RTE: Replaced SelectAssetsDialog
* RTE: Renderers with isPartOfStaticBatch == true excluded from BoxSelection
* RTE: Using Screen.currentResolution.width instead of Display.main.systemWidth
* RTE: BaseGizmoInput DefaultExecution order changed to -59
* RTE: BaseGizmo public virtual Reset method
* RTE: Replaced ToolsPanel 
* RTE: Replaced SettingsDialog
* RTE: Fixed Default Layout procedure 
* RTE: Added RectTool text outline
* RTE: Added HierarchicalDataView
* RTE: Implemented HierarchicalDataViewModel common drag & drop handling
* RTE: Replaced ProjectView 
* RTE: Fixed VirtualizingTreeView CanBeParent, CanReparent flags behaviour 
* RTE:  Added ability to disable foreground layer in Universal RP (If disabled, then the windows with the camera should not go to the floating mode).
* RTE:  Added m_depthMaskOffset field to DepthMaskingBehaviour (presumably fixes window depth masking bug on MacOS?)
* RTE:  Using the default UI shader from Unity 2020 #if UNITY_2019 (fixes incorrect transparency of the ui in windows placed above the scene)
* RTSL: GetUOAssembliesAndTypes moved to ReflectionUtils.cs
* RTSL: UnityWeld excluded from assemblies array in CodeGen.GetUOAssembliesAndTypes and EditorsMapWindow.GetUOAssembliesAndTypes
* RTE: Replaced SelectObjectDialog
* RTE: Replaced SelectColorDialog
* RTE: Added IWindowManager.CreateDialogWindow overload without preferredWidth, preferredHeight parameters
* RTE: Replaced ManageProjectsDialog
* RTSL: ProjectInfo .Name, .LastWriteTime fields replaced with properties
* RTE: canResize = true param added to IWindowManager.Dialog methods
* RTE: canResize = true param added to Workspace.Dialog methods
* RTE: Replaced SaveSceneDialog
* RTE: Added DialogBinding
* RTE: OneWayPropertyBindingSlim and TwoWayPropertyBindingSlim added to ControlBinding
* RTSL: ProjectItem .Name, .Ext, .Children, .Parent fields replaced with properties
* RTE: Added missing functionality to write gameobject name changes to undo stack.
* RTE: Activated, Deactivated events added to RuntimeWindow
* RTE: AddComponentControl moved from InspectorView to GameObjectEditor
* RTE: Replaced InspectorView
* RTE: Ability to override Inspector selection.
* RTH: Added BoundingSphereRadius property to RuntimeSceneComponent 
* RTE: Added RuntimeGameWindow 
* RTE: Replaced SceneView
* RTE: Replaced ConsoleView
* RTE: Replaced HierarchyView
* RTSL: Added .Load_Async<T> method with correct Task<T> return type.
* RTSL: IProjectAsync .DeleteFolderAsync, .DeleteAsync don't throw an exception if ProjectItem doesn't exist
* RTE: View, ViewBinding added
* UI:  Added PrefabIndex to MenuItemInfo
* UI:  Replaced Menu m_menuItemPrefab with m_menuItemPrefabs
* RTE: Fixed Strong and EditorsMap classes (Universal Windows Platform)
* UI: Added VirtualizingTreeViewBinding .ItemClick, .ItemHold, .ItemDoubleClick, .ItemExpanded, .ItemCollapsed, .ItemDragEnter, .ItemDragExit events
* UI: Added VirtualizingTreeViewBinding .Target property 
* UI: VirtualizingItemsControl .ItemClick event no longer occurs after ItemDoubleClick operation.
* UI: VirtualizingItemsControl .ItemClick event no longer occurs during a drag-and-drop operation.
* UI: Added VirtualizingTreeViewBinding DragItems, DropTargets
* UI: Added VirtualizingTreeViewBinding Drag&Drop Unity Events
* UI: Added ControlBinding, EventBindingSlim
* RTE: Added Project LockAsync WebGL implementation
* UI: VirtualizingListBox binding added
* UI: VirtualizingListBox added
* UI: VirtualizingScroller continuous mode added
* RTC: Added .Name property to ExposeToEditor.
* RTE: Added BuiltInWindows component added to Runtime Editor prefab. 
* RTE: Added WindowManager.m_useLegacyBuiltInWindows = false.
* RTE: Added View prefabs, ViewModels and ViewModelTests.
* RTE: Existing view and dialog prefabs moved to Battlehub\RTEditor\Content\Runtime\RTEditor\Prefabs\Legacy.
* RTE: Existing views and dialogs moved to Battlehub\RTEditor\Runtime\RTEditor\Legacy.
* UI:  MenuItem quick fix for InputSystem and mouse support
* RTE: InputSystem 1.0.1 package added
* RTE: WindowManager will call Editor.ActivateWindow only if ActiveWorkspace.ActivateWindow returns true;
* RTE: Workspace .ActivateWindow method fixed. Allow to activate window only in case if m_isTabDragInProgress == false;
* UI: Region .Insert method fixed. Turn on the new tab *first* or else Unity's UI.Toggle.Set() won't let us turn off the tab that was previously active.
* UI: Tab m_toggle.interactable = false; this will prevent the Toggle event from firing twice.
* RTE: Added HierarchyViewModel, HierarchyView prefab
* RTE: Deprecated prefabs from RTEditor\Content\Runtime\Prefabs\View and Dialog folders
* RTE: Deprecated AboutDialog, AssetLibraryImportDialog, AssetLibrarySelectDialog, ImportFileDialog, InputDialog, OpenFileDialog, ProjectsDialog, SaveAssetsDialog, SelectColorDialog, SelectObjectDialog, SettingsDialog
* RTE: Deprecated ProjectFolderView, ProjectFolderViewImpl, ProjectItemView, ProjectTreeViewImpl, ProjectView, ProjectViewImpl
* RTE: Deprecated SceneView, SceneViewImpl, GameView, HierarchyView, HierarchyViewImpl, InspectorView, ConsoleView, ToolsPanel
* RTE: Scene cursor cannot be changed when popup is displayed.
* RTE: Removed DragField from RangeEditor and RangeIntEditor prefabs.
* RTE: URP, HDRP UIForeground transparency fix for unity 2019.
* UI: Added IHierarchicalData<T> interface.
* UI: Added INotifyHierarchicalDataChanged interface.
* UI: Added VirtualizingTreeViewBinding editor.
* UI: VirtualizingTreeViewBinding added.
* RTE: Added UnityWeld.dll.

####RTE 2.25 - Sept 30, 2020

* Project structure: [reorganized to support unity package manager](https://docs.unity3d.com/Manual/cus-layout.html)

* Inspector: ability to select and edit multiple game objects and components.
* Inspector: layers editor.
* Inspector: layer mask editor.
* Inspector: culling mask added to camera component editor.
* Inspector: add component button keyboard navigation.
* Inspector: ability to configure visibility of buttons.
* Inspector: support for components with RequireComponent attribute.

* Hierarchy: search field added.
* Hierarchy: ability to override HierarchyView.
* Project: search field added.
* Project: ability to duplicate folders.
* Project: ability to override ProjectFolderView.
* Project: ability to override ProjectTreeView.
* Scene: ability to focus all active in hierarchy game objects with renderers.
* Scene: select objects on mouse button up.
* Scene: ability to select objects without collider.
* Scene: new box selection mode - pixel perfect depth test.
* Scene: auto save layout.
* DockPanel: Region.ClassAllTabs method added.
* DockPanel: variable tab size support.
* DockPanel: LayoutInfo must be created using IWindowManager.CreateLayoutInfo method.
* Theming: ability to set and update theme at runtime.
* Settings: dialog added.


* ProBuilder: uv unwrapping window.
* ProBuilder: uv stitch tool.
* ProBuilder: smooth groups editor.

* Graphics: HD Render Pipeline Support (Beta).
* Graphics: Universal Render Pipeline Support.
* Graphics: [GL](https://docs.unity3d.com/ScriptReference/GL.html) is not used anymore and replaced with [CommandBuffer](https://docs.unity3d.com/ScriptReference/Rendering.CommandBuffer.html).

* RTSL: k__BackingField and delegates are not visible in Persistent Classes Window anymore.
* RTSL: separate RuntimeTypeModel.dll for each build target.
* RTSL: generating link.xml to support IL2CPP scripting backend.
* RTSL: mesh, texture, audio data imported from asset bundle is not saved on disk anymore.
* RTSL: save & load time and memory consumption reduced.
* RTSL: preventing the Editor from crashing when entering play mode with selected asset library.
* RTSL: support for different types of identifiers.
* RTSL: Autogenerated TypeMap.cs replace with TypeMapCreator
* RTSL: [CustomImplementation] class structure changed
* RTSL: IProject.ToID replaced with IProject.ToPersistentID method.
* RTSL: IProject.FromID replaced with IProject.FromPersistentID method.
* RTSL: IStorage interface replaced with IStrorage<> with PersistentObject<> params

####RTE 2.11 - Jan 30, 2020	
* Terrain details & trees editor

####RTE 2.10 - Dec 30, 2019
* Terrain editor.
* ProBuilder integration.
* Animation editor.
* Runtime scripting.
* Localization.
* Transform Handles: Rect Tool.

####RTE 2.05 - April 30, 2019
* Ability to change RTE UI Colors.
* UI.Text replaced with TextMeshProUGUI;
* File importers -> obj, png, jpg;
* VTV.SelectedIndex is set to -1 when Selected Item is null;
* Creating Primitives with Default-Material;
* ProjectItem.Get method fixed;
* Loading last project by default;
* Create project folder if there was no project before.
* Create camera and directional light by default.
* Dynamic resouces cleanup procedure fixed;
* Project Folder Drag & Drop bug fixed.

####RTE 2.04 - March 30, 2019
* Rendering scene view and game view using RenderTexture and RawImage;
* LWRP support;
* Transform Handles: position handle freeze bug fixed.
* Transform Handles: correct model scale.
* Duplicate scene bug fixed.
* "Open" context menu item added.
* RuntimeWindow CanActivate property added.
* Ability to create tab groups.
* IOC container unloading bug fixed.

####RTE 2.03 - February 13, 2019
* Configurable layout.
* Configurable tools panel.
* Configurable ui scale.
* Editor mappings are stored in RTEditor_Data folder and will not be overriden anymore.
* RTSL: TypeModel.dll correct import settings.
* RTSL: close progress bar in case of exception.
* Trasnform Handles: nullref exception fixed.

#### RTE 2.02 - January 30, 2019
* RTSL: codegen & persistent class storage fixed.
* RTSL: camera.pixelRect saved by default.
* RTSL: demo scene update.
* Transform Handles: demo scene update.

#### RTE 2.01 - January 13, 2019
* Set default layout nullref exception fixed.
* Transform Handles: prefabs added;
* Transform Handles: demo scene;
     	 
####RTE 2.00 - Dec 30, 2018
* Runtime save-load subsystem.
* Ability create/manage projects.
* Project API.
* Assets & Asset bundles importer.
* Configurable layout (dock panels).
* Configurable main & context menu.
* New dialog windows and message boxes.
* Multiple viewports supported.
* Runtime handles touch support.
* Simple depency injection (IOC).
* Undo & Redo API.
* Object selection API.
* Object life-cycle events.
* Add components control.
* Console window.
* Lists and Hierarchies virtualization.

##About
Hi, I am [Vadym](https://www.facebook.com/vadim.andriyanov). I put a lot of effort into creating the Runtime Editor, but it was an interesting and rewarding experience. The first version of Runtime editor was released in June 2016 and was pretty simplistic. The current version is much more sophisticated but at the same time much more flexible and contains many useful features. 
If you have any questions or suggestions send them by email to [Battlehub@outlook.com](mailto:Battlehub@outlook.com) or join this [support group](https://t.me/battlehub). I hope you enjoy using the Runtime Editor and it will be useful.