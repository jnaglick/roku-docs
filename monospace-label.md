MonospaceLabel
==============

_Available since Roku OS 14.0_

The **MonospaceLabel** node is used to draw a single line of text with all characters spaced at a fixed distance from each other. It transforms proportional fonts into monospaced fonts. It is a substitute for using a monospace font with the **Label** node.

**Fields**

| **Field** | **Type** | **Default** | **Access Permission** | **Description** |
| --- | --- | --- | --- | --- |
| text | string |     | READ\_WRITE | Specifies the text to be displayed |
| color | color | 0xddddddff | READ\_WRITE | Specifies the text color |
| font | Font | system default | READ\_WRITE | Specifies the Font node to be used |
| horizAlign | string | left | READ\_WRITE | See [Horizontal Alignment](/docs/references/scenegraph/typographic-nodes/scrollinglabel.md#alignment) |
| vertAlign | string | top | READ\_WRITE | See [Vertical Alignment](/docs/references/scenegraph/label-nodes/label-base.md#wrapping-text) |
| width | float | 0   | READ\_WRITE | Specifies the width of the label. If set to zero, the width of the label will be set automatically |
| height | float | 0   | READ\_WRITE | Specifies the height of the label. If set to zero, the height of the label will be set automatically |
| characterWidth | float | 0   | READ\_WRITE | Specifies the width of the label characters. If set to zero, width of font’s character 'M' will be used |
| ellipsizeOnBoundary | Boolean | false | READ\_WRITE | If the width field value is greater than zero, controls whether or not the last line of text displayed should be ellipsized if it extends beyond the specified width. It is ignored if the truncateOnDelimiter field value is set to a non-empty stringWhen set to true, text will be ellipsized by whole words. Example: "This is the last line of..."When set to false, text will be ellipsized by characters. Example: "This is the last line of tex..." |
| firstCharTrueLeftAlign | Boolean | false | READ\_WRITE | Forces the first character to left align completely instead of rendering centered in the character box. Subsequent characters are centered in their character box. If enabled monospace text strings with different first characters will shift around. This is primarily used for single characters strings |
| wordBreakChars | string |     | READ\_WRITE | By default, space and hyphen characters are used to determine where lines can be divided. In addition, this field can specify additional characters to be used to determine where the text can be broken into lines |
| isTextEllipsized | Boolean | false | READ | Tells whether or not currently displayed text is clipped. |