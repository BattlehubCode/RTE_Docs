#Menu Control Docs
##Overview
The [Menu control](#menu) allows hierarchal organization of elements associated with commands. It can be used to implement main and context menu of an application.

![Screenshot](~/resources/img/menu/overview/menu.png)    

##Getting Started

1. Create Canvas
2. Add __MenuButton.prefab__ from __/Battlehub/RTEditor/Content/Runtime/UIControls/Menu/<br> Prefabs__ to hierarchy.
3. Add __Menu.prefab__ from __/Battlehub/RTEditor/Content/Runtime/UIControls/Menu/Prefabs__ to hierarchy
4. Set `Menu` field of the Menu Button:

	![Screenshot](~/resources/img/menu/get-started/set-menu-field.png)   
	<br/>
	
5. Create Empty Game Object and name it __Command Handler__
6. Create __MenuCmdHandler__ script and add it to the __Command Handler__:

		using UnityEngine;
		using Battlehub.UIControls.MenuControl;

		public class MenuCmdHandler : MonoBehaviour
		{
			public void OnValidateCmd(MenuItemValidationArgs args)
			{
				Debug.Log("Validate Command: " + args.Command);

				if(args.Command == "Item Cmd")
				{
					args.IsValid = false;
				}
			}

			public void OnCmd(string cmd)
			{
				Debug.Log("Run Cmd: " + cmd);
			}
		}

		

7. Set `Action` and `Validate` event handlers of each entry in `Items` array of the Menu:
	
	![Screenshot](~/resources/img/menu/get-started/set-event-handlers.png)  
	<br/>	           
	
8. Hit Play, open the Menu and click _Child Item_. You should see following output:
	
	![Screenshot](~/resources/img/menu/get-started/results.png)      

##Menu Item
Structure:

![Screenshot](~/resources/img/menu/menu-item/structure.png)   

Prefab: /Battlehub/RTEditor/Content/Runtime/UIControls/Menu/Prefabs/__MenuItem.prefab__  
Script: /Battlehub/RTEditor/Runtime/UIControls/Menu/__MenuItem.cs__

Fields:

 * `Selection Color` - highlighted color.
 * `Text Color` - text color.
 * `Disabled Selection Color` - highlighted color of disabled menu item.
 * `Disabled Text Color` - text color of disabled menu item.
 * `Icon` - image.
 * `Text` - text.
 * `Expander` - image visible if menu item has sub-menu.
 * `Selection` - image visible when pointer is over menu item.

##Menu

Structure:

![Screenshot](~/resources/img/menu/menu/structure.png)   


Prefab: /Battlehub/RTEditor/Content/Runtime/UIControls/Menu/Prefabs/__Menu.prefab__  
Script: /Battlehub/RTEditor/Runtime/UIControls/Menu/__Menu.cs__

Fields:

  * `Items` - array of Menu Item Info.
  * `Menu Item Prefab` - reference to [menu item](#menu-item) prefab.
  * `Anchor` - opened menu will be aligned with anchor.
  * `Panel` - parent of menu items.
  * `Canvas Group` - required to "fade-in" menu.
  * `Fade in speed` - how much the alpha property changes per second.
  
  

```C#  
[Serializable]
public class MenuItemInfo
{
	public string Path; //path to menu item e.g. Create/3DObject/Cube
	public string Text; //menu item text 
	public Sprite Icon; //menu item icon

	public string Command; //argument passed to Action event handler and Validate event handler
	public MenuItemEvent Action; //raised when menu item clicked
	public MenuItemValidationEvent Validate; //event raised when menu opened
}
```

##Main Menu Button

This button can be used to open menu. It has three states: normal, pointer over and focused.  The transition to the focused state occurs when the menu is opened. The transition to the normal state occurs when the menu is closed. 

Prefab: /Battlehub/RTEditor/Content/Runtime/UIControls/Menu/Prefabs/__MenuButton.prefab__  
Script: /Battlehub/RTEditor/Runtime/UIControls/Menu/__MenuButton.cs__

##Context Menu Trigger

This script opens the menu when you click the right mouse button.  

![Screenshot](~/resources/img/menu/context-menu-trigger/context-menu.png)   

Script: Assets/Battelhub/UIControls/Menu/Scripts/__СontextMenuTrigger.cs__

![Screenshot](~/resources/img/menu/context-menu-trigger/context-menu-trigger.png)   





