AnimationBase
=============

Extends [**Node**](/docs/references/scenegraph/node.md "**Node**")

AnimationBase is an abstract node class that contains the fields common to the [Animation](/docs/references/scenegraph/animation-nodes/animation.md "Animation"), [SequentialAnimation](/docs/references/scenegraph/animation-nodes/sequentialanimation.md "SequentialAnimation"), and [ParallelAnimation](/docs/references/scenegraph/animation-nodes/parallelanimation.md "ParallelAnimation") nodes. The purpose of the AnimationBase node class is to provide the basic functionality needed to animate screen elements, such as moving them across the display screen, fading them in and out of view, or changing their color. All node classes extended from AnimationBase require the use of the interpolator node classes [FloatFieldInterpolator](/docs/references/scenegraph/animation-nodes/floatfieldinterpolator.md "FloatFieldInterpolator"), [Vector2DFieldInterpolator](/docs/references/scenegraph/animation-nodes/vector2dfieldinterpolator.md "Vector2DFieldInterpolator"), and [ColorFieldInterpolator](/docs/references/scenegraph/animation-nodes/colorfieldinterpolator.md "ColorFieldInterpolator") as child nodes to achieve a specific animation effect.

> AnimationBase is not meant to be instantiated directly by app code

Fields
------

FieldTypeDefaultAccess PermissionDescriptioncontroloption stringnoneREAD\_WRITEControls the animation. Supported options include:

| Option | Effect |
| --- | --- |
| none | Initial state with no associated action |
| start | Always plays the animation from the beginning |
| stop | Stops the animation in its current state |
| pause | Pauses the animation in its current state |
| resume | If paused, resumes the animation from its current state. If the animation is not paused, plays the animation from the beginning. |
| finish | Jumps to the end of the animation, then stops. All animated fields will be immediately set to their final values as if the animation had completed. |

statevalue stringstoppedREAD\_ONLYIndicates the state of the animation. Values include:

| Value | Meaning |
| --- | --- |
| running | Indicates that the animation is in progress |
| paused | Indicates that the animation has been paused |
| stopped | Indicates that the animation has either run to completion or has been explicitly stopped |

repeatBooleanfalseREAD\_WRITEControls whether the animation stops when it finishes (false) or repeats from the beginning (true)delaytime0READ\_WRITEDelays the start of the animation by the specified number of seconds