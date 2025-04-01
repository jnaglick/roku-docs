BusySpinner
===========

Extends [**Group**](/docs/references/scenegraph/layout-group-nodes/group.md)

The BusySpinner node class is a simple widget that displays a continuously rotating bitmap. Since the BusySpinner node class uses an internal Poster node instance, the busy spinner bitmap can be specified by setting the internal Poster node uri field.

[SimpleBusySpinner](https://github.com/rokudev/samples/tree/master/ux%20components/widgets) is a sample app that demonstrates usage of the BusySpinner.

> Not all Roku Player hardware versions support arbitrary rotations. In particular, some hardware versions only support 90 degree rotation increments. In those cases, the icon will step through 90 degree, 180 degree, 270 degree and back to 0 degree rotations, rather than spin smoothly.

Fields
------

FieldTypeDefaultAccess PermissionDescriptionposterPoster nodeinternal instance defaultREAD\_WRITESet the uri field of the Poster node to select the bitmap for the busy spinnercontroloption stringnoneREAD\_WRITESets the operational state of the busy spinner

| Option | Effect |
| --- | --- |
| none | No operational state set. The busy spinner will run if not set to `"stop"`. |
| start | Starts the busy spinner if not running |
| stop | Stops the busy spinner if running |

clockwiseBooleantrueREAD\_WRITESpecifies whether the bitmap rotates in a clockwise or counterclockwise directionspinIntervaltime2READ\_WRITEThe number of seconds to complete a 360-degree rotation of the spinner image. A value of 0 will cause the spinner to remain stationary and not spin

Sample app
----------

[BusySpinnerExample](https://github.com/rokudev/samples/tree/master/ux%20components/widgets/BusySpinnerExample) is a sample app demonstrating BusySpinner in action.