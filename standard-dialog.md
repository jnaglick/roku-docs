StandardDialog
==============

Extends [Group](/docs/references/scenegraph/layout-group-nodes/group.md "**Group**")

The **StandardDialog** node is the base for Roku's pre-built standard message, keyboard, pinpad, and progress dialogs. It can also be used directly with a custom dialog structure built with the **StdDialogItem** nodes.

Fields
------

FieldTypeDefaultAccess PermissionDescriptionwidthfloat0.0fREAD\_WRITESets the width of the dialog:  

*   If set to 0, the standard system dialog width is used (1038 for FHD, 692 for HD). If the title or any button text is too wide to fit within the standard width, the dialog width will be automatically increased to show the full title or button text up to a preset maximum (1380 for FHD and 920 for HD).
*   If set to greater than 0, the specified width is used as the overall width of the dialog.

heightfloat0.0fREAD\_WRITESets the height of the dialog.  
  
If this field is set to greater than 0, and the layout of the dialog for the specified width results in a dialog with a height less than the value of this field, the dialog layout is increased so that the dialog height matches the value of this field. In this case, the button area is moved to the bottom of the dialog and a blank region exists between the content area and the button area.buttonSelectedint0READ\_ONLYIndicates the index of the selected button when the user selects one of the buttons in the button area.buttonFocusedint0READ\_ONLYIndicates the index of the button that gained focus when the user moved the focus onto one of the buttons in the button area.paletteRSGPalette nodenot setREAD\_WRITESets the color palette for the dialog's background, text, buttons, and other elements.  
  
By default, no palette is specified; therefore, the dialog inherits the color palette from the nodes higher in the scene graph (typically, from the dialog's [Scene](/docs/references/scenegraph/scene.md) node, which has a **palette** field that can be used to consistently color the standard dialogs and keyboards in the app).  
  
The RSGPalette color values used by the StandardDialog node are as follows:  

| Palette Color Name | Usages |
| --- | --- |
| DialogBackgroundColor | Blend color for dialog's background bitmap. |
| DialogItemColor | Blend color for the following items:  <br><br>*   [StdDlgProgressItem's](/docs/references/scenegraph/standard-dialog-framework-nodes/std-dlg-progress-item.md) spinner bitmap<br>*   [StdDlgDeterminateProgressItem's](/docs/references/scenegraph/standard-dialog-framework-nodes/std-dlg-determinate-progress-item.md) graphic |
| DialogTextColor | Color for the text in the following items:  <br><br>*   [StdDlgTextItem](/docs/references/scenegraph/standard-dialog-framework-nodes/std-dlg-text-item.md) and [StdDlgGraphicItem](/docs/references/scenegraph/standard-dialog-framework-nodes/std-dlg-graphic-item.md) if the **namedTextStyle** field is set to "normal" or "bold".<br>*   All [content area items](/docs/references/scenegraph/standard-dialog-framework-nodes/std-dlg-item-base.md), except for [StdDlgTextItem](/docs/references/scenegraph/standard-dialog-framework-nodes/std-dlg-text-item.md) and [StdDlgGraphicItem](/docs/references/scenegraph/standard-dialog-framework-nodes/std-dlg-graphic-item.md).<br>*   [Title area](/docs/references/scenegraph/standard-dialog-framework-nodes/std-dlg-title-area.md#fields). Unfocused button. |
| DialogFocusColor | Blend color for the following:  <br><br>*   The [button area](/docs/references/scenegraph/standard-dialog-framework-nodes/std-dlg-button-area.md#fields) focus bitmap.<br>*   The focused scrollbar thumb. |
| DialogFocusItemColor | Color for the text of the focused button. |
| DialogSecondaryTextColor | Color for the text of in the following items:  <br><br>*   [StdDlgTextItem](/docs/references/scenegraph/standard-dialog-framework-nodes/std-dlg-text-item.md) and [StdDlgGraphicItem](/docs/references/scenegraph/standard-dialog-framework-nodes/std-dlg-graphic-item.md) if the **namedTextStyle** field is set to "secondary".<br>*   Disabled button. |
| DialogSecondaryItemColor | Color for the following items:  <br><br>*   The divider displayed below the title area.<br>*   The unfilled portion of the [StdDlgDeterminateProgressItem's](/docs/references/scenegraph/standard-dialog-framework-nodes/std-dlg-determinate-progress-item.md) graphic. |
| DialogInputFieldColor | The blend color for the text edit box background bitmap for keyboards used inside dialogs. |
| DialogKeyboardColor | The blend color for the keyboard background bitmap for keyboards used inside dialogs |
| DialogFootprintColor | The blend color for the following items:  <br><br>*   The button focus footprint bitmap that is displayed when the [button area](/docs/references/scenegraph/standard-dialog-framework-nodes/std-dlg-button-area.md#fields) does not have focus.<br>*   Unfocused scrollbar thumb and scrollbar track. |

closebooleanfalseWRITE\_ONLYDismisses the dialog. The dialog is dismissed whenever the close field is set, regardless of whether the field is set to true or false.wasClosedeventN/AREAD\_ONLYAn event that indicates the dialog was dismissed. This event is triggered when one of the following occurs:  

*   The **close** field is set.
*   The Back, Home, or Options key is pressed.
*   Another dialog is displayed.