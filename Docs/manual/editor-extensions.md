#Editor Extensions Docs
##Overview
[__Editor Extensions__](#editor-extensions) are additional functionality that can be used optionally with the Runtime Editor. At the moment there are three built-in editor extensions:

1. [Runtime Builder](#runtime-builder) - integration and user interface for [Unity ProBuilder](https://docs.unity3d.com/Packages/com.unity.probuilder@4.2).
2. [Terrain Editor](#terrain-editor) - user interface and additional functionality for [Terrain](https://docs.unity3d.com/Manual/script-Terrain.html) editing
3. [Runtime Scripting](#runtime-scripting) - ability to create and compile [MonoBehavior scripts](https://docs.unity3d.com/ScriptReference/MonoBehaviour.html) at runtime

![Screenshot](~/resources/img/extensions/overview.png)

##Getting Started
To start working with the editor extensions perform the following steps:
 
 1. Create Runtime Editor using Tools->Runtime Editor->__Create Editor__.
 2. Create Editor Extensions using Tools->Runtime Editor->__Create Extensions__.
 ![Screenshot](~/resources/img/extensions/getting-started1.png)
 &nbsp;
 3. Hit Play.
 4. Editor extensions should be available from the Window menu item.
 ![Screenshot](~/resources/img/extensions/getting-started2.png)
 &nbsp;

##Editor Extensions

Editor extensions are located in __Assets/Battlehub/RTExtensions__ folder.
The __/Content__ folder contains resources and prefabs, the __/Runtime__ folder contains runtime scripts, and the __/Editor__ folder contains scripts that run in the Unity editor.
The prefab that initializes all extensions is called __EditorExtensions__ and is located in the __/RTExtensions/Content/Runtime__ folder.

![Screenshot](~/resources/img/extensions/editor-extensions.png)

##Runtime Builder

The __Runtime Builder__ extension allows you to create and edit meshes, prototype, edit game object materials and texture UVs in Runtime Editor using [Unity ProBuilder](https://docs.unity3d.com/Packages/com.unity.probuilder@4.2).

__RTBuilder__ prefab can be found in __/Battlehub/RTExtensions/Content/Runtime/RTBuilder__.

Standalone RTBuilder demo scene can be found in: __/Battlehub/RTEditorDemo/Content/Runtime/RTExtensions/RTBuilder__

![Screenshot](~/resources/img/extensions/rtbuilder/standalone-demo.png)

##Terrain Editor

The __Terrain Editor__ extension allows you to add to Runtime Editor the possibility of editing Terrain objects. 

__RTTerrain__ prefab can be found in __/Battlehub/RTExtensions/Content/Runtime/RTTerrain folder.

![Screenshot](~/resources/img/extensions/rtterrain/terrain-editor.png)

![Screenshot](~/resources/img/extensions/rtterrain/terrain-editor-inspector.png)

##Runtime Scripting
The __Runtime Scripting__ extension allows you to create C# scripts at run-time using the Runtime Editor. 
The __RTScripting__ prefab that initializes the extension can be found at __/Battlehub/RTScripting/Content/Runtime__.

> [!NOTE]
> Unpack __RTScripting__ and __RTScripting Demo__ packages:
>
> ![Screenshot](~/resources/img/extensions/rtscripting/unpack-rtscripting.png)

> [!NOTE]
> Runtime Scripting Extension is __only__ supported in a __Standalone Windows build__
	  
> [!NOTE]
> Runtime Scripting Extension use [Roslyn.net compiler platform](https://github.com/dotnet/roslyn)

After adding the __RTScripting__ prefab to the scene, the __Create->Script__ context menu item becomes available.
![Screenshot](~/resources/img/extensions/rtscripting/create-script.png)

The script editor can be opened using the context menu or by double-clicking the script icon.
![Screenshot](~/resources/img/extensions/rtscripting/edit-script.png)

For programmatic access to the RTScripting extension you need to use the __IRuntimeScriptManager__ interface. Here is how to add references to additional assemblies:
```C#
using Battlehub.RTCommon;
using Battlehub.RTEditor;
using Battlehub.RTScripting;
using UnityEngine.AI;

public class AddReferencesExample : EditorExtension
{
    protected override void OnEditorExist()
    {
        base.OnEditorExist();
		
        IRuntimeScriptManager scriptManager = IOC.Resolve<IRuntimeScriptManager>();
        scriptManager.AddReference(typeof(NavMesh).Assembly.Location);
    }
}

```

Example on how to create and compile script at runtime:

```C#
using Battlehub.RTCommon;
using Battlehub.RTScripting;
using Battlehub.RTSL.Interface;
using Battlehub.UIControls.MenuControl;
using System;
using System.Threading.Tasks;
using UnityEngine;

namespace Battlehub.RTEditor.Examples.Scene60
{
    [MenuDefinition]
    public class RuntimeScriptingExampleMenu : MonoBehaviour
    {
        private static string s_nl = Environment.NewLine;
        private static string s_csTemplate = 
                "using System.Collections;" + s_nl +
                "using System.Collections.Generic;" + s_nl +
                "using UnityEngine;" + s_nl + s_nl +

                "public class {0} : MonoBehaviour" + s_nl +
                "{{" + s_nl +
                "    // Start is called before the first frame update" + s_nl +
                "    void Start()" + s_nl +
                "    {{" + s_nl +
                "       {1}" + s_nl +
                "    }}" + s_nl + s_nl +

                "    // Update is called once per frame" + s_nl +
                "    void Update()" + s_nl +
                "    {{" + s_nl +
                "       {2}" + s_nl +
                "    }}" + s_nl + s_nl +

                "    void OnRuntimeEditorOpened()" + s_nl +
                "    {{" + s_nl +
                "       Debug.Log(\"Editor Opened\");" + s_nl +
                "    }}" + s_nl + s_nl +

                "    void OnRuntimeEditorClosed()" + s_nl +
                "    {{" + s_nl +
                "       Debug.Log(\"Editor Closed\");" + s_nl +
                "    }}" + s_nl + s_nl +

                "    void RuntimeAwake()" + s_nl +
                "    {{" + s_nl +
                "       Debug.Log(\"Awake in play mode\");" + s_nl +
                "    }}" + s_nl + s_nl +

                "    void RuntimeStart()" + s_nl +
                "    {{" + s_nl +
                "       Debug.Log(\"Start in play mode\");" + s_nl +
                "    }}" + s_nl + s_nl +

                "    void OnRuntimeDestroy()" + s_nl +
                "    {{" + s_nl +
                "       Debug.Log(\"Destroy in play mode\");" + s_nl +
                "    }}" + s_nl + s_nl +

                "    void OnRuntimeActivate()" + s_nl +
                "    {{" + s_nl +
                "       Debug.Log(\"Game view activated\");" + s_nl +
                "    }}" + s_nl + s_nl +

                "    void OnRuntimeDeactivate()" + s_nl +
                "    {{" + s_nl +
                "       Debug.Log(\"Game view deactivated\");" + s_nl +
                "    }}" + s_nl + s_nl +

                "}}";

        [MenuCommand("Example/Create Script")]       
        public async void CreateScript()
        {
            IProjectAsync project = IOC.Resolve<IProjectAsync>();
            IRuntimeScriptManager scriptManager = IOC.Resolve<IRuntimeScriptManager>();

            string desiredTypeName = "MyHelloWorld";

            ProjectItem assetItem = 
				await scriptManager.CreateScriptAsync(
					project.State.RootFolder, 
					desiredTypeName);
					
            RuntimeTextAsset cs = await scriptManager.LoadScriptAsync(assetItem);

            string typeName = cs.name;

            cs.Text = 
				string.Format(s_csTemplate, typeName, "Debug.Log(\"Hello World\");", " ", "");
            await scriptManager.SaveScriptAsync(assetItem, cs);
            await scriptManager.CompileAsync();

            GameObject testGo = new GameObject("RT Scripting example");
            testGo.AddComponent<ExposeToEditor>();
            testGo.AddComponent(scriptManager.GetType(typeName));

            await Task.Yield();

            IRTE editor = IOC.Resolve<IRTE>();
            editor.IsPlaying = true;

            await Task.Yield();

            editor.IsPlaying = false;
        }
    }
}
```



