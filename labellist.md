LabelList
=========

Extends [**ArrayGrid**](/docs/references/scenegraph/abstract-nodes/arraygrid.md)

The LabelList node class is a simple list class that can be used to display a list of items. Each item can include a text string and an optional icon positioned to the left of the text string.

The list items can be organized into sections that are demarcated by labelled horizontal divider lines between the sections.

In most cases, you will want to populate the content of a LabelList (or other similar components like PosterGrids, etc.) from a content feed, web service API, or some other dynamic data source. However, to illustrate the basics of how a LabelList is constructed and used, here is a simple example of creating a LabelList and populating it with static data. Static data in a LabelList may be useful for app screens such as settings or menus with fixed items.

**Static LabelList configuration**

    <LabelList
      id = "moviemenu"
      translation = "[160,92]"
      itemSize = "[440,48]" >
    
      <ContentNode id = "moviemenucontent" role = "content" >
    
        <ContentNode title = "Drama" />
        <ContentNode title = "Action" />
        <ContentNode title = "Horror" />
        <ContentNode title = "Comedy" />
    
      </ContentNode>
    
    </LabelList>
    

**Example**

This code renders the LabelList shown below.

In this screen shot, the user has scrolled down one item to highlight "Drama" showing the separator after the last list item.

![roku815px - LabelList-example1](https://image.roku.com/ZHZscHItMTc2/LabelList-example1.jpg "LabelList-example1")

Fields
------

FieldTypeDefaultAccess PermissionDescriptioncontentContentNodenoneREAD\_WRITESpecifies the content for the list. See [Data bindings](/docs/references/scenegraph/list-and-grid-nodes/labellist.md#data-bindings) below for more details.  
If the data contains section markers, section dividers will be drawn between each section. These section dividers may contain an icon and/or a string.itemSizevector2d\[0,0\]READ\_WRITESpecifies the width and height of each item in the listitemSpacingvector2d\[0,0\]READ\_WRITEThe second value of the vector specifies the vertical spacing between items in the list. The first value of the vector is ignored.numRowsinteger12READ\_WRITESpecifies the number of visible rows displayed. The actual number of rows may be more or less than the number of visible rows specified depending on the number of items in the list content.textHorizAlignoption stringleftREAD\_WRITESpecifies the horizontal alignment of the text in the list item area specified by the `itemSize` field value (the text is always centered vertically in the list item area). The possible options are:  
  

| Option | Effect |
| --- | --- |
| left | Text is left-aligned |
| right | Text is right-aligned |
| center | Text is center-aligned |

colorcolor0xddddddffREAD\_WRITESpecifies the text color for unfocused list itemsfocusedColorcolor0x262626ffREAD\_WRITESpecifies the text color for focused list itemsfontfontsystem defaultREAD\_WRITESpecifies the font for unfocused list itemsfocusedFontfontsystem defaultREAD\_WRITESpecifies the font for focused list itemsdrawFocusFeedbackBooleantrueREAD\_WRITESpecifies whether or not the focus indicator bitmap is displayeddrawFocusFeedbackOnTopBooleanfalseREAD\_WRITESpecifies whether the focus indicator bitmap is drawn below or on top of the list itemsvertFocusAnimationStylestringfixedFocusWrapREAD\_WRITESpecifies the how the focus indicator moves in a list of items in response to the remote direction pad Up and Down key presses. The possible values are:  
  

| Option | Effect |
| --- | --- |
| fixedFocusWrap | Causes the focus indicator to float up or down until it reaches the end of the list, at which point the focus indicator will stay fixed on the first or last item in the list, and the items will scroll up or down if there are items that were not visible. |
| floatingFocus | Causes the list to wrap around when the focus indicator reaches the first or last item in the list, as long as the list contains enough items to fill the list. If the list does not contain enough items to fill the list, the focus indicator will float up and down. |

focusRowinteger0READ\_WRITESpecifies the row that will have fixed focus if the `vertFocusAnimationStyle` field value is set to `fixedFocusWrap`.focusBitmapUriuriREAD\_WRITESpecifies the bitmap file used for the focus indicator when the list has focus. In most cases, this should be a 9-patch image that specifies both expandable regions as well as margins. Only set this field to specify a custom bitmap that differs in appearance from the default bitmap.focusFootprintBitmapUriuriREAD\_WRITESpecifies the bitmap file used for the focus indicator when the list does not have focus. In most cases, this should be a 9-patch image that specifies both expandable regions as well as margins. Only set this field to specify a custom bitmap that differs in appearance from the default bitmap.focusBitmapBlendColorcolor0xFFFFFFFFREAD\_WRITEBlend the graphic image specified by `focusBitmapUri` with the specified color. If set to the default, 0xFFFFFFFF, no color blending will occur. Set this field to show a focus indicator graphic image with a different color than the image specified by `focusBitmapUri.`focusFootprintBlendColorcolor0xFFFFFFFFREAD\_WRITEBlend the graphic image specified by `focusFootprintBitmapUri` with the specified color. If set to the default, 0xFFFFFFFF, no color blending will occur. Set this field to show a focus footprint indicator graphic image with a different color than the image specified by `focusFootprintBitmapUri`.wrapDividerBitmapUriuriREAD\_WRITESpecifies the bitmap file to use as a wrap divider, the visual separator between the last and first list items when the list wraps. In most cases, this should be a 9-patch image that specifies both expandable regions. Only set this field to specify a custom bitmap that differs in appearance from the default bitmap.wrapDividerHeightfloat0.0READ\_WRITESpecifies the height of the wrap divider, the visual separator between the last and first list items when the list wraps. The bitmap for the wrap divider is scaled to this height. The width of the wrap divider is set to the width of the list items as specified by the `itemSize` field width value.sectionDividerBitmapUriuriREAD\_WRITEIf the ContentNode specifies sections for a list or grid, specifies a custom bitmap to use as a visual divider between the sections of the list or grid. Only set this field to use a bitmap with a different appearance than the system default. For sections that do not include an icon or a title, the system default or custom bitmap specified as the `wrapDividerBitmapUri` field value is used for the section dividers. In most cases, you will want to use a 9-patch PNG bitmap with both expandable regions, which is the type of bitmap used as the system default.sectionDividerFontfontsystem defaultREAD\_WRITESpecifies the font for section divider labelssectionDividerTextColorcolor0xddddddffREAD\_WRITESpecifies the text color for section divider labelssectionDividerSpacingfloat10READ\_WRITEIf the ContentNode specifies sections for a list or grid, and the section dividers are specified to include an icon and/or a label, specifies the spacing between the icon, label, and section divider bitmap.sectionDividerHeightfloat40READ\_WRITESpecifies the height of the section dividers. The width of the section dividers is determined by the width of the list items as specified by the itemSize field width value.sectionDividerMinWidthfloat117READ\_WRITESpecifies the minimum width of the section divider bitmap. The section divider label will be ellipsized if necessary in order to ensure that the section divider bitmap meets the minimum width.sectionDividerLeftOffsetfloat0READ\_WRITENumber of pixels to offset the left edge of the section divider relative to the left edge of the list items.itemSelectedinteger0READ\_ONLY**Read-Only**  
When a list item is selected, itemSelected is set to the index of the selected item.itemFocusedinteger0READ\_ONLY**Read-Only**  
When a list item gains the key focus, set to the index of the focused item.itemUnfocusedinteger0READ\_ONLY**Read-Only**  
When a list item loses the key focus, set to the index of the unfocused item.jumpToIteminteger0WRITE\_ONLY**Write-Only**  
When set to a valid item index, this causes the list to immediately update so that the specified index moves into the focus position.animateToIteminteger0WRITE\_ONLY**Write-Only**  
When set to a valid item index, this causes the list to quickly scroll so that the specified index moves into the focus position.

Data bindings
-------------

The data model for the LabelList node should have a single ContentNode as the root node in its content field. The structure of the rest of the data model depends on whether or not the list items are to be grouped into sections.

**List Items Not Being Grouped Into Sections**

If the list items are not to be grouped into sections, one child ContentNode should be added to the root node for each item in the list (these child nodes can be thought of as _item nodes_). Item nodes should have their ContentNode attributes set as shown in the table below.

| Attribute | Type | Description |
| --- | --- | --- |
| `TITLE` | string | The label for the list item |
| `HDLISTITEMICONURL` | uri | The image file for the icon to be displayed to the left of the list item label when the list item is not focused |
| `HDLISTITEMICONSELECTEDURL` | uri | The image file for the icon to be displayed to the left of the list item label when the list item is focused |

**List Items Grouped into Sections**

If the list items are to be grouped into sections, one child ContentNode should be added to the root node for each section in the list (these child nodes can be thought of as _section roots_). Each section root should contain one child ContentNode for each item in the section (that is, item nodes). Each item ContentNode uses the same attributes as the item nodes when there are no sections, as shown in the table above.

The section root ContentNodes use the following attributes:

| Attribute | Type | Description |
| --- | --- | --- |
| `CONTENTTYPE` | string | Must be set to `SECTION` |
| `TITLE` | string | Label for the section divider |
| `HDGRIDPOSTERURL` | uri | The image file for the icon to be displayed to the left of the section label when the screen resolution is set to HD. |
| `SDGRIDPOSTERURL` | uri | The image file for the icon to be displayed to the left of the section label when the screen resolution is set to SD. |

Sample app
----------

[LabelListExample](https://github.com/rokudev/samples/tree/master/ux%20components/lists%20and%20grids/LabelListExample) is a sample app demonstrating LabelList in action.