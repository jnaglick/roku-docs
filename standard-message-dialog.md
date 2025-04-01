StandardMessageDialog
=====================

Extends [StandardDialog](/docs/references/scenegraph/standard-dialog-framework-nodes/standard-dialog.md "**Standard Dialog**")

The **StandardMessageDialog** node is used to displays a message to the user. It is similar to the legacy [Dialog](/docs/references/scenegraph/dialog-nodes/dialog.md) node. It may contain the following items (from top to bottom):

*   One or more blocks of text at the top.
*   One bulleted / numbered list.
*   One or more blocks of text at the bottom.

![roku815px - standard-message-dialog](https://image.roku.com/ZHZscHItMTc2/standard-message-dialog.jpg)

Structure
---------

The StandardMessageDialog is comprised of the following areas and building block nodes:

*   StdDlgTitleArea.
*   StdDlgContentArea, which may contain the following items:
    
    *   Zero or more StdDlgTextItem nodes (the block(s) of text with the main message).
    *   Zero or more StdDlgBulletTextItem nodes (bulleted/numbered list).
    *   Zero or more StdDlgTextItem nodes (the block(s) of text with the bottom message).
*   StdDlgButtonArea, which may contain zero or more StdDlgButton nodes.

Fields
------

| Field | Type | Default | Access Permission | Description |
| --- | --- | --- | --- | --- |
| title | string | ""  | READ\_WRITE | The title to be displayed at the top of the dialog. |
| message | array of strings | \[ \] | READ\_WRITE | One or more blocks of informational text displayed at the top of the dialog's content area. Each string in the array is displayed as a separate block of text with the standard amount of space left between the blocks.  <br><br>> To separate lines of text, add each line as an element in the array. Do not use newline characters. |
| bulletText | array of strings | \[ \] | READ\_WRITE | An array of strings displayed as a bulleted or numbered list. The list is displayed in the content area below the message and above the bottom message. |
| bulletType | string | "bullet" | READ\_WRITE | If the **bulletText** field is set, specifies the type of list item delimiter, which may be one of the following:  <br><br>*   "bullet" (this is the default)<br>*   "numbered"<br>*   "lettered"<br><br>. |
| bottomMessage | array of strings | \[ \] | READ\_WRITE | One or more blocks of informational text displayed at the bottom of the dialog's content area. Each string in the array is displayed as a separate block of text with the standard amount of space left between the blocks.  <br><br>> To separate lines of text, add each line as an element in the array. Do not use newline characters. |
| buttons | array of strings | \[ \] | READ\_WRITE | List of buttons to be displayed in the button area at the bottom of the dialog. Each string in the buttons array adds a new button to the button area.  <br><br>> Minimize the number of buttons in the dialog to ensure they are all visible when the dialog is displayed. |

Sample app
----------

You can download and install a [sample app](https://github.com/rokudev/standard-dialog-framework) that demonstrates how to create a standard message dialog.