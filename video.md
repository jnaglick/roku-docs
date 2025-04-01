Video
=====

Extends [**Group**](/docs/references/scenegraph/layout-group-nodes/group.md)

The Video node class provides a controlled play of live or VOD video.

The Video node includes a wide variety of internal nodes to support trick play, playback buffering indicators, and so forth. Playback buffering indicators, to indicate buffering before initial playback as well as re-buffering, use an internal instance of a ProgressBar node. For trick play, an internal instance of a TrickPlayBar node is provided. For display of BIF images for DVD-like chapter selection, an internal instance of a BIFDisplay node is provided.

Starting from Roku OS 8, the behavior of the Roku system overlay is such that the system overlay now slides in whenever the \* button is pressed, the Video node is in focus, and the app does not have its OnKeyEvent() handler fired. When the Video node is not in focus, the system overlay does not slide in and the OnKeyEvent() handler is fired.

Fields
------

### Playback fields

To set the specific video playback parameters for a particular video, set the [Content Meta-Data](/docs/developer-program/getting-started/architecture/content-metadata.md) attributes for the video in a [ContentNode](/docs/references/scenegraph/control-nodes/contentnode.md) node, and assign the ContentNode to the `content` field of the Video node.

Video playback can then be controlled by setting the value of the `control` field, such as setting the field value to `play` to begin playback.

The `control` field includes a `prebuffer` option, which allows the video to begin buffering without showing the video. You can use this option to begin buffering of a video before a user has actually selected and started the video, such as when the user is looking at information about various video offerings in a list or grid or another type of UI element. This can eliminate much or all of the apparent delay in starting the video due to buffering the video for the user. For example, you could set the `control` field value to `prebuffer` in a callback function triggered by the `itemFocused` events that occur as a user scrolls down a list of video offerings that also display information about each video. When the user makes the selection, you can begin the actual video playback by setting the `control` field value to `play` in a callback function triggered by the `itemSelected` event for the list.

FieldTypeDefaultAccess PermissionDescriptioncontentContentNodeNULLREAD\_WRITEThe ContentNode with the [Content Meta-Data](/docs/developer-program/getting-started/architecture/content-metadata.md) for the video, or a video playlist (a sequence of videos) to be played.  
  
If a video playlist is to be played, the children of this ContentNode comprise the playlist, and each ContentNode child must have all attributes required to play that video. For example, if the videos "A" and "B" are to be played, three ContentNodes must be created: the parent ContentNode (which is largely ignored), one ContentNode child for "A," and one ContentNode child for "B." The parent node is set into this content field, and when video playback is started, all of its children will be played in sequence. Any changes made to the playlist after playback has started are ignored. See the `contentIsPlaylist` and `contentIndex` fields, for more information on playlists.playStartInforoAssociativeArrayREAD\_ONLYProvides timing measurements related to the start of video playback. All measurements are in seconds.  
  
The roAssociativeArray contains the following fields:  
  

| Field | Type | Access Permission | Description |
| --- | --- | --- | --- |
| total\_dur | float | READ\_ONLY | Total video start duration. |
| manifest\_dur | float | READ\_ONLY | Manifest download and parsing. |
| drm\_load\_dur | float | READ\_ONLY | DRM system initialization. |
| drm\_lic\_acq\_dur | float | READ\_ONLY | License acquisition. This typically includes interactions with the license server. |
| prebuf\_dur | float | READ\_ONLY | Prebuffer content. |
| manifest\_start | Float | READ\_ONLY | Point at which manifest download and parsing begins. |
| drm\_load\_start | Float | READ\_ONLY | Point at which DRM system initialization begins. |
| drm\_lic\_acq\_start | Float | READ\_ONLY | Point at which license acquisition begins. |
| prebuf\_start | Float | READ\_ONLY | Point at which content pre-buffering begins. |

  

> The \_start fields correspond to the similarly named \_dur (duration) fields in this structure. In each case, the \_start point is the number of milliseconds elapsed from the initialization of the media player (t=0.000). If required, ending points for each interval can be derived from its associated starting-point and duration.

licenseStatusroAssociativeArrayREAD\_ONLIndicates whether the DRM license was acquired. If a failure occurs, this field provides additional details about the error. The roAssociativeArray contains the following fields:  
  

| Key | Type | Value |
| --- | --- | --- |
| response | string | The server response. If a license is not retrieved, the response is empty and the HTTP response code is returned instead. |
| status | string | The HTTP response code. |
| keysystem | string | The DRM technology used. |
| duration | string | The total time elapsed in sending a request to the license server and receiving a response (in milliseconds). |

  
contentIsPlaylistBooleanfalseREAD\_WRITEIf set to true, enables video playlists (a sequence of videos to be played). See the `content` and `contentIndex` field for more information on playlists.contentIndexinteger\-1READ\_ONLYThe index of the video in the video playlist that is currently playing. Generally, you would only want to check this field if video playlists are enabled (by setting the `contentIsPlaylist` field to true), but it is set to 0 when a single video is playing, and video playlists are not enabled.nextContentIndexinteger\-1READ\_WRITEIf the `contentIsPlaylist` field is set to true to enable video playlists, sets the index of the next video in the playlist to be played. Setting this field does not immediately change the video being played, but takes effect when the current video is completed or skipped. By default, this value is -1, which performs the default index increment operation. After the video specified by the index in this field begins playing, the field is set to the default -1 again, so the next video played will be set by the default index increment operation unless the field is set again to a different index.controloption stringnoneREAD\_WRITESets the desired play state for the video, such as starting or stopping the video play. Getting the value of this field returns the most recent value set, or none if no value has been set. To dynamically monitor the actual state of the video, see the `state` field.  
  
The play and stop commands to commence and discontinue playback should not be used to implement trick modes like rewind, or replay. For that use the `seek` field.  
  

| Option | Effect |
| --- | --- |
| none | No play state set |
| play | Start video play |
| stop | Stop video play |
| pause | Pause video play |
| resume | Resume video play after a pause |
| replay | Replay video |
| prebuffer | Starts buffering the video stream before the Video node actually begins playback. Only one video stream can be buffering in the application at any time. Setting the `control` field to `prebuffer` for another video stream after setting `prebuffer` for a previous video stream stops the buffering of the previous video stream. |
| skipcontent | Skip the currently-playing content and begin playing the next content in the playlist. If the content is not a playlist, or if the current content is the end of the playlist, this will end playback. |

asyncStopSemantics  
  
_Available since Roku OS 12.5_booleanfalseWRITEIndicates whether the "STOP" command is executed asynchronously (true) or synchronously (false).  
  
By default, the STOP command is executed synchronously, which blocks the UI thread. Enabling this field makes the STOP command non-blocking, which enables the video to be switched faster.  
  
When this field is enabled, the `state` field is set to "stopping" when the asynnchronous stop begins. The `state` field then changes to "stopped" once the stop has been completed.  
  
Any other media player component calls on the UI thread that require the Video node to be re-instantiated should be blocked until the asynnchronous stop has been completed (for example, updating the `control` field to "Play" or "Prebuffer" or updating the `seek` field). This is because a video node in the "stopping" state is still using the underlying media player, which is not available at that time. As a result, performing these types of operations on a different video while in the "stopping" state may result in a playback failure.statevalue stringnoneREAD\_ONLYDescribes the current video play state, such as if the video play has been paused.  
  

| Value | Meaning |
| --- | --- |
| none | No current play state |
| buffering | Video stream is currently buffering |
| playing | Video is currently playing |
| paused | Video is currently paused |
| stopping  <br>  <br>_Available since Roku OS 12.5_ | Video is in the process of being stopped. This value is only returned if the `asyncStopSemantics` field is enabled. |
| stopped | Video is currently stopped |
| finished | Video has successfully completed playback |
| error | An error has occurred in the video play. The error code, message, and diagnostics can be found in the `errorCode`, `errorMsg`, and `errorStr` fields respectively. |

errorCodeinteger0READ\_ONLYThe error code associated with the video play error set in the `state` field:

*   0 no error
*   \-1 network error (server down or unresponsive, server is unreachable, network setup problem on the client).
*   \-2 connection timed out
*   \-3 unknown/unspecified or generic Error
*   \-4 empty list; no streams were specified to play
*   \-5 media error; the media format is unknown or unsupported
*   \-6 DRM error

  
Use the **errorStr** and and **errorInfo** fields for more descriptive diagnostic information to help identify and resolve the cause of the error.errorMsgstringREAD\_ONLYAn error message describing the video play error set in the `state` field.  
  
Use the **errorStr** and and **errorInfo** fields for more descriptive diagnostic information to help identify and resolve the cause of the error.errorStrstringREAD\_ONLYA diagnostic message to help resolve the video play error set in the `state` field.  
  
The format of the errorStr is as follows: category:{category\_name}:error:{error\_code}:ignored:{0|1}:{source}:{source\_name}:{additional catcher comment}:{error\_string}:extra:{error\_attributes}  
  

| errorStr Field | Type | Description |
| --- | --- | --- |
| category\_name | string | The type of error, which includes: "http", "drm", "mediaerror", or "mediaplayer". |
| error\_code | integer | The unique code associated with the error. |
| ignored | integer | Indicates whether the error generated an exception (0) or was ignored resulting in the next item in the content list being played (1). |
| source | string | The module that generated the error. |
| source\_name | string | The module that generated the error. |
| additional catcher comment | string | Typically, the comment added when the exception is caught. |
| error\_string | string | A text message describing the video play error. |
| error\_attributes | string | The error attribute, which includes the clipId (the unique ID of the clip that failed to play). |

errorInforoAssociativeArrayREAD\_ONLYA diagnostic message to help resolve the video play error set in the `state` field.  
  
The roAssociativeArray contains the following fields:  
  

| Field | Type | Description |
| --- | --- | --- |
| clipId | integer | The unique ID for the clip |
| ignored | integer | Indicates whether the error generated an exception (0) or was ignored resulting in the next item in the content list being played (1). |
| source | string | The module that generated the error. |
| category | String | The type of error, which includes: "http", "drm", "mediaerror", or "mediaplayer". |
| errcode | integer | The internal Roku code associated with the error. Use the **dbgmsg** field for debugging. |
| dbgmsg | string | A verbose debug message that can help identify the root cause of the error. |
| drmerrcode | integer | The error code returned by the DRM system, if any, when a video player error occurs |

decoderStatsroAssociativeArray{}READ\_ONLYProvides the following video decoder statistics related to the start of video playback:  
  

| Key | Type | Value |
| --- | --- | --- |
| renderCount | integer | The number of frames that have been rendered since playback was started. This value is incremented each time a new frame is rendered |
| repeatCount | integer | The number of frames that have been repeated since playback was started.This value is incremented each time a new frame is not available in time and the current frame is rendered an additional frame period. |
| frameDropCount | integer | The number of frames that have been dropped since playback was started.This value is incremented each time the presentation time of a decoded frame is too old to be rendered and the next frame is rendered instead. |
| streamErrorCount | integer | The number of bitstream errors since playback was started.This value is incremented each time the decoder detects a bitstream error. |

  
  
Set the **enableDecoderStats** field to true to enable updates to this field.enableDecoderStatsbooleanfalseREAD\_WRITEEnables updates to the **decoderStats** field.playbackActionButtonsroArray of roAssociativeArrays\[\]READ\_WRITEComponent that shows the buttons and other specified UI elements on the pause screen at the start of playback. Each element in the array has following fields:  
  

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| text | string | system default | Text for the button label |
| icon | uri | ""  | A 9-patch or PNG of the icon to be displayed when the button does not have. |
| focusIcon | uri | ""  | A 9-patch or PNG of the icon to be displayed when the button has focus. |
| buttonIsDisabled | Boolean | false | Controls whether the button is disabled (true) or enabled (false). A disabled button is skipped and does not have focus while the user navigates the different playback action buttons with the directional pad on the Roku remote control. |

playbackActionButtonSelectedinteger0READ\_WRITEThe index of the button that is selected in the **playbackActionButtons** field.playbackActionButtonFocusedinteger0READ\_WRITEThe index of the button that has focus in the **playbackActionButtons** field.playbackActionButtonFocusedTextFontFontSmallBoldSystemFontWRITESpecifies the font of the button label when the button has key focus.playbackActionButtonUnfocusedTextFontFontSmallSystemFontWRITESpecifies the font of the button label when the button does not have key focus.playbackActionButtonFocusedTextColorColorOX121212FFWRITESpecifies the color of the button label text when the button has key focus.playbackActionButtonUnfocusedTextColorColor0xEFEFEFFFWRITESpecifies the color of the button label text when the button does not have key focus.playbackActionButtonFocusIndicatorBlendColorColor\-WRITESpecifies the button background color when the button has key focus.subtitleSelectionPreferences  
  
(_Available since Roku OS 12.5_)roAssociativeArray{ }WRITE\_ONLYThe significance and priority order of the attributes and values for the subtitle tracks available in the video stream.  
  
Provide the attribute fields from highest to lowest significance (for example, if **language** should take precedence over all other attributes, list it first). For the subtitle track languages, provide the language codes from highest to lowest priority (for example, if Spanish for Latin America and the Caribbean \["es-419"\] has precedence over Spanish \["es"\], list the language codes in the following order: \["es-419", "es"\].  
FieldTypeDescriptionvaluesroAssociativeArraySpecify values for the following subtitle track attributes. List the attributes from highest to lowest significance.  

| Field | Type | Description |
| --- | --- | --- |
| language | array of Strings | A list of language (ISO-639)/country (ISO-3166) codes for the subtitles. List the language codes in priority order (highest to lowest). |
| caption | array of Strings | A flag indicating whether captions exist for the video stream. This is equivalent to the HLS "public.accessibility.transcribes-spoken-dialog" characteristic. |
| descriptive | array of Strings | A flag indicating whether descriptives exist for the music and sounds in the video stream. This is equivalent to the HLS "public.accessibility.describes-music-and-sound" characteristic. |
| easyReader | array of Strings | A flag indicating whether subtitles are easy to read. |

overrideSystembooleanSpecify whether to use the app's preferences over the system preferences (true) or use the app's preferences only when the system preferences do not match any of the available tracks (false), which allows the app to provide additional rules in this case. The default value is false.  
  
**Example**  

    video.subtitleSelectionPreferences = { values: [
        { language: ["es-419", "es", "es-*", "fr", "en-US", "en-UK", "en"] },
        { caption: "true" },
        { descriptive: ["false"] },
        { easyReader: "true" } ],
        overrideSystem: true }
    

  
**Explanation**  
  
The subititle language with the highest priority is "es" with a country code of "419". The next highest priority language is "es" with no country code, and then "es" with any country code.audioSelectionPreferences  
  
(_Available since Roku OS 12.5_)roAssociativeArray{ }WRITE\_ONLYThe significance and priority order of the attributes and values for the audio tracks available in the video stream.  
  

> A language matching any country code does not match a track that specifies the same language but with no country code.

  
Provide the attribute fields from highest to lowest significance (for example, if the **language** should take precedence over the **description**, list **language** first. For the audio track languages, provide the language code values from highest to lowest priority (for example, if English for the United States \["en-US"\] has precedence over English for the United Kingdom \["en-UK"\], list the language codes in the following order: \["en-US", "en-UK"\].  
  
FieldTypeDescriptionvaluesroAssociativeArraySpecify values for the following audio track attributes. List the attributes from highest to lowest significance.  

| Field | Type | Description |
| --- | --- | --- |
| language | array of Strings | A list of (ISO-639)/country (ISO-3166) codes for the audio track. List the languages in priority order (highest to lowest). |
| descriptive | array of Strings | A flag indicating whether descriptives exist for the video playing in the stream. This is equivalent to the HLS "public.accessibility.describes-video" characteristic. |

overrideSystembooleanSpecify whether to use the app's preferences over the system preferences (true) or use the app's preferences only when the system preferences do not match any of the available tracks (false), which allows the app to provide additional rules in this case. The default value is false.  
  
**Example**  

    video.audioSelectionPreferences = { values: [
        { language: ["en-US", "en-UK", "en", "en-*"] },
        { descriptive: "false" } ],
        overrideSystem: true }
    

  
**Explanation**  
  
The audio language with the highest priority is "en-US". The next highest priority language is "en-UK", then "en" with no country code, and finally "en" with any country code.

### Trickplay fields

FieldTypeDefaultAccess PermissionDescriptiondurationtime0READ\_ONLYThe duration of the video being played, specified in seconds. This becomes valid when playback begins and may change if the video is dynamic content, such as a live event.loopBooleanfalseREAD\_WRITEIf set to true, the video or video playlist (if the `contentIsPlaylist` field is set to true to enable video playlists) will be restarted from the beginning after the end is reached.positiontimeinvalidREAD\_ONLYTime of the current position in the stream. Either UTC time or elapsed since start of stream depending on content type.  
  
As of Roku OS 9.3, when the video is paused, the position is recorded for that pause event. This means that playing, pausing, and resuming a video generates three separate positions.positionInforoAssociativeArrayinvalidREAD\_ONLYContains the following fields that provide information about the last rendered video and audio samples.  
  

| Field | Type | Default | Access Permission | Description |
| --- | --- | --- | --- | --- |
| audio | double | invalid | READ\_ONLY | Position of the last rendered audio sample, specified in seconds |
| clip\_id | integer | invalid | READ\_ONLY | The unique ID of the clip |
| epoch | integer | invalid | READ\_ONLY | 0 means positions are relative to videoStart; 1 means that positions are utc |
| video | double | invalid | READ\_ONLY | Position of the last rendered video sample, specified in seconds |

clipIdinteger0READ\_ONLYThe clip ID of the currently playing track.notificationIntervaltime0.5READ\_WRITEThe interval between notifications to observers of the position field, specified as the number of seconds. If the value is 0, no notifications are delivered. This value may be read or modified at any time.seektimeinvalidWRITE\_ONLYSets the current position in the video. The value is the number seconds from the beginning of the stream, specified as a double.seekModestring"default"READ\_WRITEDetermines the desired level of accuracy for seek behavior:  

| Value | Meaning |
| --- | --- |
| default | Seek to the closest sync frame (segment, or I-frame of a multi-frame segment) that is earlier than the requested seek time. |
| accurate | Seek to the exact time requested if platform support (video decoder step function) is available. |

  
autoplayAfterSeekbooleantrueREAD\_WRITEEnables video content to automatically play after rebuffering. Setting this flag to false disables this default behavior.timedMetaDataassociative array{ }READ\_ONLYThe most recent timed meta data that has been decoded from the video stream. Only meta data with a key that matches an entry in timedMetaDataSelectionKeys will be set into this field. The value of this field is an associative array which contains arbitrary keys and values, as found in the video stream.timedMetaData2associative array{ }READ\_ONLYThis field contains all the same information included in the **timedMetaData** field and the following additional fields:  

| Key | Type | Value |
| --- | --- | --- |
| data | associative array | The values from the stream's metadata tag, as defined by video provider. |
| position | time | The Presentation Time Stamp (PTS) when the tag was seen. |
| source | enum | This may be one of the following string values: ${source-enum-list} |

timedMetaDataSelectionKeysarray of strings\[ \]READ\_WRITEIf the video stream contains timed meta data such as ID3 tags, any meta data with a key matching an entry in this array is set into the timedMetaData field.  
  
For EMSG data, if any entry in this array is "\*", then all timed meta data is selected.  
  
HLS tags must be defined separately.streamInfoassociative arrayinvalidREAD\_ONLYInformation about the video stream that is currently playing or buffering.  
  

| Key | Type | Value |
| --- | --- | --- |
| isUnderrun | Boolean | If true, the stream was downloaded due to an underrun |
| isResume | Boolean | If true, playback was resumed after trickplay |
| measuredBItrate | Integer | The measured bitrate (bps) of the network when the stream was selected |
| streamBitrate | Integer | The bitrate of the stream |
| streamUrl | URI | The URL of the stream |

completedStreamInfoassociative arrayinvalidREAD\_ONLYInformation about the video stream that most recently completed playing, due to an error, user action, or end of the stream. The associative array consists of the same keys as for the `streaminfo` field, with one additional key, `isFullResult`, a Boolean type that, if true indicates the `stream` played to completion, or if false, was interrupted by an error or user action. This field is set prior to the `state` field being changed, so `state` field observer callback functions can assume that the associative array values are valid when the state field changes.timeToStartStreamingtime0READ\_ONLYThe time in seconds from playback being started until the video actually began playing. The minimum valid value is 1 millisecond, and this is only valid if the current value of the `state` field is `playing`. When the state field value is not `playing`, the value will be 0. This field is updated prior to the `state` field changing, so `state` field observer callback functions can assume this field is valid after the `state` field value changes to `playing`.bufferingStatusassociative arrayinvalidREAD\_ONLYContains information about stream buffering progress and status. This field is valid only while buffering is in progress, both at stream startup or when re-buffering is required. Observers will be notified when any element of the array changes, and also when buffering is complete and the field itself becomes invalid. The array contains the following name - value pairs.  
  

| Value | Meaning |
| --- | --- |
| percentage | Percent buffering complete as an integer. |
| isUnderrun | Boolean value indicating if a stream underrun occurred. |
| prebufferDone | A boolean value that indicates whether the player has buffered enough data to allow the player to begin playing immediately should "control" be set to "play." |
| actualStart | A time value that is automatically set when prebufferDone becomes true, specifying the actual time from which playback will resume. This may vary from the time requested in the content node's playStart field, depending on the capabilities of the player and the seekMode setting. |

  

> While it is possible to use the Video node seek field to specify the seek time, it is recommended that apps do the following:
> 
> 1.  Set the content node field playStart in seek-to-pause scenarios.
> 2.  In the video node, set "control" to "prebuffer".
> 3.  Wait for "prebufferDone" to become "true".
> 4.  Check "actualStart" (if desired).
> 5.  Set "control" to "play".

videoFormatstringREAD\_ONLYContains the format of the currently playing video stream.  
  

| Value | Meaning |
| --- | --- |
| ""  | No stream playing |
| none | Stream contains no playable video |
| unknown | Stream contains unknown video |
| hevc | ISO/IEC 23008-2, H.265, HEVC |
| hevc\_b | ISO/IEC 23008-2 Annex-B, H.265, HEVC |
| mpeg1 | ISO/IEC 11172-2, MPEG-1 part 2, H.261 |
| mpeg2 | ISO/IEC 13818-2, MPEG-2 part 2, H.262 |
| mpeg4\_2 | ISO/IEC 14496-2, MPEG-4 part 2, H.263 |
| mpeg4\_10b | ISO/IEC 14496-10, MPEG-4 part 10 Annex-B, H.264, vc-1 |
| mpeg4\_15 | ISO/IEC 14496-15, MPEG-4 part 15, H.264, vc-1 |
| AVC vc1 | vc-1 |
| wmv | Microsoft Windows Media Video |
| vp8 | VP8 codec |
| vp9 | VP9 codec |

pauseBufferStarttime0READ\_ONLYThe beginning position of the video buffered when paused. This field is only valid for live video.pauseBufferEndtime0READ\_ONLYThe ending position of the video buffered when paused. This field is only valid for live video.pauseBufferPositiontime0READ\_ONLYThe current presentation position of the video buffered when paused. This field is only valid for live video.pauseBufferOverflowBooleanfalseREAD\_ONLYIndicates that the video buffer was not able to save all video since being paused. This field is only valid for live video.streamingSegmentassociative array{ }READ\_ONLYInformation about the video segment that is currently streaming. This is only meaningful for segmented video transports, such as DASH and HLS. The associative array has the following entries:  
  

| Key | Type | Value |
| --- | --- | --- |
| segBitrateBps | integer | Bitrate of the segment in bits per second |
| segSequence | integer | The sequence number of the segment in the video |
| segStart | time | The start time of the segment from the start of the video, specified in seconds |
| segUrl | string | URL of the segment |
| SegType | integer | Type of data in the segment: 1=audio, 2=video, 3=captions, 0=mux |
| Path | string | A path indicating the Period, AdaptationSet and Representation that is played. This is in UNIX directory notation as: /// |
| Width | integer | For video segments, the width of the encoded video picture |
| Height | integer | For video segments, the height of the encoded video picture |

downloadedSegmentassociative arrayinvalidREAD\_ONLYInformation about the video segment that was just downloaded. This is only meaningful for segmented video transports, such as DASH and HLS. The associative array has the following entries:  
  

| Key | Type | Value |
| --- | --- | --- |
| Status | integer | Status of the download: 0 = success, nonzero = error |
| SegSequence | integer | Stream segment sequence number |
| SegUrl | string | Stream segment URL (i.e., .ts file for HLS, stream fragment URL for smooth) |
| DownloadDuration | integer | Amount of time spent downloading the segment, in milliseconds |
| SegSize | integer | Segment size, in bytes |
| SegType | integer | Type of data in the segment: 1=audio, 2=video, 3=captions, 0=mux |
| BitrateBPS | integer | Bitrate of the segment, in bits per second |
| SegStart | time | The start time of the segment from the start of the video, specified in seconds |
| SegDuration | string | The duration of the segment in milliseconds. |
| Path | string | A path indicating the Period, AdaptationSet and Representation that is played. This is in UNIX directory notation as: /// |
| Width | integer | For video segments, the width of the encoded video picture |
| Height | integer | For video segments, the height of the encoded video picture |
| HdrMode | Indicates the HDR format of the content, which may be one of the following values:<br><br>*   0: UNKNOWN<br>*   1: NONE (SDR)<br>*   2: HDR10<br>*   3: DOLBY\_VISION<br>*   4: HLG10<br>*   5: HDR10\_PLUS<br>*   6: SL\_HDR2 |     |
| Latency | The time, in milliseconds, between the current live edge (or most recent available media segment on the CDN) and the segment currently being played. |     |

enableLiveAvailabilityWindowBooleanFalseREAD\_WRITEEnables the scrubbing of the trickplay bar during the availability window of live linear streams.enableThumbnailTilesDuringLiveBooleanFalseREAD\_WRITEEnables the **thumbnailTiles** field to be set and updated in the case of live HLS and DASH streams, which contain thumbnails as the thumbnails become available.  
  
By default and when this is set to false, the **thumbnailTiles** field is not written during live streams to maintain backwards compatibility with older applications and to avoid performance or memory issues. This is becuase they might not be expecting constant updates to the **thumbnailTiles** field if they were written to handle VOD streams, which rarely update the **thumbnailTiles** field.thumbnailTilesroAssociativeArray\[\]READ\_WRITEContains the information about HLS and DASH standard thumbnail tiles as they are discovered within the manifest for streams which contain them.  
  
This field was first introduced (for VOD only) starting in Roku OS 9.1. Starting with Roku OS 11.0, the app can enable this field for HLS and DASH live streams containing standard thumbnails by setting enableThumbnailTilesDuringLive to true.  
  

> For Roku OS releases before 9.4, the **thumbnailTiles** associative array has the following structure: {tile\_id: tile\_set} (string to associative array)
> 
> For Roku OS 9.4 and later, the **thumbnailTiles** associative array has the following structure: {tile\_id: \[tile\_set, tile\_set, tile\_set,...\]}(string to array of associative arrays). This format allows discontinuous tile\_sets of the same resolution to be grouped together as a "choice" for display.

  
  
The **tile\_id** field is a unique string identifier for the **tile\_set**, which is an associative array containing the attributes of the tile set as well as information about the thumbnails.  
  
The **tile\_set** field contains the following fields:  
  

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| htiles | integer | 0   | Horizontal number of thumbnails in a tile (columns.) |
| vtiles | integer | 0   | Vertical number of thumbnails in a tile (rows.) |
| width | integer | 0   | Number of horizontal pixels in a thumbnail (this is not the tile as the one in the DASH spec). |
| height | integer | 0   | Number of vertical pixels in a thumbnail (this is not the same tile as the one in the DASH spec). |
| bandwidth | integer | 0   | Max tile size in bits / duration. |
| duration | float | 0.0 | Duration of one tile in seconds (assuming a full tile). |
| initial\_time | float | 0.0 | Presentation start time of current **tile\_set** in seconds. Thumbnails in tiles beginning before this time should be skipped, and the first relevant thumbnail duration should be updated accordingly. |
| final\_time | float | 0.0 | End time of current tile\_set in seconds. |
| tiles | roArray | \[\] | Contains information about each tile in the **tile\_set**. This contains the following fields:  <br><br>*   url (index 0). A string with the URL of the tile.<br>*   start\_time (index 1). A float with the start time of the tile in seconds.<br/<br>*   format (index 2). Typically, an empty string, but it may contain the tile format "jpeg", "png", and so on. |

trickPlayBackgroundOverlayuri""WRITEThe background overlay to be displayed whenever the playback UI is visible during the video playback experience.

### UI fields

FieldTypeDefaultAccess PermissionDescriptionwidthfloat0.0READ\_WRITESets the width of the video play window in pixels. If set to 0.0 (the default), the video play window is set to the width of the entire display screen.heightfloat0.0READ\_WRITESets the height of the video play window in pixels. If set to 0.0 (the default), the video play window is set to the height of the entire display screen.enableUIBooleantrueREAD\_WRITEIf set to true (the default), the entire Video node user interface (such as ProgressBar and TrickPlayBar nodes, and BIF navigation) appear in response to stream events and remote control key presses.  
  
If set to false, most of the Video node user interface will not be shown, and the application is expected to implement the UI. The one exception is the closed-caption dialog, which always appears when the video is playing fullscreen (either full height or full width) and the user presses the Options (\*) button.  
  
When using the [Roku Advertising Framework (RAF)](/docs/developer-program/advertising/roku-advertising-framework.md), the RAF library may temporarily set this field to false while playing ads.enableTrickPlayBooleantrueREAD\_WRITE**Controls whether trickplay is allowed during playback. When set to false the user trickplay buttons on the remote will have no effect. This only applies when enableUI is set to true.**bifDisplayBifDisplay nodeinternal instance defaultREAD\_WRITEComponent that displays BIFs and allows navigation. The fields of this internal node are as follows:  
  

| Field | Type | Default | Use |
| --- | --- | --- | --- |
| frameBgBlendColor | color | 0xFFFFFFFF | A color to be blended with the image displayed behind individual BIF images displayed on the screen. The blending is performed by multiplying this value with each pixel in the image. If not changed from the default value, no blending will take place. |
| frameBgImageUri | uri | ""  | The URI of an image to be displayed behind individual frames on the screen. The actual frame image is displayed opaquely on top of this background, so only the outer edges of this image are visible. Because of that, this background image typically appears as a border around the video frame. If the frameBgBlendColor field is set to a value other than the default, that color will be blended with the background image. |
| getNearestFrame | time | invalid | **Write-Only**  <br>Requests the nearest BIF to the time specified. This would normally be an offset from the current playback position. The getNearestFrame request is passed to the BifCache which uses the getNearestFrame() method implemented on all BIF storage classes. Existing BifCache functionality is then used to retrieve the bitmap data and load it into the texture manager. |
| nearestFrame | string | ""  | **Read-Only**  <br>Contains the URI of the requested BIF. The returned URIs will be of the form 'memory://BIF_%d_%d'. These URIs can then be used directly in the 'uri' field of a Poster SGN (or similar). |

trickPlayBarTrickPlayBar nodeinternal instance defaultREAD\_WRITEThe visible TrickPlayBar node. The fields of this internal node are as follows:  
  

| Field | Type | Default | Use |
| --- | --- | --- | --- |
| currentTimeMarkerBlendColor | color | 0xFFFFFFFF | This is blended with the marker for the current playback position. This is typically a small vertical bar displayed in the TrickPlayBar node when the user is fast-forwarding or rewinding through the video. |
| textColor | color | system default | Sets the color of the text next to the trickPlayBar node indicating the time elapsed/remaining. |
| thumbBlendColor | color | 0xFFFFFFFF | Sets the blend color of the square image in the trickPlayBar node that shows the current position, with the current direction arrows or pause icon on top. The blending is performed by multiplying this value with each pixel in the image. If not changed from the default value, no blending will take place. |
| filledBarBlendColor | color | 0xFFFFFFFF | This color will be blended with the graphical image specified in the `filledBarImageUri` field. The blending is performed by multiplying this value with each pixel in the image. If not changed from the default value, no blending will take place. |
| liveFilledBarBlendColor | Color | 0xFFFFFFFF | The color of the trickplay progress bar to be blended with the **filledBarImageUri** for live linear streams. |
| filledBarImageUri | uri | ""  | A 9-patch or ordinary PNG of the bar that represents the completed portion of the work represented by this ProgressBar node. This is typically displayed on the left side of the track. This will be blended with the color specified by the `filledBarBlendColor` field, if set to a non-default value. |
| trackBlendColor | color | 0xFFFFFFFF | This color is blended with the graphical image specified by `trackImageUri` field. The blending is performed by multiplying this value with each pixel in the image. If not changed from the default value, no blending will take place. |
| trackImageUri | uri | ""  | A 9-patch or ordinary PNG of the track of the progress bar, which surrounds the filled and empty bars. This will be blended with the color specified by the `trackBlendColor` field, if set to a non-default value. |

bufferingBarProgressBar nodeinternal instance defaultREAD\_WRITEComponent that shows the progress of re-buffering, after video playback has started. The fields of this internal node are as follows:  
  

| Field | Type | Default | Use |
| --- | --- | --- | --- |
| width | float | system default | Sets a custom width for an instance of the ProgressBar node |
| height | float | system default | Sets a custom height for an instance of the ProgressBar node |
| emptyBarBlendColor | color | 0xFFFFFFFF | A color to be blended with the graphical image specified in the `emptyBarImageUri` field. The blending is performed by multiplying this value with each pixel in the image. If not changed from the default value, no blending will take place. |
| emptyBarImageUri | uri | ""  | A 9-patch or ordinary PNG of the bar presenting the remaining work to be done. This is typically displayed on the right side of the track, and is blended with the color specified in the `emptyBarBlendColor` field, if set to a non-default value. |
| filledBarBlendColor | color | 0xFFFFFFFF | This color will be blended with the graphical image specified in the `filledBarImageUri` field. The blending is performed by multiplying this value with each pixel in the image. If not changed from the default value, no blending will take place. |
| filledBarImageUri | uri | ""  | A 9-patch or ordinary PNG of the bar that represents the completed portion of the work represented by this ProgressBar node. This is typically displayed on the left side of the track. This will be blended with the color specified by the `filledBarBlendColor` field, if set to a non-default value. |
| trackBlendColor | color | 0xFFFFFFFF | This color is blended with the graphical image specified by `trackImageUri` field. The blending is performed by multiplying this value with each pixel in the image. If not changed from the default value, no blending will take place. |
| trackImageUri | uri | ""  | A 9-patch or ordinary PNG of the track of the progress bar, which surrounds the filled and empty bars. This will be blended with the color specified by the `trackBlendColor` field, if set to a non-default value. |
| percentage | integer | top | The percentage of the work that is done. Setting this field controls the visual appearance of the ProgressBar node. |

bufferingTextColorcolorsystem defaultREAD\_WRITEThe color of the text displayed near the buffering bar defined by the `bufferingBar` field, when the buffering bar is visible. If this is 0, the system default color is used. To set a custom color, set this field to a value other than 0x0.retrievingBarProgressBar nodeinternal instance defaultREAD\_WRITEComponent that shows the progress of initial retrieving of the video, prior to starting playback. The fields of this internal node are the same as for the `bufferingBar` field, which are the fields of the internal ProgressBar node.retrievingTextColorcolorsystem defaultREAD\_WRITEThe color of the text displayed near the retrieving bar, when the retrieving bar defined in the `retrievingBar` field is visible. If this is 0, the system default color is used. To set a custom color, set this field to a value other than 0x0.pivotNoderenderable node\-READ\_WRITEThe visible pivot node. This is a generic renderable node that can be used to display any component. This node is only displayed when video is paused.

### Closed caption fields

FieldTypeDefaultAccess PermissionDescriptionglobalCaptionModeoption stringOffREAD\_WRITESets the value of the global Roku closed-caption mode. This can be used to allow the user or the application to change the closed-caption mode in an application just before or during video playback. The possible options are:  
  

| Option | Effect |
| --- | --- |
| "Off" | Turns the global Roku closed-caption mode off. |
| "On" | Turns the global Roku closed-caption mode on. |
| "Instant replay" | Sets the global Roku closed-caption setting to display captions only during instant replay. |
| "When mute" | Sets the global Roku closed-caption setting to display captions only when the volume is muted. (This only applies to Roku TVs.) |

  
  
The app should set the `subtitleTrack` field regardless of the selected Caption Mode.suppressCaptionsbooleanfalseREAD\_WRITESuppresses the closed caption for the purpose of resolving conflicts in cases where UI elements are drawn.  
  
Note that most of the disabling/enabling of the captions are done by the Roku OS, including enabling closed caption for Instant Replay.subtitleTrackstringREAD\_WRITEThe identifier of the selected subtitle track. Subtitles may or may not be visible on the screen, depending upon the user's caption mode setting.  
  
Reading this field will return the identifier of the subtitle track selected by the user. Writing this the field will change the track.  
  
See also: [globalCaptionMode](#closed-caption-fields)currentSubtitleTrackstringREAD\_ONLYThe identifier of the selected subtitle track. Subtitles may or may not be visible on the screen, depending upon the user's caption mode setting.  
  
Reading this field will return the identifier of the subtitle track that is playing. When the user has not selected a track, the Roku media player will select a track based on the preferred caption language system setting.availableSubtitleTracksarray of associative arrays\[ \] empty arrayREAD\_ONLYThe list of subtitle tracks available in the video stream. The array is initially populated with the tracks specified in the Content Meta-Data, and additional tracks are added if they are detected by the digital video player. Each associative array has the following entries:  
  

| Key | Type | Value |
| --- | --- | --- |
| Description | string | Descriptive name of the subtitle track |
| Language | string | ISO 639-2 three-letter language code |
| TrackName | string | The track identifier. The value of this field may be used to select the subtitle track. |
| HasAccessibilityDescription  <br>  <br>_Available since Roku OS 13.0_ | boolean | HLS: represents "public.accessibility.describes-music-and-sound." |
| HasAccessibilityCaption  <br>  <br>_Available since Roku OS 13.0_ | boolean | HLS: represents "public.accessibility.transcribes-spoken-dialog."  <br>  <br>DASH: Subtitle track contains captions |
| HasAccessibilitySign  <br>  <br>_Available since Roku OS 13.0_ | boolean | DASH: Subtitle track contains a sign-language interpretation of an audio component info. |

captionStyleassociative arraysystem defaultREAD\_WRITEAllows apps to style closed captions. For any keys that are absent from the associative array, or for unexpected values, the Default value is assumed for that property. Following are the possible key names and values for this field:  
  

| Property | Possible Values |
| --- | --- |
| Text/Font | Default  <br>Serif Fixed Width  <br>Serif Proportional  <br>Sans Serif Fixed Width  <br>Sans Serif Proportional  <br>Casual  <br>Cursive  <br>Small Caps |
| Text/Effect | Default  <br>None  <br>Raised  <br>Depressed  <br>Uniform  <br>Drop shadow (left)  <br>Drop shadow (right) |
| Text/Size | Default  <br>Large  <br>Medium  <br>Small |
| Text/Color | Default  <br>White  <br>Black  <br>Red  <br>Green  <br>Blue  <br>Yellow  <br>Magenta  <br>Cyan |
| Text/Opacity | Default  <br>25%  <br>50%  <br>75%  <br>100% |
| Background/Color | Default  <br>White  <br>Black  <br>Red  <br>Green  <br>Blue  <br>Yellow  <br>Magenta  <br>Cyan |
| Background/Opacity | Default  <br>Off  <br>25%  <br>50%  <br>75%  <br>100% |
| Window/Color | Default  <br>White  <br>Black  <br>Red  <br>Green  <br>Blue  <br>Yellow  <br>Magenta  <br>Cyan |
| Window/Opacity | Default  <br>Off  <br>25%  <br>50%  <br>75%  <br>100% |

### Audio fields

FieldTypeDefaultAccess PermissionDescriptionmuteBooleanfalseREAD\_WRITESet to true to mute the audio of the video currently playing in the Video node. Set to false to restore audio.audioTrackstringREAD\_WRITEThe track identifier of the selected audio track.  
  
Reading this field will return the track identifier of the audio selected by the user.  
  
Writing this value will change the audio track. However, apps should not do this unless they are implementing their own track selection menu that users control. This is because the Roku OS selects the best track automatically based on preferred language setting on the device. See [Automatic audio track selection](#automatic-audio-track-selection) for more information.currentAudioTrackStringREAD\_ONLYThe track identifier of the audio being played. Reading this field will return the track that is being played, which may be different than the track being selected (for example, when the Roku media player cannot play a certain format).  
  
When the user has not selected an audio track, the platform will select a track based on the preferred audio language setting.availableAudioTracksarray of associative arrays\[ \] empty arrayREAD\_ONLYEach associative array has the following entries:  
  

| Key | Type | Value |
| --- | --- | --- |
| Language | string | ISO 639-2 three-letter language code |
| Name | string | Descriptive name of the audio track |
| Track | string | The track identifier. The value of this field may be used to select the audio track. |
| HasAccessibilityDescription  <br>  <br>_Available since Roku OS 13.0_ | boolean | HLS: represents "public.accessibility.describes-video."  <br>  <br>DASH: Audio track contains a textual description (intended for audio synthesis) or an audio description describing a visual component. |
| HasAccessibilityEAI  <br>  <br>_Available since Roku OS 13.0_ | boolean | DASH: Audio track contains an element for improved intelligibility of the dialogue \[Enhanced Audio Intelligibility\]. |

  
  
The field also retrieves audio description tracks which are typically seen on broadcast TV. An audio description track is mixed with the main audio track.seamlessAudioTrackSelection  
  
_Available since Roku OS 13.0_BooleanfalseREAD\_WRITEEnables apps to continuously play video when the audio track is switched. This feature currently supports HLS only.  
  

*   **true**: Continues video playback when the audio track changes (provided that HLS is being used and the audio format of the new audio track is the same as the original one). In this case, a brief period of no audio may occur while the audio tracks are switched.
*   **false**: Pauses video playback for approximately 1 second when the audio track changes (default behavior). In this case, a black screen and/or buffering appears while the audio tracks are switched.

  
  
To enable this feature, you must set this field before sending any command to the Video node. This field may not be changed during video playback.audioFormatstringREAD\_ONLYIn all other cases they shouldn't .Contains the format of the currently playing audio.  
  

| Value | Meaning |
| --- | --- |
| ""  | No stream playing |
| none | Stream contains no playable audio |
| unknown | Stream contains unknown audio |
| aac | ISO/IEC 14496-3, Advanced Audio Coding |
| aac\_adif | ISO/IEC 14496-3, Advanced Audio Coding, ADIF container |
| aac\_adts | ISO/IEC 14496-3, Advanced Audio Coding, ADTS container |
| aac\_latm | ISO/IEC 14496-3, Advanced Audio Coding, LATM container |
| ac3 | Dolby Digital |
| ac4 | Dolby Audio - AC-4 |
| alac | Apple Lossless |
| dts | DTS Coherent Acoustics |
| eac3 | Dolby Digital Plus |
| flac | Free Lossless Audio Codec |
| flac | Free Lossless Audio Codec |
| mat | Dolby Audio - TrueHD |
| mp3 | ISO/IEC 11172-3, MPEG Audio Layer III |
| pcm | linear PCM |
| vorbis | Ogg Vorbis |
| wma | Microsoft Windows Media Audio (sunset as of Roku OS 12.5) |
| wmapro | Microsoft Windows Media Pro Audio (sunset as of Roku OS 12.5) |

supplementaryAudioVolumeint50READ\_WRITESets the volume of the description tracks separately from the main audio track. The field value can range from 0 to 100.

#### Automatic audio track selection

If multiple audio tracks are available for video content, the Roku OS automatically selects the best track based on the preferred audio track settings on the device (language, country code, and descriptive setting) and the quality of the audio track (bitrate/format).

The user can manually set their preferred language in the **Settings > Audio > Audio preferred language** menu, and the country code and descriptive setting are automatically set when the user selects an audio track. The preferred language setting is also automatically updated when the user selects an audio track (the preferred language is set to the language of the selected track).

For example, if the user chooses Portuguese as their preferred language, the Roku OS will by default select the Portuguese audio track the next time they watch content (if available). If the selected audio track is in Portuguese (Brazil), the user's preferred country is set to Brazil, and the Portuguese (Brazil) audio track is selected by default the next time the user watches content.

> It is recommend that apps use the audio track selection logic provided by the Roku OS instead of implementing their own.

Overall, the Roku OS uses the following criteria (listed in order of priority) to determine which audio track to play:

1.  Preferred audio track settings:
    
    a. The track explicitly selected by the user.
    
    b. The track with the user's preferred language, country code and descriptive setting.
    
    c. The track with the preferred language and the country code.
    
    d. The track with the preferred language that is marked as the default audio track.
    
    e. The track with the preferred language.
    
    f. The first track.
    
2.  Highest quality audio track (based on bitrate/format)
    

> Any language not included in the provided list of common languages is added to the list as the last entry. The common languages list may only have a single unlisted language. For example, if the user selects Korean as the audio track for a movie, the last entry in the common languages list is Korean, which is used as the preferred language from thereon. If the user then selects a Chinese audio track, Chinese overwrites Korean as the last entry in the common language list and is used as the preferred language.

### CDN fields

Developers can receive event-based notifications when the CDN is switched during content playback.

AttributeTypeValuesDescriptioncdnSwitchroArray of roAssociativeArrays

| Key | Required/ Optional | Type | Description |
| --- | --- | --- | --- |
| URLFilter | Required | String | A substring that identifies the (base)URL to which these CDN settings apply.  <br>  <br>The Roku media player matches this string against all (base)URLs listed in the manifest and applies the setting to all (base)URLs that contain this substring. |
| ContentFilter | Optional | String | For DASH streams, a substring that filters the period or asset ID to which these CDN settings apply.  <br>  <br>The Roku player only applies these CDN setting to periods with a period ID or asset ID that contains this substring.  <br>  <br>This match is used in addition to the URL filter. |
| Priority | Required | Integer | For configuring failovers, sets the priority for this (base)URL from 1 to x (a priority of 0 or less is invalid).  <br>  <br>A lower value indicates a higher priority. For example, a (base)URL with a priority of 1 is higher than another with a priority of 10.  <br>  <br>If the highest priority server fails, traffic is routed to the server with the next highest priority. If all servers are configured with the same priority, and one fails, no failover will happen. |
| Weight | Required | Integer | For configuring load balancing, sets the relative weight for all (base)URLs with the same priority. This must be a value of 1 or greater (a weight of 0 disables a CDN).  <br>  <br>The weight of a given BaseURL is its weight value divided by the sum of all weight values. This means that to spread the load equally across multiple CDNs with the same priority, set the weight for each to the same value. To configure the weights for two servers to 80% and a third server to 20%, for example, set servers one and two to 8 and server three to 4. |
| ServiceLocation | Optional | String | A blacklist of failed BaseURL locations. |

Indicates that a CDN switching event has occurred.  
  
Apps can monitor this field in the background. When the Video player detects a CDN change, it automatically updates this field.

### Miscellaneous fields

FieldTypeDefaultAccess PermissionDescriptionMaxVideoDecodeResolutionvector2d (width, height)\[0,0\]READ\_WRITESets the max resolution required by your video.  
  
Video decode memory is a shared resource with OpenGL texture memory. The Brightscript 2D APIs are implemented using OpenGL texture memory on Roku models that support the Open GL APIs (see [Hardware specifications](/docs/specs/hardware.md) for a list of these models).  
  
On models that do not support Open GL APIs, this field exists for API compatibility but has no effect on actual memory allocations.  
  
Video decode memory allocation is based on a resolution of 1920x1080 or 1280x720 as the maximum supported resolution for a particular Roku model (see [Hardware specifications](/docs/specs/hardware.md) for a list of these models).  
  
This field enables applications that want to use both the 2D APIs and video playback with a lower resolution than 1080p. Without this field, these applications are likely to not have enough memory for either video playback or UI rendering.  
  
If width is 0 (the default), it is unlimited. If height is 0 (the default), it is unlimited.cgmsinteger0READ\_WRITESets the CGMS (Copy Guard Management System) on analog outputs to the desired level. The valid values are:  
  

| Value | Effect |
| --- | --- |
| 0   | No copy restriction |
| 1   | Copy no more |
| 2   | Copy once allowed |
| 3   | No copying permitted |

enableScreenSaverWhilePlayingBooleanfalseREAD\_WRITESet this to true to allow the screensaver to activate even if video is playing, as long as that video does not cover 50% or more of the screen. Set to false to block the screensaver activating if any video is playing. Note that this field has no effect when the video node plays audio only streams. For screensaver control with audio only streams, use the disableScreenSaver field.disableScreenSaverBooleanfalseREAD\_WRITESet this to true to suppress the screensaver. This is typically used to suppress the screensaver when playing audio-only streams.contentBlockedBooleanfalseREAD\_ONLY_Available since Roku OS 8._  
  
Determines whether the current content is blocked.

Data bindings
-------------

See [Content Meta-Data](/docs/developer-program/getting-started/architecture/content-metadata.md) for the required and optional play parameters, and descriptive information for video playback. Set these parameters in a [ContentNode](/docs/references/scenegraph/control-nodes/contentnode.md) node, and assign the ContentNode to the content field of the Video node to apply the parameters to a particular video content item.

For HTTPS access, note the following Content Meta-Data attributes:

*   `HttpCertificatesFile`
*   `HttpCookies`
*   `HttpHeaders`
*   `HttpSendClientCertificates`

These attributes must be set to handle secure HTTP transfers of video files. Note that this is a different HTTPS mechanism than used for other SceneGraph nodes as described in [roHttpAgent](/docs/references/brightscript/components/rohttpagent.md).

> Prior to Roku OS 7.2, each Audio and Video node created and configured an `HttpAgent` only when the first content was played in a given Audio or Video node instance. This sometimes meant that additional content would fail to play in the same node because headers, cookies, and certificates were not updated or correctly replaced from the new content record. Apps that are dependent upon this behavior will need to be updated to set the required data into the Content Meta-Data for each piece of content, or to programmatically set those values into the `HttpAgent` before playing each piece of content.

Example
-------

To play video in an application, you first need to create a Video node, either in BrightScript using the roSGNode [ifSGNodeChildren](/docs/references/brightscript/interfaces/ifsgnodechildren.md) interface, or in XML markup. For example, in XML markup:

    <Video
      id="musicvideos"
      width="1280"
      height="720"
      translation="[0,0]"
    />
    

The Video node is then scripted to specify the URL of the video stream, streaming format, video title, and any other [Content Meta-Data](/docs/developer-program/getting-started/architecture/content-metadata.md) attributes needed for the particular playback. Once the video properties are specified, the video can be played by setting the Video node `control` field value to `play`.

    <script type="text/brightscript" >
    <![CDATA[
    
    sub init()
      m.top.setFocus(true)
      setVideo()
    end sub
    
    function setVideo() as void
      videoContent = createObject("RoSGNode", "ContentNode")
      videoContent.url = "https://roku.s.cpl.delvenetworks.com/media/59021fabe3b645968e382ac726cd6c7b/60b4a471ffb74809beb2f7d5a15b3193/roku_ep_111_segment_1_final-cc_mix_033015-a7ec8a288c4bcec001c118181c668de321108861.m3u8"
      videoContent.title = "Test Video"
      videoContent.streamformat = "hls"
    
      m.video = m.top.findNode("musicvideos")
      m.video.content = videoContent
      m.video.control = "play"
    end function
    
    ]]>
    </script>
    

#{source-enum-list}

*   "emsg"
*   "id3"
*   "hls"
*   "unk"

Sample app
----------

[VideoExample](https://github.com/rokudev/samples/tree/master/media/VideoExample) is a sample app demonstrating Video in action.