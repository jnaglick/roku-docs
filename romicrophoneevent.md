roMicrophoneEvent
=================

The [roMicrophone](/docs/references/brightscript/components/romicrophone.md "roMicrophone") component sends the `roMicrophoneEvent` with the following predicates that indicate its valid event types:

Supported methods
-----------------

### IsRecordingDone() as Boolean

Checks if the microphone recording session has been closed. This method returns true if the recording session is closed; otherwise, it returns false.

### IsRecordingInfo() as Boolean

Checks whether the microphone is open. This method returns true when the microphone is open; otherwise, it returns false.

#### GetInfo() as Object

Returns the information regarding a particular microphone recording session. This method returns an roAssociativeArray containing the following information:

| Key | Type | Value |
| --- | --- | --- |
| format | string | The audio data format (ex. pcm-s16-le) |
| num\_channels | integer | The number of channels (ex. 1 for mono) |
| sample\_rate | integer | The audio sample rate (ex. 16000 for 16kHz) |
| sample\_data | roByteArray | Signed 16-bit integer containing audio data as PCM (little-endian format) |
| level | integer | Value displaying a calculated volume level between 0 (silence) and 100 (maximum) |