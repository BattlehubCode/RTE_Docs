#Common Infrastructure Docs
##Overview

Common infrastructure classes and interfaces form core api of runtime editor. Among other there are [selection](#runtime-selection), [object lifecycle](#runtime-objects), 
[tools](#runtime-tools), [undo-redo](#runtime-undo) and [drag&drop](#drag-and-drop) API's.

##Expose To Editor

Add /Battlehub/RTEditor/Runtime/RTCommon/__ExposeToEditor__ component to any Game Object you want to make available for selection and editing.

![Screenshot](~/resources/img/infrastructure/expose-to-editor/expose-to-editor.png)

UnityEvents:

 * `Selected Event` raised when object added to [Runtime Selection](#runtime-selection).
 * `Unselected Event` raised when object removed from [Runtime Selection](#runtime-selection).
 
Fields & Properties:

 * `AddColliders` – add colliders to enable selection? (default: true).
 * `Bounds Object` – reference to the GameObject which will be used to calculate bounds.
 * `Custom Bounds` – used if BoundsType == Custom.
 * `Bounds Type`:  
	   * _Mesh_ – MeshFilter.mesh.bounds will be used to calculate bounds.
	   * _SkinnedMesh_ – SkinnedMeshRenderer.mesh.bounds to calculate bounds.
	   * _Sprite_ - SpriteRenderer.sprite.bounds will be used to calculate bounds.
	   * _RectTransform_ CalculateRelativeRectTransformBounds method will be used to calculate bounds.
	   * _Custom_ – user defined bounds.
	   * _Any_ – any of the above.
	   
##Event methods

Event methods are called for all scripts of game objects with the [Expose To Editor](#expose-to-editor) component attached. 

> [!NOTE]
> RuntimeAwake, RuntimeStart, OnRuntimeDestroy,  OnRuntimeActivate, OnRuntimeDeactivate are called in play mode only. 
> See `IsPlaying` property of [IRTE interface](#irte-interface)


```C#
using UnityEngine;

public class ScriptEventsExample : MonoBehaviour
{
	private void OnRuntimeEditorOpened()
    {
        Debug.Log("Editor Opened");
    }

    private void OnRuntimeEditorClosed()
    {
        Debug.Log("Editor Closed");
    }

    private void RuntimeAwake()
    {
        Debug.Log("Awake in play mode");
    }

    private void RuntimeStart()
    {
        Debug.Log("Start in play mode");
    }

    private void OnRuntimeDestroy()
    {
        Debug.Log("Destroy in play mode");
    }

    private void OnRuntimeActivate()
    {
        Debug.Log("Game View activated");
    }

    private void OnRuntimeDeactivate()
    {
        Debug.Log("Game View deactivated");
    }
}
```

##IOC

/Battlehub/RTEditor/Runtime/RTCommon/__IOC__ is a simple IoC container implementation.

Methods:

  * `static T Resolve<T>()` - resolve dependency of type 'T'
  * `static void Register<T>(Func<T> func)` - register construction function.
  * `static void Register<T>(T instance)` - register instance.
  * `static void Unregister<T>(Func<T> func)` - unregister construction function.
  * `static void Register<T>(T instance)` - unregister instance.

Example 1:

``` C#
using UnityEngine;
using Battlehub.RTCommon;

public interface IDependency { }

public class Dependency : MonoBehaviour, IDependency
{
    void Awake()
    {
        IOC.Register<IDependency>(this);
    }

    void OnDestroy()
    {
        IOC.Unregister<IDependency>(this);
    }
}

public class User : MonoBehaviour
{
    void Start()
    {
        IDependency dependency = IOC.Resolve<IDependency>();
    }
}
	
```

Example 2:

``` C#
using UnityEngine;
using Battlehub.RTCommon;

[DefaultExecutionOrder(-1)]
public class Registrar : MonoBehaviour
{
	void Awake()
	{
		IOC.Register<IDependency>(() =>
		{
			GameObject go = new GameObject();
			return go.AddComponent<Dependency>();
		});
	}

    private void OnDestroy()
    {
		IOC.Unregister<IDependency>();
    }
}

public interface IDependency { }
public class Dependency : MonoBehaviour, IDependency
{
}

public class User : MonoBehaviour
{
	void Awake()
	{
		IDependency dependency = IOC.Resolve<IDependency>();
	}
}
	
```

##IRTE interface

Core editor functions and APIs are accessible through IRTE interface. This interface defined in /Battlehub/RTEditor/Runtime/RTCommon/__RTEBase.cs__
	
Properties:

  * `CameraLayerSettings CameraLayerSettings { get; }` - layers used by [RTE](rendering.md#graphics-layer-camera).
  * `IInput Input { get; }` - low level [input](#input).
  * `IRuntimeSelection Selection { get; }` - [selection](#runtime-selection) functionality.
  * `IRuntimeUndo Undo { get; }` - [undo redo](#runtime-undo) functionality.
  * `RuntimeTools Tools { get; }` - [tools](#runtime-tools) functionality.
  * `CursorHelper CursorHelper { get; }` - cursor helper.
  * `IRuntimeObjects Object { get; }` - [object](#runtime-objects) lifecycle events.
  * `IDragDrop DragDrop { get; }` - [drag and drop](#drag-and-drop) functions.
  * `bool IsOpened { get; set; }` - is editor opened? 
  * `bool IsBusy { get; set; }` - is editor busy? blocks user input.
  * `bool IsPlaymodeStateChanging { get; }` - is editor either in play mode, or about to switch to it? 
  * `bool IsPlaying { get; set; }` - is editor currently in play mode?
  * `bool IsApplicationPaused { get; }` - is application paused?
  * `Transform Root { get; }` -  editor root transform.
  * `RuntimeWindow ActiveWindow { get; }` - currently active [window](#runtime-window). 
  * `RuntimeWindow[] Windows { get; }` - all registered windows.
        
Methods:

  * `void ActivateWindow(RuntimeWindow window);` - active window and associated [RTE Components](#rte-component) will receive user input.
  
  * `void RegisterCreatedObjects(GameObject[] go);` - register created game objects to undo stack.
  * `void Duplicate(GameObject[] go);` - duplicate objects and register created duplicates to undo stack.
  * `void Delete(GameObject[] go);` - delete game objects and register this operation to undo stack.

Events:

  * `RTEEvent PlaymodeStateChanging` - raised before play mode change.
  * `RTEEvent PlaymodeStateChanged` - raised when play mode state changed.
  * `RTEEvent ActiveWindowChanged` - raised when window activated.
  * `RTEEvent<RuntimeWindow> WindowRegistered` - raised when window registered.
  * `RTEEvent<RuntimeWindow> WindowUnregistered` - raised when window unregistered.
  * `RTEEvent IsOpenedChanged` - raised when IsOpened property changed.

Here is how to get reference to __IRTE__:
``` C#
 using UnityEngine;
 using Battlehub.RTCommon;
 
 public class GetIRTE : MonoBehaviour
 {
	void Awake()
	{
		IRTE editor = IOC.Resolve<IRTE>();
	}
 }
	
```

Open example:
``` C#
 using UnityEngine;
 using Battlehub.RTCommon;
 
 public class OpenExample : MonoBehaviour
 {
	void Start()
	{
		IRTE rte = IOC.Resolve<IRTE>();
		rte.IsOpened = true;
	}
 }
	
```

Play example:
``` C#
 using UnityEngine;
 using Battlehub.RTCommon;
 
 public class PlayExample : MonoBehaviour
 {
	void Start()
	{
		IRTE rte = IOC.Resolve<IRTE>();
		rte.IsPlaying = true;
	}
 }
	
```

Lock user input example:

``` C#
 using UnityEngine;
 using System.Collections;
 using Battlehub.RTCommon;
 
 public class BlockUserInput : MonoBehaviour
 {
	IRTE m_rte;
	
	void Start()
	{
		m_rte = IOC.Resolve<IRTE>();
		StartCoroutine(DummyOperation());
	}
	
	IEnumerator DummyOperation()
    {
		yield return new WaitWhile(() => m_rte.IsBusy);

		m_rte.IsBusy = true;
		Debug.Log("User input locked");

		yield return new WaitForSeconds(5);

		m_rte.IsBusy = false;
		Debug.Log("User input unlocked");
	}
 }
	
```

##Input

[Runtime editor](runtime-editor.md), [transform handles](transform-handles.md) and [gizmos](gizmos.md) are using IInput interface to access user input.
Here is how to get reference to IInput:
``` C#
 using UnityEngine;
 using Battlehub.RTCommon;
 
 public class GetIRTE : MonoBehaviour
 {
	void Awake()
	{
		IInput input = IOC.Resolve<IRTE>().Input;
	}
 }
	
```

IInput has following methods:

 * `bool IsAnyKeyDown()`
 * `float GetAxis(InputAxis axisType)` 
 * `bool GetKeyDown(KeyCode key)`
 * `bool GetKeyUp(KeyCode key)`
 * `bool GetKey(KeyCode key)`        
 * `Vector3 GetPointerXY(int pointer)`
 * `bool GetPointerDown(int button)`
 * `bool GetPointerUp(int button)`        
 * `bool GetPointer(int button)`
        

##RTE Component

/Battlehub/RTEditor/Runtime/RTCommon/__RTEComponent__ is the base class for all [transform handles](transform-handles.md) and [gizmos](gizmos.md).
It hold reference to [runtime window](#runtime-window) and check if this window is in active state. 

##Runtime Selection
__IRuntimeSelection__ interface defined in Assets/Battlehub/RTCommon/Scripts/__RuntimeSelection.cs__. Here is how subscribe to `SelectionChanged` event and select object:

``` C#
using UnityEngine;
using Battlehub.RTCommon;

public class SelectObjectBehaviour : MonoBehaviour
{
	IRuntimeSelection m_selection;

	void Start()
	{
		m_selection = IOC.Resolve<IRTE>().Selection;
		m_selection.SelectionChanged += OnSelectionChanged;

		GameObject go = GameObject.CreatePrimitive(PrimitiveType.Capsule);
		go.AddComponent<ExposeToEditor>();

		m_selection.activeObject = go;
	}

	void OnDestroy()
	{
		if (m_selection != null)
		{
			m_selection.SelectionChanged -= OnSelectionChanged;
		}
	}

	void OnSelectionChanged(Object[] unselectedObjects)
	{
		if (unselectedObjects != null)
		{
			for (int i = 0; i < unselectedObjects.Length; ++i)
			{
				Object unselected = unselectedObjects[i];
				Debug.Log(unselected);
			}
		}

		if (m_selection.objects != null)
		{
			for (int i = 0; i < m_selection.objects.Length; ++i)
			{
				Object selected = m_selection.objects[i];
				Debug.Log(selected);
			}
		}
	}
}
	
```

##Runtime Objects
__IRuntimeObjects__ interface defined in /Battlehub/RTEditor/Runtime/RTCommon/__RuntimeObjects.cs__. It can be used to track lifecycle events of objects exposed to editor.

Methods:
 
 * `IEnumerable<ExposeToEditor> Get(bool rootsOnly, bool useCache = true)` - call this method to get exposed to editor objects. If `rootsOnly` parameter set to _false_ it return all objects, otherwise it return objects without parent.
	If `useCache` parameter set to _false_, then full traversal of object tree will be used.
	
Events:

 * `event ObjectEvent Awaked` - see corresponding [MonoBehaviour event](https://docs.unity3d.com/Manual/ExecutionOrder.html).
 * `event ObjectEvent Started` - see corresponding [MonoBehaviour event](https://docs.unity3d.com/Manual/ExecutionOrder.html).
 * `event ObjectEvent Enabled` - see corresponding[MonoBehaviour event](https://docs.unity3d.com/Manual/ExecutionOrder.html).
 * `event ObjectEvent Disabled` - see corresponding [MonoBehaviour event](https://docs.unity3d.com/Manual/ExecutionOrder.html).
 * `event ObjectEvent Destroyed` - see corresponding [MonoBehaviour event](https://docs.unity3d.com/Manual/ExecutionOrder.html).
 * `event ObjectEvent Destroying` - raised just before `Destroyed` event. 
 * `event ObjectEvent MarkAsDestroyedChanged` - raised when object marked as destroyed by [undo&redo](#runtime-undo)    
 * `event ObjectEvent MarkAsDestroyedChanging` - raised just before `MarkAsDestroyedChanged` event.
 * `event ObjectEvent TransformChanged` - position, rotation or localScale changed.
 * `event ObjectParentChangedEvent ParentChanged` - transform.parent changed.
 * `event ObjectEvent NameChanged` - object name changed.
 * `event ObjectEvent<Component> ComponentAdded` - raised when new component added.




Example:
	
``` C#
using UnityEngine;
using Battlehub.RTCommon;

public class ListenAwakeEvent : MonoBehaviour
{
	IRuntimeObjects m_object;

	void Start()
	{
		m_object = IOC.Resolve<IRTE>().Object;
		m_object.Awaked += OnObjectAwaked;

		GameObject go = new GameObject();
		go.AddComponent<ExposeToEditor>();
	}

	void OnDestroy()
	{
		if(m_object != null)
		{
			m_object.Awaked -= OnObjectAwaked;
		}
	}

	void OnObjectAwaked(ExposeToEditor obj)
	{
		Debug.Log(obj);
	}
}

```

##Runtime Tools

__RuntimeTools__ class can be found in /Battlehub/RTEditor/Runtime/RTCommon/__RuntimeObjects.cs__. 

Properties:

 * `LockObject LockAxes` - this object hold aggregated [locking state](transform-handles.md#locking-axes) of selected objects.
 * `UnityObject ActiveTool` - reference to active transform handle or gizmo. Active means that user is currently interacting with it.
 * `RuntimeTool Current` - The tool that is currently selected for the Scene View:
	* _None_
    * _Move_
    * _Rotate_
    * _Scale_
    * _View_
	* _Rect_
	* _Custom_	
	&nbsp;
 * `RuntimePivotMode PivotMode` - Are we in _Center_ or _Pivot_ mode.
 * `RuntimePivotRotation PivotRotation` - What's the rotation of the tool handle:
	* _Global_
    * _Local_
	&nbsp;
 
 
If property change, corresponding event is raised:

 * `event RuntimeToolsEvent ToolChanged`
 * `event RuntimeToolsEvent PivotRotationChanged`
 * `event RuntimeToolsEvent PivotModeChanged`
 * `event RuntimeToolsEvent LockAxesChanged`
	
Here is how to change current tool:

``` C#
using UnityEngine;
using Battlehub.RTCommon;
 
public class SwitchToolBehaviour : MonoBehaviour
{
	void Start()
	{
        IRTE editor = IOC.Resolve<IRTE>();
        editor.Tools.Current = RuntimeTool.Rotate;
    }
}
	
```

Here is how to [lock axes](transform-handles.md#locking-axes) for all selected objects:

``` C#
using UnityEngine;
using Battlehub.RTCommon;
 
public class LockAxesForAllObjects : MonoBehaviour
{
	IRTE m_editor;
	void Start()
    {
		m_editor = IOC.Resolve<IRTE>();
		m_editor.Selection.SelectionChanged += OnSelectionChanged;
		m_editor.Tools.ToolChanged += OnToolChanged;
	}

	void OnDestroy()
	{
		if(m_editor != null)
		{
			m_editor.Selection.SelectionChanged -= OnSelectionChanged;
			m_editor.Tools.ToolChanged -= OnToolChanged;
		}
	}

	void OnToolChanged()
	{
		Lock();
	}

	void OnSelectionChanged(Object[] unselectedObjects)
	{
		Lock();
	}

	static void Lock()
	{
		IRTE editor = IOC.Resolve<IRTE>();
		editor.Tools.LockAxes = new LockObject
		{
			PositionY = true,
			RotationX = true,
			RotationZ = true
		};
	}
}
	
```

##Runtime Undo

__IRuntimeUndo__ interface defined in /Battlehub/RTEditor/Runtime/RTCommon/__RuntimeUndo.cs__. It is used to record changes, maintain undo/redo stack and perform
undo and redo operations.

Properties:
	
 * `bool Enabled { get; set; }` - is undo & redo enabled?
 * `bool CanUndo { get; }` - can undo?
 * `bool CanRedo { get; }` - can redo?
 * `bool IsRecording { get; }` - are we recording multiple changes?
 
Methods:

 * `void BeginRecord()` - begin record multiple changes.
 * `void EndRecord()` - end record multiple changes.
 * `Record CreateRecord(UndoRedoCallback redoCallback, UndoRedoCallback undoCallback, PurgeCallback purgeCallback = null, EraseReferenceCallback eraseCallback = null)`
		\- generic create record method.
		
 * `void RegisterCreatedObjects(ExposeToEditor[] createdObjects)` - register created objects.
 * `void DestroyObjects(ExposeToEditor[] destoryedObjects)` - register destroy objects operation.
 * `void BeginRecordValue(object target, MemberInfo memberInfo)` - begin record value.
 * `void EndRecordValue(object target, MemberInfo memberInfo)` - end record value.
 * `void BeginRecordTransform(Transform target)` - begin record transform.
 * `void EndRecordTransform(Transform target)` - end record transform.
 * `void AddComponent(ExposeToEditor obj, Type type)`  
      \- add component and push corresponding record to the stack.
 * `void DestroyComponent(Component destroy, MemberInfo[] memberInfo)`  
      \- destroy component and push corresponding record to the stack.
 
 * `void Redo()` - redo.
 * `void Undo()` - undo.
 * `void Purge()` - purge all records. All “marked as destroyed” objects will be destroyed.
 * `void Erase(object oldRef, object newRef)` - replace oldRef with newRef for all records in stack.
 * `void Store()` - create new stack and store current undo&redo stack.
 * `void Restore()` - restore previously stored stack.

Events:

 * `event RuntimeUndoEventHandler BeforeUndo` - raised before undo operation.
 * `event RuntimeUndoEventHandler UndoCompleted` - raised after undo operation.
 * `event RuntimeUndoEventHandler BeforeRedo` - raised before redo operation.
 * `event RuntimeUndoEventHandler RedoCompleted` - raised after redo operation.
 * `event RuntimeUndoEventHandler StateChanged` - raised whenever one of the following operations performed: 
	* _Store_
    * _Restore_ 
	* _Purge_


Here is how to record value and then undo changes:

``` C#
using UnityEngine;
using System.Reflection;
using Battlehub.RTCommon;
using Battlehub.Utils;

public class RecordValueThenUndoChanges : MonoBehaviour
{
	IRuntimeUndo m_undo;

	[SerializeField]
	int m_value = 1;

	void Start()
	{
		m_undo = IOC.Resolve<IRTE>().Undo;
		m_undo.UndoCompleted += OnUndoCompleted;

		MemberInfo valueInfo = Strong.MemberInfo((RecordValueThenUndoChanges x) => x.m_value);

		m_undo.BeginRecordValue(this, valueInfo);
		m_value = 2;
		m_undo.EndRecordValue(this, valueInfo);

		m_undo.Undo();
	}

	void OnDestroy()
	{
		if (m_undo != null)
		{
			m_undo.UndoCompleted -= OnUndoCompleted;
		}
	}

	void OnUndoCompleted()
	{
		Debug.Log(m_value); //1
	}
}
```
	
##Drag And Drop
__IDragDrop__ interface defined in /Battlehub/RTEditor/Runtime/RTCommon/__DragDrop.cs__. It is used as a common interface for all drag & drop operations.

Properties:
 
 * `object[] DragObjects { get; }` - objects being dragged.
 * `object Source { get; }` - drag & drop operation source object.
 * `bool InProgress { get; }` - is drag and drop in progress?
 
Methods:

 * `void Reset()` - cancel current drag & drop operation.
 * `void SetCursor(KnownCursor cursorType)` - set cursor. 
	* _KnownCursor.DropNotAllowed_
    * _KnownCursor.DropAllowed_
 * `void RaiseBeginDrag(object source, object[] dragItems, PointerEventData pointerEventData)`   
 \- begin drag & drop.
 * `void RaiseDrag(PointerEventData eventData)` - drag.
 * `void RaiseDrop(PointerEventData pointerEventData)` - end drag & drop.

Events:

 * `event DragDropEventHander BeginDrag` - raised by `RaiseBeginDrag` method.
 * `event DragDropEventHander Drag` - raised by `RaiseDrag` method.
 * `event DragDropEventHander Drop` - raised by `RaiseDrop` method.
 
 
In this example we will handle Drag Drop operation into console window:
``` C#
using Battlehub.RTCommon;
using UnityEngine;
using UnityEngine.EventSystems;

public class ConsoleDragDropHandler : MonoBehaviour
{
    IDragDrop m_dragDrop;
    RuntimeWindow m_target;
        
    void Start()
    {
        m_target = IOC.Resolve<IRTE>().GetWindow(RuntimeWindowType.Console);            
        m_dragDrop = IOC.Resolve<IRTE>().DragDrop;
        m_dragDrop.Drop += OnDrop;
    }

    void OnDestroy()
    {
		if(m_dragDrop != null)
		{
			m_dragDrop.Drop -= OnDrop;
		}
    }

    void OnDrop(PointerEventData pointerEventData)
	{
        if(m_target != null && m_target.IsPointerOver)
        {
            Debug.Log(m_dragDrop.DragObjects[0]);
        }
    }
}
	
```
		
##Runtime Window

__RuntimeWindow__ class can be found in /Battlehub/RTEditor/Runtime/RTCommon/__RuntimeWindow.cs__.  If you want to extend [Runtime Editor](runtime-editor.md) with new windows, then they must be inherited from  
Runtime Window class. Here is how to do it:

``` C#

using Battlehub.RTCommon;
using Battlehub.Utils;
using UnityEngine;
using UnityEngine.EventSystems;

public class CustomWindow : RuntimeWindow
{
    protected override void AwakeOverride()
    {
        WindowType = RuntimeWindowType.Custom;
        base.AwakeOverride();
    }

    protected override void OnDestroyOverride()
    {
        base.OnDestroyOverride();
    }

    protected override void OnActivated()
    {
        base.OnActivated();
        Debug.Log("On Custom Window Activated");
    }

    protected override void OnDeactivated()
    {
        base.OnDeactivated();
        Debug.Log("On Custom Window Deactivated");
    }

    public override void DragEnter(object[] dragObjects, PointerEventData eventData)
    {
        base.DragEnter(dragObjects, eventData);
        Editor.DragDrop.SetCursor(KnownCursor.DropAllowed);
		Debug.Log("On Drag Enter");
    }

    public override void DragLeave(PointerEventData eventData)
    {
        base.DragLeave(eventData);
        Editor.DragDrop.SetCursor(KnownCursor.DropNotAllowed);
		Debug.Log("On Drag Leave");
    }

    public override void Drop(object[] dragObjects, PointerEventData eventData)
    {
        base.Drop(dragObjects, eventData);
		Debug.Log("On Drop");
		
        for(int i = 0; i < dragObjects.Length; ++i)
        {
            Debug.Log(dragObjects[i]);
        }
		
    }
}

```

> [!NOTE]
> For information on how to register custom window please proceed to -> [this](runtime-editor.md#how-to-add-custom-window-to-window-manager) <- section
     

