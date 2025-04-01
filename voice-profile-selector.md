Selecting user profiles with Roku Voice
=======================================

Apps with a profile selection screen can audibly and visually prompt the viewer to select a user profile and then handle a voice request with the name or position of the selected profile.

![roku815px - profile-selector](https://image.roku.com/ZHZscHItMTc2/profile-selector-800.jpg)

Updating the manifest
---------------------

To identify that your app supports a hands-free voice profile selection screen, add the following flag to the [manifest](/docs/developer-program/getting-started/architecture/channel-manifest.md): `voice_action_launch_screen=1`.

Implementing profile selector APIs
----------------------------------

To implement voice support for a profile selection screen, integrate the following APIs:

*   **Voice request trigger**. Upon launch, apps can call the [roAppManager.StartVoiceActionSelectionRequest()](/docs/references/brightscript/interfaces/ifappmanager.md#startvoiceactionselectionrequest-as-void) function to trigger a voice request for the viewer to select a user profile on devices that are paired with a hands-free Roku Voice remote control, such as the Roku Voice Remote Pro.
    
    Before calling the **StartVoiceActionSelectionRequest()** function, developers can call the [roDeviceInfo.HasFeature("handsfree\_voice")](/docs/references/brightscript/interfaces/ifdeviceinfo.md#hasfeaturefeature-as-string-as-boolean) function to check whether a Roku device is paired with a hands-free Roku remote control.
    
          appMgr = CreateObject("roAppManager")
          deviceInfo = CreateObject("roDeviceInfo")
          ' channel is launched and profile selection screen is displayed
          if deviceInfo.HasFeature("handsfree_voice")
              appMgr.StartVoiceActionSelectionRequest()
          end if
        
    

*   **Profile selection via registered/matched text strings**. Apps can call the [roAppManager.SetVoiceActionStrings()](/docs/references/brightscript/interfaces/ifappmanager.md#setvoiceactionstringsactions-as-object-as-void) function to register a list of text strings, such as user profile names, that can be matched to voice requests.
    
          appMgr = CreateObject("roAppManager")
          profile1 = { text: "kids", link: "d46ge-i8Y5-192"}
          profile2 = { text: "Jane", link: "2a2Nu-u1D4-555"}
          profile3 = { text: "John", link: "6Nu70-N37x-901"}
        
          actions = [profile1, profile2, profile3]
        
          appMgr.SetVoiceActionStrings(actions)
        
    
    When the name uttered by the user matches the registered text string, the matched text string is provided to the app via the [roInput voice command handler](/docs/references/brightscript/interfaces/ifinput.md#eventresponseroassociativearray-aa-as-boolean). Specifically, if the **command** received by the handler is "action", the associative array returned by the [**roInputEvent.GetInfo()**](/docs/references/brightscript/events/roinputevent.md#getinfo-as-object) method includes a **text** field that is set to the matched text string.
    
          function handleTransport(evt)
              cmd = evt.command
              ret = {status: "unhandled"}
              if cmd = "action"
                  print "profile name uttered by user"
                  print evt.text ' prints "kids", "Jane", or "John"
              end if
              return ret
          end function
        
    

*   **Profile selection via ordinal numbers**. The [roInput voice command handler](/docs/references/brightscript/interfaces/ifinput.md#eventresponseroassociativearray-aa-as-boolean) also supports profile selection via ordinal numbers. For example, when a user says "first", "number one", "pick the first", "select the first", "choose the first", and so on to select a user profile within a row, the app will receive a value of "1".
    
    Specifically, if the **command** received by the handler is "select", the associative array returned by the [**roInputEvent.GetInfo()**](/docs/references/brightscript/events/roinputevent.md#getinfo-as-object) method includes a **ordinal** field that is set to a numerical value corresponding to the ordinal number spoken by the user. Values may range between 1–6 (one-based indexing is used).
    
          function handleTransport(evt)
              cmd = evt.command
              ret = {status: "unhandled"}
              if cmd = "select"
                  print "Voice ordinal event"
                  print evt.ordinal ' prints 1
              end if
              return ret
          end function
        
    

Sample app
----------

The [custom UI voice control sample (**Transport\_Control\_CustomUI**)](https://github.com/rokudev/transport-control) demonstrates how to add voice support to a profile selection screen.