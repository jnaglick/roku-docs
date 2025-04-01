Audio
=====

Extends [**Node**](/docs/references/scenegraph/node.md)

The Audio node class plays streaming audio.

The Audio node class has no built-in visual UI, but you can build your own UI for the node, including trick play, or showing an album cover or similar graphical image for each song selected by a user.

Fields
------

FieldTypeDefaultAccess PermissionDescriptioncontentContentNodeNULLREAD\_WRITEThe ContentNode with the [Content Meta-Data](/docs/developer-program/getting-started/architecture/content-metadata.md) for the audio or audio playlist (a sequence of audios) to be played. If a audio playlist is to be played, the ContentNode must include complete child ContentNodes for each audio in the playlist, with all attributes required to play that audio.contentIsPlaylistBooleanfalseREAD\_WRITEIf set to true, enables audio playlists (a sequence of audios to be played). To enable audio playlists, the ContentNode set in the `content` field must have children ContentNodes for each audio in the playlist. When audio playback is started, all of the audios in the playlist will be played in sequence.nextContentIndexinteger\-1READ\_WRITEIf the `contentIsPlaylist` field is set to true to enable audio playlists, sets the index of the next audio in the playlist to be played. Setting this field does not immediately change the audio being played, but takes effect when the current audio is completed or skipped. By default, this value is -1, which performs the default index increment operation. After the audio specified by the index in this field begins playing, the field is set to the default -1 again, so the next audio played will be set by the default index increment operation, unless the field is set again to a different index.loopBooleanfalseREAD\_WRITEIf set to true, the audio or audio playlist (if the `contentIsPlaylist` field is set to true to enable audio playlists) will be restarted from the beginning after the end is reached.bufferingStatusassociative arrayinvalidREAD\_ONLYContains information about stream buffering progress and status. This field is valid only while buffering is in progress, both at stream startup or when re-buffering is required. Observers will be notified when any element of the array changes, and also when buffering is complete and the field itself becomes invalid. The array contains the following name - value pairs.  
  

| Value | Meaning |
| --- | --- |
| percentage | Percent buffering complete as an integer. |
| isUnderrun | Boolean value indicating if a stream underrun occurred. |

controloption stringnoneREAD\_WRITESets the desired play state for the audio, such as starting or stopping the audio play. Getting the value of this field returns the most recent value set, or `none` if no value has been set. In order to dynamically monitor the actual state of the audio, see the `state` field.  
  

| Option | Effect |
| --- | --- |
| none | No play state set |
| play | Start audio play |
| start | Start audio play |
| stop | Stop audio play |
| pause | Pause audio play |
| resume | Resume audio play after a pause |
| replay | Replay audio |
| prebuffer | Starts buffering the audio stream before the Audio node actually begins playback. Only one audio stream can be buffering in the application at any time. Setting the `control` field to `prebuffer` for another audio stream after setting `prebuffer` for a previous audio stream stops the buffering of the previous audio stream. |
| skipcontent | Skip the currently-playing content, and begin playing the next content in the playlist. If the content is not a playlist, or if the current content is the end of the playlist, this will end playback. |

notificationIntervaltime0.5READ\_WRITEThe interval between notifications to observers of the position field, specified as the number of seconds. If the value is 0, no notifications are delivered. This value may be read or modified at any time.timedMetaDataSelectionKeysarray of strings\[ \]READ\_WRITEIf the audio stream contains timed meta data such as ID3 tags, any meta data with a key matching an entry in this array will be set into the timedMetaData field. If any entry in this array is "\*", then all timed meta data will be selected.seektimeinvalidWRITE\_ONLYSets the current position in the audio. The value is the number seconds from the beginning of the stream, specified as a double.contentIndexinteger\-1READ\_ONLYThe index of the audio in the audio playlist that is currently playing. Generally, you would only want to check this field if audio playlists are enabled (by setting the `contentIsPlaylist` field to true), but it is set to 0 when a single audio is playing and audio playlists are not enabled.timedMetaDataassociative array{ }READ\_ONLYThe most recent timed meta data that has been decoded from the audio stream. Only meta data with a key that matches an entry in timedMetaDataSelectionKeys will be set into this field. The value of this field is an associative array which contains arbitrary keys and values, as found in the audio stream.  
  
As of Roku OS 10.5, this field can be used to read ID3 tags embedded in an audio stream.statevalue stringnoneREAD\_ONLYDescribes the current audio play state, such as if the audio play has been paused.  
  

| Value | Meaning |
| --- | --- |
| none | No current play state |
| buffering | Audio stream is currently buffering |
| playing | Audio is currently playing |
| paused | Audio is currently paused |
| stopped | Audio is currently stopped |
| finished | Audio has completed play |
| error | An error has occurred in the audio play. The error code and error message can be found in the `errorCode` and `errorMsg` fields respectively. |

positiontimeinvalidREAD\_ONLYThe current position in the audio play, as the number of seconds.durationtime0READ\_ONLYThe duration of the audio being played, specified in seconds. This becomes valid when playback begins and may change if the audio is dynamic content, such as a live event.errorCodeinteger0READ\_ONLYThe error code associated with the audio play error set in the `state` fielderrorMsgstringREAD\_ONLYAn error message describing the audio play error set in the `state` field.audioFormatstringREAD\_ONLYContains the format of the currently playing audio.  
  

| Value | Meaning |
| --- | --- |
| ""  | No stream playing |
| aac | ISO/IEC 14496-3, Advanced Audio Coding |
| aac\_adif | ISO/IEC 14496-3, Advanced Audio Coding, ADIF container |
| aac\_adts | ISO/IEC 14496-3, Advanced Audio Coding, ADTS container |
| aac\_latm | ISO/IEC 14496-3, Advanced Audio Coding, LATM container |
| ac3 | Dolby Digital |
| alac | Apple Lossless |
| dts | DTS Coherent Acoustics |
| eac3 | Dolby Digital Plus |
| flac | Free Lossless Audio Codec |
| mp2 | ISO/IEC 11172-3, MPEG Audio Layer II |
| mp3 | ISO/IEC 11172-3, MPEG Audio Layer III |
| none | Stream contains no playable audio |
| pcm | linear PCM |
| unknown | Stream contains unknown audio |
| vorbis | Ogg Vorbis |
| wma  <br>_sunset as of Roku OS 12.5_ | Microsoft Windows Media Audio.  <br>  <br>As of Roku OS 10.5, the Roku platform no longer supports this audio format. As part of the Roku OS 12.5 release, this format was officially sunset. |
| wmapro  <br>_sunset as of Roku OS 12.5_ | Microsoft Windows Media Pro Audio.  <br>  <br>As of Roku OS 10.5, the Roku platform no longer supports this audio format. As part of the Roku OS 12.5 release, this format was officially sunset. |

streamingSegmentassociative array{ }READ\_ONLYInformation about the audio segment that is currently streaming. This is only meaningful for segmented audio transports, such as DASH and HLS. The associative array has the following entries:  
  

| Key | Type | Value |
| --- | --- | --- |
| segBitrateBps | integer | Bitrate of the segment in bits per second |
| segSequence | integer | The sequence number of the segment in the audio |
| segStart | time | The start time of the segment from the start of the audio, specified in seconds |
| segUrl | string | URL of the segment |

focusedChildN/AN/AREAD\_WRITEWhen a node or one of its children gains or loses the keyboard focus, the focusedChild field will be set and call its observer functions. In the observer function, typically, you use [ifSGNodeFocus](/docs/references/brightscript/interfaces/ifsgnodefocus.md) functions to query whether this node or some other node has the key focus or is in the key focus chain.  
  
Accessing the value of the field will result in script errors.autoplayAfterSeekbooleantrueREAD\_WRITEEnables audio content to automatically play after rebuffering. Setting this flag to false disables this default behavior.mutebooleanfalseREAD\_WRITESet to true to mute the audio currently playing in the Audio node. Set to false to restore audio.streamInfoassociative arrayinvalidREAD\_ONLYInformation about the audio stream that is currently playing or buffering.  
  

| Key | Type | Value |
| --- | --- | --- |
| isUnderrun | Boolean | If true, the stream was downloaded due to an underrun |
| isResumed | Boolean | If true, playback was resumed after trickplay |
| measuredBItrate | Integer | The measured bitrate (bps) of the network when the stream was selected |
| streamBitrate | Integer | The bitrate of the stream |
| streamUrl | URI | The URL of the stream |

timeToStartStreamingtime0READ\_ONLYThe time in seconds from playback being started until the audio actually began playing. The minimum valid value is 1 millisecond, and this is only valid if the current value of the `state` field is `playing`. When the state field value is not `playing`, the value will be 0. This field is updated prior to the `state` field changing, so `state` field observer callback functions can assume this field is valid after the `state` field value changes to `playing`.

Data bindings
-------------

See [Content Meta-Data](/docs/developer-program/getting-started/architecture/content-metadata.md) for the required and optional play parameters, and descriptive information for audio playback. Set these parameters in a [ContentNode](/docs/references/scenegraph/control-nodes/contentnode.md) node, and assign the ContentNode to the content field of the Audio node to apply the parameters to a particular audio content item.

For HTTPS access, note the following Content Meta-Data attributes:

*   `HttpCertificatesFile`
*   `HttpCookies`
*   `HttpHeaders`
*   `HttpSendClientCertificates`

These attributes must be set to handle secure HTTP transfers of audio files. Note that this is a different HTTPS mechanism than used for other SceneGraph nodes as described in [roHttpAgent](/docs/references/brightscript/components/rohttpagent.md).

> Prior to Roku OS 7.2, each Audio and Video node created and configured an `HttpAgent` only when the first content was played in a given Audio or Video node instance. This sometimes meant that additional content would fail to play in the same node because headers, cookies, and certificates were not updated or correctly replaced from the new content record. Apps that are dependent upon this behavior will need to be updated to set the required data into the Content Meta-Data for each piece of content, or to programmatically set those values into the `HttpAgent` before playing each piece of content.

Example
-------

**Example application:** [AudioExample](https://github.com/rokudev/samples/tree/master/media/AudioExample)

[AudioExample](https://github.com/rokudev/samples/tree/master/media/AudioExample) uses a [LabelList](/docs/references/scenegraph/list-and-grid-nodes/labellist.md) node to select from several spoken audio examples. The [Content Meta-Data](/docs/developer-program/getting-started/architecture/content-metadata.md) for the example is found in the `pkg:/server/audiocontent.xml` file, read into a [ContentNode](/docs/references/scenegraph/control-nodes/contentnode.md) node by the [Task](/docs/references/scenegraph/control-nodes/task.md) node `audiocontentreader.xml` component file.

Sample app
----------

[AudioExample](https://github.com/rokudev/samples/tree/master/media/AudioExample) is a sample app demonstrating Audio in action.