StdDlgGraphicItem
=================

Extends [StdDlgItemBase](/docs/references/scenegraph/standard-dialog-framework-nodes/std-dlg-item-base.md "**StdDlgItemBase**")

The **StdDlgGraphicItem** node is used to display an image in the dialog's content area with an optional text label displayed to the left, right, above, or below the image. It should only be used as a child of a [**StdDlgContentArea**](/docs/references/scenegraph/standard-dialog-framework-nodes/std-dlg-content-area.md) node.

![roku815px - std-dlg-graphic-item](https://image.roku.com/ZHZscHItMTc2/std-dlg-graphic-item.jpg)

Fields
------

FieldTypeDefaultAccess PermissionDescriptiontextstring""READ\_WRITESpecifies the text to be displayed next to the graphic. If the text width does not fit within the width of the content area, the text will wrap onto multiple lines.graphicUriuri""READ\_WRITEThe URI of the image to be displayed.graphicWidthfloat0READ\_WRITEThe image width to be used instead of the image's actual width.graphicHeightfloat0READ\_WRITEThe image height to be used instead of the image's actual height.graphicAlignstring"left"READ\_WRITESpecifies where to position and align the graphic and its text label, relative to the content area. This may be one of the following values:  

| Value | Text Position |
| --- | --- |
| left | The graphic is left-aligned in the content area.  <br>The text label is positioned horizontally to the right of the graphic, and centered vertically. |
| right | The graphic is right-aligned in the content area.  <br>The text label is positioned horizontally to the left of the graphic, and centered vertically. |
| center\_below | The graphic and text label are centered horizontally in the content area.  <br>The graphic is positioned below the text label. |
| center\_above | The graphic and text label are centered horizontally in the content area.  <br>The graphic is positioned above the text label. |