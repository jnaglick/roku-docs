API reference
=============

Construction
------------

### Roku\_Ads() as Object

This is the main entry point for instantiating the ad interface. This object manages ad server requests, parses ad structure, schedules and renders ads, and triggers tracking beacons.

The Roku ad parser/renderer object returned has global scope because it is meant to represent interaction with external resources (the ad server and any tracking services) that have persistence and state independent of the ad rendering within a client application.

Control
-------

### fireTrackingEvents(adStructure as Object, ctx as Object) as Boolean

#### Description

Triggers event tracking, including parameter substitution for Nielsen DAR, when library client code handles the ad rendering. This method can be used in scenarios where the RAF ad renderer is not used (for example, custom ad rendering or server-stitched ads).

#### Parameters

| Name | Type | Description |
| --- | --- | --- |
| adStructure | Object | Can refer to a pod (array) of ads or a single ad. Must at least contain a Tracking array member (see [Ad Structure example](/docs/developer-program/advertising/integrating-roku-advertising-framework.md#ad-structure)), and may optionally contain an ‘adServer’ member string. |
| ctx | Object | Structure to capture context-specific trigger conditions. ‘type’ key-value pair used to trigger events of a specific type. ‘time’ key-value pair used to trigger time-dependent events at or prior to this time |

#### Return Value

A flag indicating whether all beacons of the requested type were successfully fired.

### getAds(msg as string) as Object

#### Description

Gets the set of ads to be rendered now. This method may be called with no parameters or with a **msg** parameter.

*   When called with no parameters, this function returns the full list of all ad pods parsed from the ad server response.
*   When called with the **msg** parameter, this function can be used as an event listener in the client application’s main video playback loop to check whether midroll or postroll ads should be shown or not.

#### Parameters

| Name | Type | Description |
| --- | --- | --- |
| msg | String | Optional, depending on use case. Typically, this would be a message returned from a [WaitMessage()](/docs/references/brightscript/interfaces/ifmessageport.md#waitmessagetimeout-as-integer-as-dynamic) call on the message port of the [roVideoScreen](/docs/references/brightscript/components/rovideoscreen.md) or [roVideoPlayer](/docs/references/brightscript/components/rovideoplayer.md) object during content playback.  <br>  <br>This allows determination of which ads are scheduled for rendering based on playback position, user action, or other conditions. |

#### Return Value

Available ad pod(s) scheduled for rendering or invalid, if none are available

### showAds(ads as Object, ctx as Object, view as Object) as Boolean

#### Description

Renders any ads scheduled for display.

When this method is called with an array of ad pods (for example, using the value returned from the initial call to the [getAds()](/docs/developer-program/advertising/raf-api.md#client-ad-insertion) method), this is interpreted to mean that any preroll ad pod present should be rendered.

Client applications should always check the return value. If it is false, an application should exit content playback and return to the content selection screen. Typically, this occurs when the user presses the “Back” button during ad playback.

#### Parameters

| **Argument** | **Type** | **Required**? | **Description** |
| --- | --- | --- | --- |
| ads | array of ad pods | required | Ads to be rendered. Can represent either a single pod of ads or an array of ad pods. |
| ctx | associative array | optional | An associative array that allows client code to provide new offset and total to ad counter to support use cases involving interleaving RAF rendering with custom rendering within a single pod of ads. When used, it should be in the form of:  <br>  <br>`{ start: Integer, total: Integer }`  <br>  <br>For example, `{ start: 1, total: 4 }` would display as: "Ad 1 of 4" in the top left corner during ad playback. |
| view | renderable node | required (for SceneGraph applications) | Parameter representing a renderable node to which the ad UI can be parented.  <br>  <br>The **view** parameter allows SceneGraph rendering of ads into an app that uses SceneGraph for content rendering.  <br><br>*   For server-stitched use case, this should be the Video node of the content player.<br>*   For non-stitched use cases, this can be any renderable node in the scene whose lifetime is guaranteed during the duration of ad rendering. Render any ads scheduled for display.<br><br>  <br>The dimensions of the view object will be used to position RAF's UI elements, so it must be properly sized. Having dimensions larger than the current video playback resolution can place RAF UI elements such as the progress bar off screen. |

#### Return Value

A flag indicating whether the ad pod was rendered to completion. This will be false if the user exited before render completion.

Configuration
-------------

### setAdUrl(url as String)

#### Description

Sets the ad URL to be used for a new [getAds()](/docs/developer-program/advertising/raf-api.md#client-ad-insertion) request.

> You can only receive payment for ads shown in your application when the Roku Ad Framework is properly configured with a valid URL assigned by your ad service or by Roku.
> 
> Please contact adsupport@roku.com to discuss monetization options and obtain an ad URL if you wish to use Roku to fill ad inventory in your application.
> 
> Using the default URL is useful only for development and testing purposes and you will not receive payment for ad impressions from the default URL.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| url | String | The URL to be set as the current ad service request. Since version 2.14, supports parsing ads from local xml files in tmp:/, e.g., raf.setAdURL("tmp:/myVASTorVMAPorSMRX.xml").  <br>  <br>To use the default Roku ad service, call the **setAdUrl()** method without the url parameter. |

### getAdUrl() as String

#### Description

Gets the currently-configured ad server URL, or the default Roku ad server URL if none has been configured.

#### Return Value

The current ad server URL.

### setAdPrefs(useRokuAdsAsFallback as Boolean, maxRequests as Integer)

#### Description

Configures general ad request preferences.

The default is for Roku to backfill ads if this method is not called or **useRokuAdsAsFallback** is not set to false

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| useRokuAdsAsFallback  <br>  <br>_Deprecated_ | Boolean | Indicates whether the default Roku backfill ad service URL should be used in case the client-configured URL fails to return any renderable ads.  <br>  <br>_This parameter has been deprecated and will be ignored in future updates to the RAF library._ |
| maxRequests | Integer | The maximum number of attempts the [getAds()](#getadsmsg-as-string-as-object) function is allowed to make. For example, if the first attempt to the client-configured URL fails to return any renderable ads and this field is set to 2, and the **useRokuAdsAsFallback** field is set to false, then a second attempt is made to the same client-configured URL. |

### setAdConstraints(maxHeight as Integer, maxWidth as Integer, maxBitrate as Integer, supportedMimeTypes as Object)

#### Description

Configures media constraints to filter renderable video ads.

By default, the MIME types are configured for “video/mp4”, “video/mp4-h264”, “video/x-mp4”, “application/x-mpegurl”, and “application/json”.

Any additional known types can be mapped to their stream format by setting this parameter before calling [getAds()](/docs/developer-program/advertising/raf-api.md#client-ad-insertion).

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| maxHeight | Integer | Maximum vertical dimension of renderable ad (in pixels). |
| maxWidth | Integer | Maximum horizontal dimension of renderable ad (in pixels). |
| maxBitrate | Integer | Maximum allowable bitrate for renderable ad streams (in Kbps) |
| supportedMimeTypes | Object | Associative array with entries of the form:  <br>  <br>`{“mimeType” : “stream- Format”}` |

### setAdBreaks(contentLength as Integer, adBreakTimes as Integer)

#### Description

Configures content playback parameters, which can be used for scheduling relative-positioned ad breaks in VMAP ad service responses.

*   If your application uses VMAP ad URLs and they are configured to use “nn%” timeOffset values, then you must specify the contentLength prior to calling [getAds()](/docs/developer-program/advertising/raf-api.md#client-ad-insertion).
*   If VMAP is configured to use “#mm” timeOffset values, you must first specify a set of ad break times.
*   Calling with empty parameters will reset these to invalid values.

The content length can also be set independently via [setContentLength()](/docs/developer-program/advertising/raf-api.md#setcontentlengthlength-as-integer) if ad break times are not required.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| contentLength | Integer | Total length of video content (in seconds). |
| adBreakTimes | Integer | Array of suggested offsets into content playback to insert ad breaks (in seconds). |

### setAdExit(enabled as Boolean)

**Note: setAdExit() is deprecated and disabled - check showAds() return value instead**

~\#### Description~

~The default behavior is to enable exiting during ad rendering (for example, via the “Back” button) to return to content selection screen in the application.~

~Some use cases may require disabling this behavior if the user should not be allowed to skip ads when there is no applicable content selection mechanism.~

~\#### Parameters~

~| Argument | Type | Description |~ ~| -------- | ------ | ------------------------------------------------------------ |~ ~| enabled | Boolean | Enables ad exit behavior during rendering. |~

### importAds(adPodArray as Object)

#### Description

Resets the internal ad pod cache to allow client code to import a set of ads from unsupported ad service response formats or when aggregating ads from multiple ad services.

The application is responsible for ensuring that the ad pods in the array contain all the required data members.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| adPodArray | Object | Array of ad pods structured in accordance with the required [Ad Structure](/docs/developer-program/advertising/integrating-roku-advertising-framework.md#ad-structure). |

### enableJITPods(enabled as Boolean)

_Available since version 2.4_

#### Description

For applications that use a VMAP or SmartXML ad response to structure multiple ad pods, including midrolls, the JIT (or “Just In Time”) feature can be used to avoid pre-fetching all ad metadata before the content playback begins.

When enabled, ad call redirects for midrolls are deferred until a certain time before the ad pod is rendered. This mechanism relies on the host app’s continuous use of the BrightScript **getAds()** API method with the content video position event to determine when to resolve the deferred ads.

> JIT is used as a global setting; if the app has mixed content streams, where some content should not use JIT (such as server-stitched ads), then the host app is responsible for disabling this functionality before any ad calls are made for such streams.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| enabled | Boolean | Enables “Just In Time” fetching of midroll ads. By default, JIT is disabled and must be explicitly enabled via this API. |

### enableInPodStitching(isIPS as Boolean)

_Available since version 2.14_

#### Description

"In-pod stitching" (IPS) mode brings some of the benefits from [CSAS API](/docs/developer-program/advertising/csas.md) to apps using the classic CSAI API [showAds()](/docs/developer-program/advertising/raf-api.md#showadsads-as-object-ctx-as-object-view-as-object-as-boolean). When IPS mode is enabled and _showAds()_ is called for an ad break with multiple ads, it would stitch together the video clips for playback, prebuffering the next ad in the background while the current ad is finishing. The viewer experience is better because of the fast transitions between ads. Conversely, when IPS is disabled, each video plays individually and a few seconds are spent in a buffering screen between the ads.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| isIPS | Boolean | Whether to enable IPS (in-pod stitching) for showAds() |

### setLimitAdTracking(enabled as Boolean)

_Available since version 3.1_

For apps that collect explicit in-app consent for ad targeting (for example, to adhere to GDPR), this function specifies the value of the [ROKU\_ADS\_LIMIT\_TRACKING URL parameter macro](/docs/developer-program/advertising/integrating-roku-advertising-framework.md#url-parameter-macros) to be passed into beacons and ad requests.

This function cannot override the ROKU\_ADS\_LIMIT\_TRACKING value if the customer has cleared the **Personalize ads** check box in the **Settings > Privacy** menu.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| enabled | Boolean | Sets the [ROKU\_ADS\_LIMIT\_TRACKING URL parameter macro](/docs/developer-program/advertising/integrating-roku-advertising-framework.md#url-parameter-macros) to be passed into beacons and ad requests to either 1 (true; ad targeting is disabled for the customer) or 0 (false; ad targeting is disabled for the customer). |

### setTrackingCallback(callback as Function, obj as Object)

#### Description

Allows library client to set a callback function to be called when ad tracking events are fired or checked.

Callback functions must have the following signature:

    Sub CallbackFunc(obj = Invalid as Dynamic, eventType = Invalid as Dynamic, ctx = Invalid as Dynamic)
    

*   The obj parameter is an opaque object always passed through to the callback.
    
*   The eventType, if set, is a string specifying a tracking event that is fired. Event names correspond to [Tracking](/docs/developer-program/advertising/integrating-roku-advertising-framework.md#tracking-events).
    
*   The ctx is an optional associative array that encapsulates metadata associated with VAST-specified macros or ad render progress. Each member of the ctx array should separately be considered optional (for example, client code should check for valid values before operating on these data members). Generally, if `ctx.eventType` is not set, then `ctx.time` should be set and indicate ad render progress:
    

      {
          errType: String,
          errCode: String,
          errMsg : String,
          time : Int | Float (playback position, in s),
          url : String (rendered asset URI),
          ad : Associative Array representing ad structure for current ad,
          adIndex: Int (logical index of current ad within ad pod)
      }
    

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| callback | Function | Function matching the required function signature . |
| obj | Object | The object to be passed to the callback function. |

### setDebugOutput(enabled as Boolean)

#### Description

Enables a library client to configure extended debug output, which is disabled by default.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| enabled | Boolean | Enables extended debug logging. |

### getLibVersion() as String

#### Description

Gets the RAF library version.

#### Return Value

The library version in the following format: “`<major>.<minor>`”

General audience measurement
----------------------------

### enableAdMeasurements(enabled)

_Available since version 2.1_

#### Description

Applications using audience measurement features must explicitly enable the framework to operate on the custom impression tag parameters. This function is used in conjunction with the [setContentGenre()](/docs/developer-program/advertising/raf-api.md#setcontentgenregenres-as-string-kidscontent-as-boolean), [setContentId()](/docs/developer-program/advertising/raf-api.md#setcontentidid-as-string), and [setContentLength()](/docs/developer-program/advertising/raf-api.md#setcontentlengthlength-as-integer) APIs to provide measurement data to third-party ad measurement platforms such as NielsenDAR, ComScore CCR, and ComScore VCE.

> Contact [adsupport@roku.com](mailto:adsupport@roku.com) for more information on how to use audience measurement features.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| enabled | Boolean | Enables audience identifiers in measurement tags. |

### setContentGenre(genres as String, kidsContent as Boolean)

#### Description

Enables potential ad targeting by specifying a set of genre tags to associate with the content or the ad request.

To clear genre tags, pass an empty string in the **genres** parameter or omit it.

The semantics and implementation of targeting based on genre values are dependent on the configured ad server, but for a list of currently-supported tags supported by the Roku ad server, see [Roku Genre Tags](/docs/developer-program/advertising/integrating-roku-advertising-framework.md#roku-genre-tags).

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| genres | String | Comma-delimited string or array of genre tag strings. |
| kidsContent | Boolean | Optional. Specify whether content is targeted towards children (true) or not (false).  <br><br>> Per Roku's [certification requirements](https://developer.roku.com/docs/developer-program/certification/certification.md#1-advertising), apps with child-directed content must set this flag to **true** if serving ads during child-directed content. |

### setContentId(id as String)

#### Description

Enables potential ad targeting on a video content item by specifying its identifier.

Passing an empty string or omitting the **id** parameter will clear the content ID.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| id  | string | The content video item on which ad targeting may potentially be allowed. |

### setContentLength(length as Integer)

#### Description

Configures the content length to extend ad targeting properties for Nielsen DAR.

This method may also be used to determine VMAP relative ad break times.

Omitting the **length** parameter will clear any content length that was previously set.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| length | integer | Total length of content (in seconds). |

Nielsen DAR
-----------

> The Nielsen DAR APIs have been deprecated. Use the [general audience measurement APIs instead](#general-audience-measurement).

### setNielsenGenre(genre as String)

#### Description

Enables ad campaign measurement using Nielsen DAR tags by specifying a primary genre for the content being played, according to the Nielsen genres defined in [Nielsen DAR Genre Tags](/docs/developer-program/advertising/integrating-roku-advertising-framework.md#nielsen-dar-genre-tags).

**Examples**:

“CS” for a “Seinfeld” episode. “N” for a “60 Minutes” episode.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| genre | String | The primary content genre to be passed into Nielsen DAR tags. |

### setNielsenAppId(id as String)

Enables ad campaign measurement using Nielsen DAR tags.

The value of this application ID is uniquely assigned to your application by Nielsen and must be configured before rendering any ads containing Nielsen beacons.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| id  | String | The Nielsen-assigned application ID. |

Nielsen DCR
-----------

### getNielsenContentData() as String

#### Description

Provides an encrypted Nielsen RIDA parameter string for apps using the Nielsen SDK for DCR measurements.

#### Return Value

Encrypted Nielsen RIDA parameter string.

Client stitched ads
-------------------

### constructStitchedStream(contentMetaData as Object, ads as Object) as Object

#### Description

Merges a video feed and a set of one or more ad pods into a single playlist for playback via the [renderStitchedStream()](#renderstitchedstreamcsasstream-as-object-view-as-object-as-boolean) function.

#### Parameters

| Argument | Type | Required? | Description |
| --- | --- | --- | --- |
| contentMetaData | Content Node | Required | The content metadata of the video feed to be combined into the stitched stream. |
| ads | roArray | Required | Array of ad breaks to be combined into the stitched stream using RAF's [ad structure](https://developer.roku.com/docs/developer-program/advertising/integrating-roku-advertising-framework.md#ad-structure) format.  <br>  <br>The object may been parsed earlier from VMAP/SMRX by calling  <br>`raf.setAdURL(myAdTag): adBreaks = raf.getAds()`. |

#### Return Value

A single video stream containing the specified video feed and ads.

### renderStitchedStream(csasStream as Object, view as Object) as Boolean

#### Description

Renders a video stream that uses client-side ad stitching.

Tracking events are triggered automatically during ad rendering by this method.For client applications that perform their own ad rendering, the valid event types that must be handled are represented in the `tracking` array of the [Ad Structure](/docs/developer-program/advertising/integrating-roku-advertising-framework.md#ad-structure).

For client-side stitched streams, the app will also get tracking events during content playback in addition to those received during ad rendering.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| csasStream | Object | The video stream returned by [constructStitchedStream()](#constructstitchedstreamcontentmetadata-as-object-ads-as-object-as-object) method. |
| view | Object | A renderable node to which the ad UI can be attached. This enables the rendering of ads for apps that use SceneGraph for content rendering.  <br>  <br>The dimensions of the view object will be used to position RAF's UI elements, so it must be properly sized. Having dimensions larger than the current video playback resolution can place RAF UI elements such as the progress bar off screen. |

#### Return Value

A flag indicating whether the stream played to completion. This is false if the user exited playback before the stream completed.

Server stitched ads
-------------------

### stitchedAdsInIt(adPodArray as roArray)

#### Description

Imports ad metadata to be used for server-stitched ad rendering and resets the internal state before handling events.

The application is responsible for ensuring that the ad pods in the array contain all the required data members. In particular, for server-stitched ads, all time-dependent tracking beacons (Impression and quartile beacons) must have a valid time data member set, with a value relative to the entire stitched stream. For example, if a 30-second ad starts at 10:00 within the stitched stream, its Impression beacons should have track.time = 600.0 and its Midpoint beacons should have track.time = 615.0, and so on.

This method is used in conjunction with [stitchedAdHandledEvent()](/docs/developer-program/advertising/raf-api.md#server-stitched-ads) to implement ad rendering within server-stitched video streams.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| adPodArray | roArray | Set of ad pods structured in accordance with the required [Ad Structure](/docs/developer-program/advertising/integrating-roku-advertising-framework.md#ad-structure). |

### stitchedAdHandledEvent(msg as Object, player as Object) as roAssociativeArray

#### Description

Determines whether a stitched ad is being rendered, lets the ad renderer attempt to handle the event, and returns metadata about the ad and the event handled state.

This method is only intended for use in rendering server-stitched ads.

The advertising framework must first be initialized using the stitchedAdsInit() method before calling this method.

#### Parameters

| **Argument** | **Type** | **Description** |
| --- | --- | --- |
| player | Object | Player interface to allow ad renderer to control stitched video stream. If invalid or not specified, only beacons will be fired, and no interaction will be allowed or additional UI rendered during ad display.  <br>  <br>This parameter may be either the roVideoPlayer instance used to play the stitched stream, or an roAssociativeArray that contains methods congruent to the ifVideoPlayer interface. The roAssociativeArray is used in case there is additional client code that should be executed when an ad renderer controls the stream (for example, analytics).  <br>  <br>If the player parameter is passed as an roAssociativeArray in an app where video is played with roVideoPlayer (non-RSG), the app must contain the following methods:<br><br>    {<br>      ' Returns message port for player<br>      GetMessagePort : Function() as Object,<br>    <br>      ' Pauses a stitched video stream<br>      Pause : Function() as Boolean,<br>    <br>      ' Resumes a paused stitched stream<br>      Resume : Function() as Boolean,<br>    <br>      ' Seeks to absolute position (in ms) within stream<br>      Seek : Function(offsetMs as Integer) as Boolean,<br>    <br>      ' Plays stitched video stream<br>      Play : Function() as Boolean,<br>    <br>      ' Stops stitched video stream<br>      Stop : Function() as Boolean<br>    }<br>    <br><br>  <br>  <br>For SceneGraph apps that use a Video node for stitched ad playback, the **player** parameter should be an roAssociativeArray of the following form:<br><br>    {<br>      sgNode : video, ' the video node which will render the stitched stream<br>      port : port ' the message port on which (at least) the "position" and "state" fields of<br>      the above video node are observed<br>    } |
| msg | Object | Returned object from a Wait() call on the message port used by the stitched video player. May be consumed by the ad renderer to measure playback state or provide user interactivity with stitched ad. |

#### Return Value

*   If a stitched ad is being rendered, this method returns an roAssociativeArray that represents the current ad context and state. The return value is of the form:
    
          {
              adIndex : Integer, 'Index of current ad within pod
              adPodIndex : Integer, 'Index of current pod
              evtHandled : Boolean, 'True if event was handled by ad renderer
              adExited : Boolean, 'True if user exited ad rendering
              adCompleted : Boolean, 'True if ad has completed rendering
          }
        
    

*   If no stitched ad is being rendered, this method returns **Invalid**.

*   If the return value indicates that there is a stitched ad being rendered and that the event was handled by the renderer, the client application must take no action on that event. If the ad was exited, the client app should stop playback and return to the content selection screen.

Buffer screen customization
---------------------------

### setAdBufferScreenContent(contentMetaData as Object)

#### Description

Enables the client application to set metadata for the content populating the default ad buffer screen. contentMetaData conforms to the format defined in [Content Meta-Data](/docs/developer-program/getting-started/architecture/content-metadata.md) and can contain any or all of the following:

    {
      HDBackgroundImageUrl : String (URL for HD background image),
      SDBackgroundImageUrl : String (URL for SD background image),
      HDPosterUrl : String (URL for HD video poster),
      SDPosterUrl : String (URL for SD video poster),
      Title : String (Content title),
      Description : String (Content description)
    }
    

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| contentMetaData | roAssociativeArray | Contains metadata representing information to be displayed in the default ad buffer screen. |

### enableAdBufferMessaging(enableMsg as Boolean, enableProgressBar as Boolean)

#### Description

Enables the client application to display messaging text and a progress bar on the default ad buffer screen.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| enableMsg | Boolean | Enables ad messaging text on the default ad buffer screen. The default value is true. |
| enableProgressBar | Boolean | Enables an ad buffering progress bar on the default ad buffer screen. The default value is true. |

### setAdBufferScreenLayer(zOrder as Integer, contentMetaData as Object)

#### Description

Enables the client application to set individual layer metadata for the custom ad buffer UI. contentMetaData conforms to the format defined in [Content Meta-Data](/docs/developer-program/getting-started/architecture/content-metadata.md).

The values that can be passed in the **zOrder** and **contentMetaData** parameters are specified by roImageCanvas.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| zOrder | Integer | Layer index to be used to display the **contentMetaData**. |
| contentMetaData | roAssociativeArray | The metadata for this UI layer. |

### clearAdBufferScreenLayers()

#### Description

Enables the client application to clear all metadata in all layers previously set for the custom buffer screen.

### setAdBufferRenderCallback(callback as Function, obj as Object, timeout as Integer)

#### Description

Enables the client application to set a callback function and timeout value for ad buffering events, to provide opportunity for analytics methods or animation of elements on custom buffer screen.

#### Parameters

| Argument | Type | Description |
| --- | --- | --- |
| callback | Function | The callback function to receive ad buffer events. The default value is Invalid. This function must have the following signature:  <br>`Function(obj as Dynamic, eventType as String, ctx as Dynamic) as Void`  <br>  <br>The **eventType** parameter can take the following values:  <br><br>*   BufferingStart<br>*   BufferingEnd<br>*   ReBufferingStart<br>*   ReBufferingEnd<br>*   Progress<br>*   Timeout<br><br>  <br>The **ctx** parameter is an roAssociativeArray that can contain the following:<br><br>    {<br>        'array of content metadata set via setAdBufferScreenLayer, or Invalid<br>         canvasLayers : roArray of roAssociativeArrays,<br>    <br>        'progress percentage [0-100]. Optional, only for "Progress" event type<br>         progress : Integer<br>    <br>        'ad metadata for currently buffering ad<br>        ad : roAssociativeArray,<br>    <br>        'index of current ad within pod<br>        adIndex : Integer<br>    } |
| obj | Object | The object to be passed to the callback function. The default value is Invalid. |
| timeout | Integer | The number of milliseconds to wait on buffer events before timing out. The default value is 0 (no timeout). |