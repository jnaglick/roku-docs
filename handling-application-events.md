Handling application events
===========================

Observer callback models
------------------------

Roku OS 7.5 introduces a fundamental change in the observer callback model changing from a queued or deferred model to a more intuitive and expected recursive callback model.

### Queued callback model

In Roku OS releases prior to v7.5, callbacks operated in a queued manner.

**Nested functions example**

    function init()
        m.top.observeField("f1", "c1")
        m.top.observeField("f2", "c2")
        m.top.f1 = v1
        print "init(): "; m.top.f2
    end function
    function c1()
        m.top.f2 = v2 ' immediate callback of c2() right here
        print "c1(): "; m.top.f2
    end function      ' deferred callback of c2() on return
    function c2()
        print "c2(): "; m.top.f2
    end function
    

In the nested functions example above, the queued callback model would have the ordering of the print statements as:

**Output**

    c1(): v2
    c2(): v2
    init(): v2
    

### Recursive callback model

With the recursive callback model, the expected output is now more intuitive and the order of the print statements would be:

**Output**

    c2(): v2
    c1(): v2
    init(): v2
    

The `rsg_version` entry in your the manifest file defaults to 1.2. To check and test different SceneGraph versions without refactoring your app, see the guide on [Debugging](/docs/developer-program/debugging/debugging-channels.md)

> Note that support for the [“rsg\_version=1.0"](/docs/developer-program/getting-started/architecture/channel-manifest.md#special-purpose-attributes) manifest flag is deprecated as of Roku OS 8. This deprecation means that the 1.0 features continue to work in Roku OS 8, but will no longer be supported (and thus should not be expected to work) starting with the next major firmware release. All apps must adopt the [current observer callback](/docs/developer-program/core-concepts/handling-application-events.md#recursive-callback-model) model in successive firmware updates.

### Event handling

There are three types of events that can occur in a SceneGraph application that you can use to control the behavior of an application component.

*   User remote control key presses
*   Component and node field value changes, such as state changes of a previously-launched component, and node actions
*   Functional Fields

Handling remote control key presses
-----------------------------------

You can handle remote control key events by writing an [`onKeyEvent()`](/docs/references/scenegraph/component-functions/onkeyevent.md) function in the [<script>](/docs/references/scenegraph/xml-elements/script.md) element of the component you want to handle the key press event. When the component, or its children, have remote control focus, the `onKeyEvent()` function is called whenever an unhandled key event bubbles up the focus chain to the component.

### Using the onKeyEvent() function

The `onKeyEvent()` function takes two parameters, `key` and `press`. The `press` parameter is a Boolean value that is true if the key was pressed, and false if the key was released. The `key` parameter of the `onKeyEvent()` function contains a string, which is case-sensitive, that identifies the key that was pressed. The `key` strings supported by the `onKeyEvent()` function, and the corresponding remote key, are as follows:

![roku815px - rokuremotekeysnew](https://image.roku.com/ZHZscHItMTc2/rokuremotekeysnew-v1.png "rokuremotekeysnew")

| String | Key | Appearance/Icon |
| --- | --- | --- |
| back | **Back** | left-pointing arrow at top of remote |
| up  | **Up** | up-pointing caret of remote directional pad |
| down | **Down** | down-pointing caret of remote directional pad |
| left | **Left** | left-pointing caret of remote directional pad |
| right | **Right** | right-pointing caret of remote directional pad |
| OK  | **OK** | key usually labeled **OK** near or in the center of remote directional pad |
| replay | **Replay** | key usually labeled with a circular-pointing arrow |
| play | **Play/Stop** | key usually labeled with a right-pointing triangle and two bars |
| rewind | **Rewind** | key usually labeled with two left-pointing triangles |
| fastforward | **Fast Forward** | key usually labeled with two right-pointing triangles |
| options | **Options** | key labeled with an asterisk |

The `onKeyEvent()` function must return true if the component handled the event, or false if it did not handle the event. Returning false allows the event to continue bubbling up the focus chain (see [Remote control events](/docs/developer-program/core-concepts/scenegraph-xml/remote-control-events.md)) so that ancestors of the component can handle the event.

Starting from Roku OS 8, the behavior of the Roku system overlay is such that the system overlay now slides in whenever the \* button is pressed, the Video node is in focus, and the app does not have its OnKeyEvent() handler fired. When the Video node is not in focus, the system overlay does not slide in and the OnKeyEvent() handler is fired.

There are one or more keys on any Roku remote control which are not handled by the `onKeyEvent()` function (or any Roku application event handler), such as the **Home** key. Presses of these keys are handled by the global Roku firmware event handler in a default manner that cannot be modified by application code. Also note that several node classes handle certain remote control key events automatically, so `onKeyEvent()` is not required to handle those events, and should not be used for those events in those nodes. As an example of node classes that automatically handle certain remote control key events, grid node classes such as [PosterGrid](/docs/references/scenegraph/list-and-grid-nodes/postergrid.md) automatically handle **Up**, **Down**, **Right**, and **Left** key presses when the poster grid has focus. Typically, you should use the [ifSGNodeField](/docs/references/brightscript/interfaces/ifsgnodefield.md) `observeField()` method to handle changes in the subject node fields caused by automatic key event handling of the node.

### Example

The following `onKeyEvent()` example handles supported remote control key presses other than the **Back** key by displaying a warning message until the **OK** key is pressed.

**onKeyEvent() event handling example**

    function onKeyEvent(key as String, press as Boolean) as Boolean
      handled = false
      if press then
        if (key = "back") then
          handled = false
        else
          if (m.warninglabel.visible = false)
            m.warninglabel.visible="true"
          else
            if (key = "OK") then
              m.warninglabel.visible="false"
            end if
          end if
          handled = true
        end if
      end if
      return handled
    end function
    

Handling node field value changes
---------------------------------

There are two [ifSGNodeField](/docs/references/brightscript/interfaces/ifsgnodefield.md) methods that allow you to create (and remove) observers that continuously monitor any field value of any [roSGNode](/docs/references/brightscript/components/rosgnode.md) object, including the interface fields you have created for custom SceneGraph components:

*   `observeField()`
*   `unobserveField()`

### Using the ifSGNodeField observeField() and unobserveField() methods

`observeField()` is an overloaded method with two versions, useful for different purposes. The first allows you to trigger a specified callback function in response to any change in the value of the observed node field. For example, to set up an observer of a [Timer](/docs/references/scenegraph/control-nodes/timer.md) node `fire` field that calls a `handleexampletimerfire()` event handler function that you write:

`exampletimer.ObserveField("fire", "handleexampletimerfire")`

Once this observer is set up, the component will continuously monitor the `exampletimer` node object `fire` field for the remaining existence of the component or node, or until `unobserveField()` is called (perhaps as part of the event handler function itself):

`exampletimer.unobserveField("fire")`

The event handler function you write must be included in the component <script> element, and manipulate objects within the scope of the component.

By default observers are only called when the value of a field changes. Code that assigns a field value to the same value as currently assigned to the field will not trigger observers. If you want observers to be called any time a field value is set, regardless of the value, an <interface> field must be defined with the `alwaysNotify` attribute set to true.

Here is an example of a field observer and the associated event handler function:

**Field observer XML BrightScript example**

    <script type = "text/brightscript" >
    
      <![CDATA[
    
      sub init()
        m.top.setFocus(true)
        m.bottomlabel = m.top.findNode("bottomLabel")
        m.texttimer = m.top.findNode("textTimer")
        m.defaulttext = "All The Best Videos!"
        m.alternatetext = "All The Time!!!"
        m.textchange = false
        m.texttimer.ObserveField("fire", "changetext")
      end sub
    
      sub changetext()
        if (m.textchange = false) then
          m.bottomlabel.text = m.alternatetext
          m.textchange = true
        else
          m.bottomlabel.text = m.defaulttext
          m.textchange = false
        end if
      end sub
    
      ]]>
    
    </script>
    

> **Optional roSGNodeEvent Callback Function** Argument Field observer callback functions can specify an [roSGNodeEvent](/docs/references/brightscript/events/rosgnodeevent.md) argument. For example, the changetext() callback function signature in the example above could have been written as sub changetext(event as roSGNodeEvent). In this case, the callback function can call the [roSGNodeEvent](/docs/references/brightscript/events/rosgnodeevent.md) functions to extract information about the node that triggered the callback, specific field that triggered the callback, etc.

The second version of `observeField()` lets you specify a message port to notify when the observed field changes:

        `m.texttimer.ObserveField("fire", m.port)`
    

This second case is used when you want a field change to trigger an event in a BrightScript message loop. This is useful when using BrightScript components (such as roChannelStore) which can only be instantiated in the main BrightScript thread or a **Task** node thread. In this case, when the observed field changes, an [roSGNodeEvent](/docs/references/brightscript/events/rosgnodeevent.md) is sent to the port passed to the `observeField()` call. The event `getField()` and `getData()` functions can be called to determine the specific node field that changed, and the new value of the field, respectively.

### Event cascades

Setting the value of a field triggers any field observer functions that may be associated with the field as the result of an observeField() call. These observer functions may set other component fields, triggering calls to their observer functions. Such a situation triggers what is called an **event cascade**. Simply put, this is the chain of field setting and observer function calls that result from setting a single field of a node.

For example, if the width field of a Rectangle that contains a Label is set, an observer of that width field might set the width field of the Label. The width field of the Label might have an observer function that sets the Label’s wrap field. The chain of field settings and observer callback functions that result from setting a field (in this case, the Rectangle’s width field) is an event cascade.

With the release of Roku OS 7.5, nested observer callbacks are no longer deferred. Observer callbacks now happen recursively. See the [Queued Callback Model](/docs/developer-program/core-concepts/handling-application-events.md#queued-callback-model) section above for details.

Handling component <interface> field value changes
--------------------------------------------------

Any <field> element defined in a component [<interface>](/docs/references/scenegraph/xml-elements/interface.md) element can have an observer attached by setting the value of the optional `onChange` attribute. Set the `onChange` attribute to the callback function name that will handle the component field value change.

Note that the <field> element also includes a related optional attribute, `alwaysNotify`. You can set this attribute to true to have the callback function triggered every time the component field value is set, even if the value itself does not change. To have the callback function triggered only when the component field value changes, set the `alwaysNotify` attribute to false.

Functional fields
-----------------

In addition to the field observer model, functions can be called procedurally using the [callFunc() method](/docs/references/brightscript/interfaces/ifsgnodedict.md#callfunc).