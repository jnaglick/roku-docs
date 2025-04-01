SimpleLabel
===========

Extends [**Group**](/docs/references/scenegraph/layout-group-nodes/group.md "**Group**")

The SimpleLabel node class is used to display a single line of text. SimpleLabel is a lightweight complement to the Label node. It supports simplified font style specification and is more memory efficient than the Label node.

The SimpleLabel node class supports:

*   Specifying either a built-in system font or a TrueType/OpenType font file
*   Specifying the color of the font
*   Customizable Horizontal and Vertical origin

The following shows a text layout derived from the SimpleLabel node:

![roku815px - simpleLabel node](https://image.roku.com/ZHZscHItMTc2/simplelabel.png "simplelabel")

With ui\_resolutions=hd specified in the manifest, the following displays the text string "Application Development Made Easy!" using the medium bold system font, centered horizontally on display, and with the baseline of the text at the vertical center of the display.

    <?xml version="1.0" encoding="utf-8" ?>
    
    <!--********** Copyright 2018 Roku Corp.  All Rights Reserved. **********-->
    
    <component name="simpleLabeltest" extends="Group" >
    
    <script type="text/brightscript" >
    <![CDATA[
    
      sub init()
        m.top.setFocus(true)
      end sub
    
    ]]>
    </script>
    
    <SimpleLabel
      id="testLabel"
      font="fontUri:MediumBoldSystemFont"
      text = "Application Development Made Easy!"
      horizOrigin = "left"
      vertOrigin = "baseline"
      translation="[640,360]" />
    
    </component>
    

### Text Origin

The SimpleLabel node uses the horizOrigin and vertOrigin fields to control the origin of the coordinate system for the node.

#### Horizontal Origin

The horizOrigin field allows controlling the x=0 position of the SimpleLabel node's local coordinate system.

There are three possible values for the horizOrigin field:

*   left The left edge of the text is located at the 0 x-coordinate position of the SimpleLabel node's local coordinate system
    
*   center The horizontal center of the text is positioned at the 0 x-coordinate position of the SimpleLabel node's local coordinate system
    
*   right The right edge of the text is positioned at the 0 x-coordinate position of the SimpleLabel node's local coordinate system
    

#### Vertical Origin

The vertOrigin field allows controlling the y=0 position of the SimpleLabel node's local coordinate system.

There are four possible values for the vertOrigin field:

*   top The top edge of the text is located at the 0 y-coordinate position of the SimpleLabel node's local coordinate system
    
*   center The vertical center of the text is located at the 0 y-coordinate position of the SimpleLabel node's local coordinate system
    
*   baseline The baseline of the text is located at the 0 y-coordinate position of the SimpleLabel node's local coordinate system
    
*   bottom The bottom edge of the text is located at the 0 y-coordinate position of the SimpleLabel node's local coordinate system
    

#### SimpleLabel Origin Example

The following image illustrates the horizontal and vertical origin options supported by the SimpleLabel node. The manifest includes ui\_resolutions=fhd, so all coordinate values are in the range 1920x1080.

*   On the top left, in the image below, a Group node is displayed that has a 1-pixel wide Rectangle node and three SimpleLabel nodes as children. The Group node's x-translation is set to 200, and the x translation of the Rectangle and all three SimpleLabel nodes is set to 0.
    
    *   The Rectangle serves as a visual reference of where x=0 is located in its parent Group's local coordinate system.
    *   The three SimpleLabel nodes illustrate each option for the horizOrigin field (i.e., SimpleLabel horizontal origin at the left edge, center, and right edge of the text).
*   In the bottom of the image, a Group node is displayed that has a 1-pixel tall Rectangle node and four SimpleLabel nodes as children.
    
    *   The Group node's y-translation is set to 480, and the y translation of the Rectangle and all four SimpleLabel nodes is set to 0.
    *   The Rectangle serves as a visual reference of where y=0 is located in its parent Group's local coordinate system.
    *   The four SimpleLabel nodes illustrate each option for the vertOrigin field (i.e., SimpleLabel vertical origin at the top edge, center, baseline, and bottom edge of the text).

![roku815px - simple_label_origin](https://image.roku.com/ZHZscHItMTc2/simpleLabelOriginExample.jpg "simplelabel origin example")

### Rotation

Rotation of SimpleLabel nodes is supported. The center of rotation is determined by the origin point as determined by the horizOrigin and vertOrigin fields. On platforms that do not support OpenGL, only rotations divisible by 90 are supported (e.g., 0, 90, 180, and 270 degrees). For those platforms, other values are rendered using the nearest 90-degree value (e.g., 103 degrees is rendered as a 90-degree rotation, -262 degrees is rendered as a -270 rotation).

Fields
------

FieldTypeDefaultDescriptiontextstring""Specifies the text to be displayedcolorcolor0xddddddffSpecifies the text colorfontUristringsystem defaultSpecifies either a path to a TrueType or OpenType font file or a built-in system font name.  
  
For TrueType or OpenType font files, the file must be included with the application (e.g. `pkg:/fonts/SomeFontFile.ttf`). If no fontUri is specified, the System Default font is used.  
  
The table below shows the options for using built-in system fonts. The "**Fixed Size?"** column indicates whether the `fontSize` field of the **SimpleLabel** is respected or not. For those where the size is fixed, the font size cannot be modified.

| fontUri String | Fixed Size? |
| --- | --- |
| `font:SmallestSystemFont` | Yes |
| `font:SmallSystemFont` | Yes |
| `font:MediumSystemFont` | Yes |
| `font:LargeSystemFont` | Yes |
| `font:SmallestBoldSystemFont` | Yes |
| `font:SmallBoldSystemFont` | Yes |
| `font:MediumBoldSystemFont` | Yes |
| `font:LargeBoldSystemFont` | Yes |
| `font:SystemFontFile` | No  |
| `font:BoldSystemFontFile` | No  |
| System Default (field not set) | Yes |

fontSizeintegersystem defaultSpecifies the size of the font in points. As noted in the description of the `fontUri` field, the use of fixed size system fonts ignores the value of the `fontSize` field.horizOriginstringleftSee [**Horizontal Origin**](/docs/references/scenegraph/renderable-nodes/simplelabel.md#SimpleLabel-HorizontalOrigin)vertOriginstringtopSee [**Vertical Origin**](/docs/references/scenegraph/renderable-nodes/simplelabel.md#SimpleLabel-VerticalOrigin)

The following [Fields](/docs/references/scenegraph/layout-group-nodes/group.md#fields "Fields") derived from the Group base class can also be used:

FieldTypeDefaultAccess PermissionDescriptionscaleRotateCentervector2d\[0.0,0.0\]READ\_WRITEDescribes the location of a point in the node local coordinate that serves as the center of the scale and rotation operationsvisibleBooleantrueREAD\_WRITEIf true, the node and its children are rendered. If false, the node and its children do not renderinheritParentOpacityBooleantrueREAD\_WRITEIf true, the node opacity is determined by multiplying opacity attribute of the node by the opacity of the parent node, which may have been determined by multiplying the opacity of its ancestor nodes. If false, the node opacity is determined by the opacity attribute set for the node or the default opacity attribute valuerotationfloat0.0READ\_WRITEDefines the rotation angle about the scaleRotateCenter point (in radians) of the node local coordinate system. Positive values specify a counterclockwise rotation, negative values specify a clockwise rotation. For some Roku Player hardware, specifically Roku Players without OpenGL graphics support, only rotations of 0, 90, 180 and 270 degrees (in equivalent radians) are supported. (See [Roku Models and Features](/docs/specs/hardware.md#current-models "Roku Models and Features") for information on OpenGL support)scalevector2d\[1.0,1.0\]READ\_WRITEDefines the scale factor to be applied to the node local coordinaterenderTrackingoption as stringdisabledREAD\_WRITErenderTracking is set to "disabled" when enableRenderTracking is set to false. The following options are only available when enableRenderTracking is set to true:

| Option | Description |
| --- | --- |
| `"none"` | renderTracking is set to `"none"` if **one or more** of these conditions is true:  <br>the node's `visible` field is set to `false`the node's `opacity` field is set to `0.0`no `clippingRect` is specified and the node is completely offscreena `clippingRect` is specified and the node lies completely outside that `clippingRect's` coordinates or is completely offscreen |
| `"partial"` | renderTracking is set to `"partial"` if **all** of the following conditions are true:  <br>the node's `visible` field is set to `true`the node's `opacity` field is greater than `0.0`no `clippingRect` is specified and the node is partially offscreena `clippingRect` is specified and the node lies partially inside the `clippingRect's` coordinates |
| `"full"` | renderTracking is set to `"full"` if **all** of the following conditions are true:  <br>the node's `visible` field is set to `true`the node's `opacity` field is greater than `0.0`no `clippingRect` is specified and the node is completely onscreena `clippingRect` is specified and the node lies completely inside the `clippingRect's` coordinates |

inheritParentTransformBooleantrueREAD\_WRITEIf true, the node overall transformation is determined by combining the accumulated transformation matrix of all of its ancestors in the SceneGraph with the node local 2D transformation matrix described by its translation, rotation, scale and scaleRotateCenter fields. If false, the accumulated transformation of all of its ancestors in the SceneGraph is ignored and only the node local transformation matrix is used. This causes the node to be transformed relative to the root of the SceneGraph (that is, the Scene component)renderPassinteger0READ\_WRITEUsed in combination with the numRenderPasses field of nodes extended from the [ArrayGrid](/docs/references/scenegraph/abstract-nodes/arraygrid.md "ArrayGrid") abstract node class, to optimize rendering of lists and grids. This should never be set to a non-zero value unless you are optimizing the performance of a list or grid rendering by specifying the sequence of rendering operations for sub-elements of the list or grid items, and have set the numRenderPasses field value for the list or grid to a value greater than 1. If the numRenderPasses field value for the list or grid is set to a value greater than 1, you must set this field to a value greater than 0 for all sub-elements of the list or grid items, and not greater than the numRenderPasses field value. If the numRenderPasses field is set to a value greater than 1, and you set this field for a list or grid item sub-element to 0 (the default), or a value greater than the numRenderPasses field value, the list or grid item sub-element will not renderchildRenderOrderoption as stringrenderLastREAD\_WRITE

| Option | Description |
| --- | --- |
| `"renderFirst"` | any drawing done by this node will be done **before** the node children are rendered |
| `"renderLast"` | any drawing done by this node will be done **after** the node children are rendered |

clippingRectarray of float\[ 0.0, 0.0, 0.0, 0.0 \]READ\_WRITESpecifies a rectangle in the node local coordinate system that is used to limit the region where this node and its children can render. If a non-empty rectangle is specified, then all drawing by this node and its children will be limited to that rectangular area.

*   `ClippingRects` can be specified by the node or by any of its ancestors in the SceneGraph.
*   `ClippingRects` are automatically set by some nodes such as lists and grids.
*   `ClippingRects` are always clipped to the screen boundaries, so if a `clippingRect` is specified that is partially or completely offscreen, it will be clipped to the screen boundaries. With respect to render tracking, although the node could be completely within the bounds of the specified `clippingRect`, it's `renderTracking` field could be set to `"none"` if the portion of the `clippingRect` it occupies is completely offscreen.

enableRenderTrackingBooleanfalseREAD\_WRITEIf true, renderTracking will be set to a string describing how much of the node is rendered on screentranslationvector2d\[0.0,0.0\]READ\_WRITEDefines the origin of the node local coordinate system relative to its parent nodeopacityfloat1.0READ\_WRITESets the opacity of the node and its children. Opacity is the opposite of transparency. Opacity values range from 0.0 (fully transparent) to 1.0 (fully opaque). As the SceneGraph is traversed, the opacity values are combined by multiplying the current accumulated opacity with the node opacity, so that if the accumulated opacity of a node ancestors is 0.25 (75% transparent), the node will have opacity of 0.25 or less. This allows entire branches of the SceneGraph to fade in and out by animating the opacity of the node at the root of the branchmuteAudioGuideBooleanfalseREAD\_WRITESet to true to suppress the default CVAA text to speech. This allows apps to provide their own custom implementation

Sample app
----------

[LabelExample](https://github.com/rokudev/samples/tree/master/ux%20components/screen%20elements/renderable%20nodes/LabelExample) is a sample app demonstrating Label in action.