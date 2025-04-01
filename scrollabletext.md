ScrollableText
==============

Extends [**Group**](/docs/references/scenegraph/layout-group-nodes/group.md "**Group**")

The ScrollableText node class provides an interactive, vertically scrolling pane of text. This is typically used to display several paragraphs of text to the user that are too long to fit onto the display, such as a license agreement.

### Alignment

The ScrollableText node class uses the horizAlign and vertAlign fields to allow you to position the rendered text relative to a specified bounding rectangle.

#### Horizontal Alignment

The horizAlign field allows you to position text horizontally relative to the computed width of the ScrollableText node. The computed width is determined by subtracting the width of the scrollbar from the value specified by the width field.

There are three possible values for the horizAlign field:

*   left The left edge of the text is drawn at the 0 x-coordinate position of the ScrollableText node's local coordinate system.
    
*   center The horizontal center of each line of text is positioned at the x-coordinate corresponding to half the computed width of the ScrollableText node's local coordinate system.
    
*   right The right edge of each line of text is positioned at x-coordinate position corresponding to the computed width of the ScrollableText node's local coordinate system.
    

In most cases, the horizAlign field should remain set to left.

#### Vertical Alignment

The vertAlign field allows you to position text vertically relative to the height of the ScrollableText node, as specified by the height field.

There are three possible values for the vertAlign field:

*   top The top edge of the text is drawn at 0 y-coordinate position of the ScrollableText node's local coordinate system.
    
*   center The vertical center of the rendered text is positioned at y-coordinate position corresponding to half the computed height of the ScrollableText node's local coordinate system.
    
*   bottom The text is drawn so that bottom edge of the rendered text is positioned at the y-coordinate position corresponding to the computed height of the ScrollableText node's local coordinate system.
    

In most cases, the vertAlign field should remain set to top.

Fields
------

| Field | Type | Default | Access Permission | Description |
| --- | --- | --- | --- | --- |
| text | string | ""  | READ\_WRITE | Specifies the text to be displayed |
| color | color | 0xddddddff | READ\_WRITE | Specifies the text color |
| font | Font | system default | READ\_WRITE | Specifies the Font node to be used |
| width | float | 0.0 | READ\_WRITE | Specifies the width of the node. This includes both the area where the text is rendered in addition to the scroll bar on the right |
| height | float | 0.0 | READ\_WRITE | Specifies the height of the node. If the text to be displayed is larger than this height, a scrollbar is automatically added on the right, allowing users to scroll up and down using the remote's arrow keys |
| lineSpacing | float | 8   | READ\_WRITE | If the text is displayed on more than one line, specifies the amount of additional space added between lines |
| horizAlign | string | left | READ\_WRITE | See [Horizontal Alignment](/docs/references/scenegraph/typographic-nodes/scrollinglabel.md#alignment "Horizontal Alignment") |
| vertAlign | string | top | READ\_WRITE | See [Vertical Alignment](/docs/references/scenegraph/typographic-nodes/scrollinglabel.md#alignment "Vertical Alignment") |
| scrollbarTrackBitmapUri | string | ""  | READ\_WRITE | Specifies the URI of an image file to be loaded to replace the default scrollbar track. This should be a 9-patch image so that it can be stretched to the appropriate height specifed by the height field |
| scrollbarThumbBitmapUri | string | ""  | READ\_WRITE | Specifies the URI of an image file to be loaded to replace the default scrollbar thumb. This should be a 9-patch image so that it can be stretched to the appropriate size |

Sample app
----------

[ScrollableTextExample](https://github.com/rokudev/samples/tree/master/ux%20components/text/ScrollableTextExample) is a sample app demonstrating ScrollableText in action.