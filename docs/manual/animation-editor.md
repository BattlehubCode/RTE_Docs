#Animation Editor Docs
##Overview
The Animation Editor adds to the Runtime Editor the ability to create and edit animations.

![Screenshot](~/resources/img/animation-editor/animation-editor.png)

Animation Editor Prefabs can be found in __Assets\Battlehub\RTEditor\Content\Runtime\RTEditor\Prefabs__.


This is example on how to create RuntimeAnimation:

``` C#

using Battlehub.RTCommon;
using Battlehub.UIControls.MenuControl;
using System.Collections.Generic;
using UnityEngine;

namespace Battlehub.RTEditor.Examples.Scene90
{
    /*
        Property names can be looked up by setting Asset Serialization to Force Text mode 
		in the Editor settings.  The text files that are then written by the editor will 
		include the names of the properties. 
        For example, the yaml file written for a Scene object will include the Camera settings.
        Looking at this yaml file will show:
        m_BackGroundColor: {r: .192156866, g: .301960796, b: .474509805, a: .0196078438}
        m_NormalizedViewPortRect:
        serializedVersion: 2
        x: 0
        y: 0
        width: 1
        height: 1
        near clip plane: .300000012
        far clip plane: 1000
        field of view: 60
        orthographic: 0
        orthographic size: 5
        m_Depth: -1
        This shows that the name for the FOV parameter is "field of view". 
        If you wanted to create an animation clip to animate the camera field of view, 
        you would pass "field of view" as the propertyName.
     */

    [MenuDefinition]
    public class AnimationEditorExampleMenu : MonoBehaviour
    {
        [MenuCommand("Example/Create Runtime Animation")]
        public void CreateRuntimeAnimation()
        {
            GameObject go = GameObject.CreatePrimitive(PrimitiveType.Cube);
            go.AddComponent<ExposeToEditor>();

            RuntimeAnimationClip clip =
				ScriptableObject.CreateInstance<RuntimeAnimationClip>();

            RuntimeAnimationProperty property = new RuntimeAnimationProperty();
            property.Component = go.GetComponent<Transform>();
            property.ComponentTypeName = "UnityEngine.Transform, UnityEngine.CoreModule";
            property.ComponentDisplayName = "Transform";
            property.AnimationPropertyName = "m_LocalPosition";
            property.PropertyDisplayName = "Position";
            property.PropertyName = "localPosition";

            property.Children = new List<RuntimeAnimationProperty>
            {
                new RuntimeAnimationProperty
                {
                    Parent = property,
                    Component = property.Component,
                    ComponentTypeName = property.ComponentTypeName,
                    PropertyDisplayName = "x",
                    PropertyName = "x",
                    Curve = new AnimationCurve
                    {
                        keys = new[]
                        {
                            new Keyframe(0, go.transform.position.x),
                            new Keyframe(0.5f, go.transform.position.x),
                            new Keyframe(1, go.transform.position.x),
                        }
                    }
                },
                new RuntimeAnimationProperty
                {
                    Parent = property,
                    Component = property.Component,
                    ComponentTypeName = property.ComponentTypeName,
                    PropertyDisplayName = "y",
                    PropertyName = "y",
                    Curve = new AnimationCurve
                    {
                        keys = new[]
                        {
                            new Keyframe(0, go.transform.position.y),
                            new Keyframe(0.5f, go.transform.position.y + 3.0f),
                            new Keyframe(1, go.transform.position.y),
                        }
                    }
                },
                new RuntimeAnimationProperty
                {
                    Parent = property,
                    Component = property.Component,
                    ComponentTypeName = property.ComponentTypeName,
                    PropertyDisplayName = "z",
                    PropertyName = "z",
                    Curve = new AnimationCurve
                    {
                        keys = new[]
                        {
                            new Keyframe(0, go.transform.position.z),
                            new Keyframe(0.5f, go.transform.position.z),
                            new Keyframe(1, go.transform.position.z),
                        }
                    }
                }
            };

            clip.Add(property);

            RuntimeAnimation animation = go.AddComponent<RuntimeAnimation>();
            animation.Clips = new List<RuntimeAnimationClip>
            {
                clip
            };

            IRTE rte = IOC.Resolve<IRTE>();
            rte.Selection.activeGameObject = go;
        }
    }
}
```