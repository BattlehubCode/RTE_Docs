#Dock Panel Docs
##Overview

The [__Dock Panel__](#dock-panel) is a control, that provides an easy docking of content [regions](#region) to the left, right, top, bottom or center of the panel. The control also allow region to become an independent floating window, modal popup or dialog.

![Screenshot](~/resources/img/dock-panels/overview/dock-panels.png)

##Getting Started

  1. Create Canvas.
  2. Add __DockPanel.prefab__ from __Assets/Battlehub/RTEditor/Content/Runtime/UIControls/DockPanels/Prefabs__ to hierarchy.
  3. Create __GettingStarted.cs script__ and add it to __DockPanel__ Game Object.
  4. Hit Play.
  
![Screenshot](~/resources/img/dock-panels/get-started/create-dock-panel.png)  
	
``` C#
using Battlehub.UIControls.DockPanels;
using UnityEngine;
using UnityEngine.UI;

public class GettingStarted : MonoBehaviour
{
	DockPanel m_dockPanel;

	void Start()
	{
		m_dockPanel = GetComponent<DockPanel>();

		GameObject testContent = new GameObject();
		testContent.AddComponent<Image>().color = new Color(0.2f, 0.2f, 0.2f, 1.0f);

        LayoutInfo layout = new LayoutInfo(false,
			new LayoutInfo(testContent.transform, "Tab 1"),
				new LayoutInfo(
					isVertical: true,
						child0: new LayoutInfo(Instantiate(testContent).transform, "Tab 2"),
						child1: new LayoutInfo(Instantiate(testContent).transform, "Tab 3"),
						ratio: 0.5f),
				0.5f);

        m_dockPanel.RootRegion.Build(layout);
    }              
}

```

![Screenshot](~/resources/img/dock-panels/get-started/get-started-result.png)


> [!NOTE]
> Full demo can be found in  __Assets/Battlehub/RTEditorDemo/Content/Runtime/UIControls/DockPanels__ folder

##Dock Panel

Structure:
![Screenshot](~/resources/img/dock-panels/dock-panel/structure.png)


Prefab: /Battlehub/RTEditor/Content/Runtime/UIControls/DockPanels/Prefabs/__DockPanel.prefab__   
Script: /Battlehub/RTEditor/Runtime/UIControls/DockPanels/__DockPanel.cs__   

Fields:

  * `Mask` - masking everything outside of DockPanel (image with Rect2D Component)
  * `Docked` - parent for docked [regions](#region)
  * `Free` - parent for floating [regions](#region)
  * `Preview` - parent for tab preview visible during [tab](#tab) re-parenting operation.
  * `Modal` - parent for modal pop-ups and dialogs.
  * `Region Prefab` - reference to [region](#region) prefab.
  * `Tab Prefab` - reference to default [tab](#tab) prefab.
  
##Tab 

Default Style:  
![Screenshot](~/resources/img/dock-panels/tab/tab.png)

Structure:
![Screenshot](~/resources/img/dock-panels/tab/structure.png)

Prefab: /Battlehub/RTEditor/Content/Runtime/UIControls/DockPanels/Prefabs/__Tab.prefab__   
Script: /Battlehub/RTEditor/Runtime/UIControls/DockPanels/__Tab.cs__   

Fields:

   * `Canvas Group` - required to hide tab but preserve occupied space during rearrangements.
   * `Img` - tab icon.
   * `Text` - tab header text.
   * `Close Button` - tab close button.
   * `Toggle` - reference to toggle control (tab have 2 states 'selected' and 'unselected').

##Region

Default Style:  
![Screenshot](~/resources/img/dock-panels/region/region.png)

Structure:
![Screenshot](~/resources/img/dock-panels/region/structure.png)

Prefab: /Battlehub/RTEditor/Content/Runtime/UIControls/DockPanels/Prefabs/__Region.prefab__   
Script: /Battlehub/RTEditor/Runtime/UIControls/DockPanels/__Region.cs__   

Fields:

   * `Maximize Toggle` - expands owner region and hide others.
   * `Can Resize` - enables/disable ability to resize region.
   * `Layout Element` - reference to root Layout Element.
   * `Tab Panel` - panel for [tabs](#tab).
   * `Content Panel` - panel for content.
   * `Children Panel` - panel for child regions.
   * `Root` - reference to [dock panel](#dock-panel).
   * `Header Image` - region header graphics.
   * `Frame Image` - region content frame graphics.
   
Methods:

  
  * `void Build(LayoutInfo layoutInfo)` - build layout.
  
	
		GameObject content1 = new GameObject();
		content1.AddComponent<Image>().color = Color.red;

		GameObject content2 = new GameObject();
		content2.AddComponent<Image>().color = Color.yellow;

		m_region.Build(new LayoutInfo(
			isVertical: false,
			child0: new LayoutInfo(content1.transform, "Header 1"),
			child1: new LayoutInfo(content2.transform, "Header 2"), 
			ratio: 0.25f));

  * `void RemoveAt(int index)` - remove content at index.
  * `void Add(Sprite icon, 
		string header,
		Transform content, 
		bool isFree = false, 
		RegionSplitType splitType = RegionSplitType.None)` - add content to region.
	
		Sprite icon = null;
	    GameObject content = new GameObject();
        content.AddComponent<Image>().color = Color.green;
		
		//1. Add content
		m_region.Add(icon, "Header1", content.transform, false, RegionSplitType.None);
		
		//2. Add content to the left
		m_region.Add(icon, "Header2", content.transform, false, RegionSplitType.Left);
		
		//3. Add content to the right
		m_region.Add(icon, "Header3", content.transform, false, RegionSplitType.Right);
		
		//4. Add content on the top
		m_region.Add(icon, "Header4", content.transform, false, RegionSplitType.Top);
		
		//5. Add content to the bottom
		m_region.Add(icon, "Header5", content.transform, false, RegionSplitType.Bottom);
		
		//6. Add content to floating region
		m_region.Add(icon, "Tab 3", content.transform, true);
        
  
##Dialog Manager

Dialog manager allows you to create various dialogs and pop-ups. Dialogs are created as a modal floating regions of [dock panel](#dock-panel).
![Screenshot](~/resources/img/dock-panels/dialog-manager/dialog-manager.png)

To use dialog manager do following:

 1. Add Assets/Battlehub/RTEditor/Runtime/UIControls/Dialog/__DialogManager__ component.
 2. Set `Dock Panel` field.
 3. Set `Dialog` field to Assets/Battlehub/RTEditor/Content/Runtime/UIControls/Dialog/Prefabs/__Dialog.prefab__.
 
![Screenshot](~/resources/img/dock-panels/dialog-manager/usage.png)

``` C#
using Battlehub.UIControls.Dialogs;
using UnityEngine;
using UnityEngine.UI;

public class DialogManagerExamples : MonoBehaviour
{
	[SerializeField]
	DialogManager m_dialogManager;

	[SerializeField]
	Sprite m_icon;

	void Start()
    {
		//Show message box with "OK" button
		m_dialogManager.ShowDialog(m_icon, "Message Header", "Message Text");

		//Show confirmation with "Yes" and "No" buttons
		m_dialogManager.ShowDialog(m_icon, "Scene has been modified", "Do you want to save changed you made in the scene?",
			(dialog, cancelArgs) => 
			{
				Debug.Log("Yes clicked");
			}, "Yes",
			(dialog, cancelArgs) =>
			{
				Debug.Log("No clicked");
			}, "No");

		//Show popup with custom content
		GameObject content = new GameObject();
		content.AddComponent<Image>().color = Color.red;
		m_dialogManager.ShowDialog(m_icon, "Popup", content.transform);
	}           
}

```