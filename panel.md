Panel
=====

Extends [**Group**](/docs/references/scenegraph/layout-group-nodes/group.md)

The Panel node is used to create sliding panels for app UI, similar to those in the Roku OS home screen.

Fields
------

FieldTypeDefaultAccess PermissionUsepanelSizestringnarrowWRITE\_ONLY**Write-Only**  
Specifies one of the default panel sizes. Setting the field causes the width and leftPosition fields to be set to values that match the RSG preferred layout for a panel of the specified size.  
  

| Value | Meaning |
| --- | --- |
| narrow | Set the width and leftPosition fields to the values for a narrow Panel |
| medium | Set the width and leftPosition fields to the values for a medium width Panel |
| wide | Set the width and leftPosition fields to the values for a wide Panel |
| full | Set the width and leftPosition fields to the values for a full width Panel |

  
  
Note that PanelSet usage mandates that whenever two Panels are visible, they should include either one narrow and one wide panel or two medium width panels. If one Panel is visible, it's panelSize should be set to "full".widthfloat388READ\_WRITESpecifies the width of the panel in pixels. In most cases, this should be set by setting the panelSize field to one of the pre-configured settings.heightfloat\-1READ\_WRITESpecifies the height of the panel. In most cases, this will be set by the PanelSet and should treated as a read-only value.leftPositionfloat105READ\_WRITESpecifies the horizontal position of the panel relative to the left edge of the PanelSet (which is a the left edge of the display by default). In most cases, this should be set by setting the panelSize field to one of the pre-configure settings.overhangTitlestring""READ\_WRITEWhen the panel is used as part of the OverhangPanelSetScene, setting the overhangTitle field will cause that text to be displayed as the title in the overhang when the panel slides into the left position of the PanelSet.clockTextstring""READ\_WRITEWhen the panel is used as part of the OverhangPanelSetScene, setting the clockText field will cause that text to be displayed instead of the clock in the overhang when the panel slides into the left position of the PanelSet.optionsAvailableBooleanfalseREAD\_WRITEWhen the panel is used as part of the OverhangPanelSetScene, setting optionsAvailable will enable/disable the options button handling when the panel slides into the left position of the PanelSet. The overhang's options prompt will change appearance to provide feedback to the user that the options button is enabled/disabled.leftOrientationBooleanfalseREAD\_WRITEWhen the panel is used as part of the OverhangPanelSetScene, leftOrientation will be set to true when the panel moves into the left position of the PanelSet and set to false when the panel moves into the right position of the PanelSet.leftOnlyBooleanfalseREAD\_WRITEThe leftOnly field provides information to the PanelSet that this Panel should never appear in the right position of the PanelSet. When the panels are sliding back towards the home position (as a result of a **Left** or **Back** key press), and the panel slides into the right position, the PanelSet initiates another slide in the same _back_ direction so that the panel does not end up on the right.hasNextPanelBooleanfalseREAD\_WRITEThe hasNextPanel field provides information to the PanelSet as to whether or not this panel has another panel to its right. If set to true, the PanelSet's right arrow indicator is displayed and pressing the right arrow button on the remote triggers the PanelSet to move the focus one panel to the right, sliding the Panels as needed to make sure the panel that has the focus ends up onscreen. If set to false, the PanelSet's right arrow indicator is not displayed and the right arrow button does not trigger any change to the focused panel.isFullScreenBooleanfalseREAD\_WRITEThe isFullScreen field indicates that this panel should be the only panel displayed (i.e. it will take up both the left and right positions in the PanelSet.goBackCountinteger1READ\_WRITESetting goBackCount field to a value greater than 1 causes the PanelSet to move the focus back that many panels when the user presses the left arrow button, sliding the Panels as needed to make sure the panel that has the focus ends up onscreen.selectButtonMovesPanelForwardBooleantrueREAD\_WRITEWhen set to true, pressing the OK/Select button on the remote control causes the PanelSet focus to move to the next panel.isOffscreenLeftBooleanfalseREAD\_WRITEThis field is set by the PanelSet to indicate that the panel is positioned offscreen of the left edge of the PanelSet. This field is often observed to cancel outstanding load requests for images that are displayed on the panel.

Sample app
----------

[PanelExample](https://github.com/rokudev/samples/tree/master/ux%20components/sliding%20panels/PanelExample) is a sample app demonstrating Panel in action.