TargetSet
=========

Extends [**Node**](/docs/references/scenegraph/node.md)

The TargetSet node class is used to specify a set of target regions where items in a [TargetGroup](/docs/references/scenegraph/layout-group-nodes/targetgroup.md) node are rendered. This information includes an array of rectangles that is used to define the location and size of a region that will be occupied by an item in the TargetGroup as well as an optional index that identifies one rectangle in the array to be treated as the region where the item with focus is located.

Fields
------

| Field | Type | Default | Access Permission | Description |
| --- | --- | --- | --- | --- |
| targetRects | array of rectangles | \[ \] | READ\_WRITE | Specifies an array of rectangles that define the target regions used by a TargetGroup node. To specify a rectangle, you can either specify a associative array with x, y, width and height elements or an array of 4 numeric values. For example, you could specify an array of two rectangles like this:  <br>  <br>\[ \[ x:10, y:5, width: 200, height:150 \], \[ x:10, y:160, width: 200, height:150 \] \]  <br>  <br>Alternately, you could specify the same array like this:  <br>  <br>\[ \[ 10, 5, 200, 150 \], \[ 10, 160, 200, 150 \] \] |
| focusIndex | integer | \-1 | READ\_WRITE | Identifies the index of an element of the targetRects array that will be treated as the region occupied by the focus item. The default of of -1 indicates that the TargetGroup's current focus index will not be changed when the TargetGroup is set to use the TargetSet to define its target regions. |
| color | Color | 0xFFFFFF80 | READ\_WRITE | If the TargetGroup using this TargetSet has its showTargetRects field set to true, the target rectangles of the current TargetSet will be drawn using the specified color. Drawing the TargetSet's target rectangles is generally only done when debugging an application. |

Sample app
----------

[TwoRowFixedFocus](https://github.com/rokudev/samples/tree/master/ux%20components/screen%20elements/target_group/TwoRowFixedFocus) is a sample app demonstrating TargetSet in action.