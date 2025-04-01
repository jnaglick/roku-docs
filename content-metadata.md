Content metadata
================

Content metadata describes a viewable title that will be shown to the user. Content may be any supported type of video and the metadata is used by the UI to format and display the title to the user. Some attributes (e.g. ContentType) affect how the title is displayed on screen, other attributes (e.g. SDPosterURL) specify where to fetch artwork to display with the content and other attributes (e.g. Title) are just rendered as text.

Overview
--------

The content metadata is stored in an associative array by the script and provided to the various screen objects as needed for display. In some cases an array of content metadata may be provided so that the screen can render multiple items as a list. The attribute names are critical and used as the key to look up the attribute at run time. The following table details the attributes in use. Certain attributes are recognized by particular screens, while others are more globally applicable. If the attribute is not a generally recognized attribute, the table below specifies where the attributes are recognized.

Keep in mind that there are two ways to specify stream content metadata, **data.Stream** and **data.Streams**:

*   **data.Stream**: This is used when there is one stream URL, typically an HLS or smooth streaming manifest URL.
*   **data.Streams**: This is used when you have a set of fixed bitrate streams. This is typically the case for non-adaptive MP4 streams, in which case multiple variants are specified to simulate true adaptation.

Descriptive attributes
----------------------

Descriptive metadata attributes can be used to describe the content item to the user, in a user interface element that allows the user to select the item.

AttributesTypeValuesExampleContentTypeStringAlthough ContentType accepts type String, the return value is of type [roInt](/docs/references/brightscript/components/roint.md). See table below.

| Content Type | Return Value |
| --- | --- |
| audio | 5   |
| episode | 4   |
| movie | 1   |
| not set or not supported | 0   |
| season | 3   |
| series | 2   |

"movie"TitleStringContent title: movie title for films; episode title for TV series"The Dark Knight"TitleSeasonStringSeason title for TV series"Battlestar Galactica Season 5"SecondaryTitleStringSecondary title for the video content"2022" (release year of the movie)DescriptionStringDescription of content"Batman, Gordon and Harvey Dent are forced…"SDPosterUrlStringURL for SD content artworkmysite.com/img/sd1932.jpgHDPosterUrlStringURL for HD content artworkmysite.com/img/hd1932.jpgFHDPosterUrlStringURL for FHD content artworkmysite.com/img/fhd1932.jpgReleaseDateStringFormatted Date String"3/31/2009"RatingStringSelects an icon to be displayed for the corresponding MPAA or TV rating, that is, the value will display as an icon artwork. See [Rating Attribute Icons](/docs/developer-program/getting-started/architecture/content-metadata.md#rating-attribute-icons) for a list of the acceptable values and the corresponding icon."PG-13"StarRatingIntegerSpecifies the star rating to display as red star icon artwork, as a number from 1 to 100:

*   20 displays one star
*   40 displays two stars
*   60 displays three stars
*   80 displays four stars
*   100 displays five stars

Numbers not divisible by 20 are displayed as a fractional star (A number of 30 will display one and a half stars)80UserStarRatingIntegerSpecifies the user star rating to display as yellow star icon artwork, as a number from 1 to 100:

*   20 displays one star
*   40 displays two stars
*   60 displays three stars
*   80 displays four stars
*   100 displays five stars

Does not display fractional stars for numbers not divisible by 2080ShortDescriptionLine1StringLine 1 of Poster Screen Description"The Dark Knight"ShortDescriptionLine2StringLine 2 of Poster Screen Description"Rent $1.99, Buy $14.99"EpisodeNumberStringEpisode Number"1"NumEpisodesIntegerNumber of episodes for a "season" or "series" contentType40ActorsroArrayList of Actor Names\["Brad Pitt", "Angelina Jolie"\]ActorsStringIndividual Actor Name"Brad Pitt"DirectorsroArrayList of Director Names\["Joel Coen", "Clint Eastwood"\]CategoriesroArrayList of Category/Genre Names\["Comedy", "Drama"\]CategoriesStringIndividual Category/Genre Name"Comedy"AlbumStringroSpringboard audio style uses this to display the album"Achtung"ArtistStringroSpringboard audio style uses to show artist"U2"TextOverlayULStringroSlideShow displays this string in Upper Left corner of slide"Joe's Photos"TextOverlayURStringroSlideShow displays this string in Upper Right corner of slide"3 of 20"TextOverlayBodyStringroSlideShow displays this string on the bottom part of slide"Wanda's 40'th Birthday"

Digital rights management (DRM) control attributes
--------------------------------------------------

Digital rights management (DRM) content meta-data control attributes are available in the Roku OS through the drmParams parameter of type [roAssociativeArray](/docs/references/brightscript/components/roassociativearray.md). The table below enumerates all usable attributes of drmParams.

**Note:** Not all attributes are required, and may not have the same semantic meaning when applied to different DRM systems.

| Attribute | DRM System | Type | Value | Example |
| --- | --- | --- | --- | --- |
| appData | Playready, Widevine, Verimatrix: Optional | String | Special meaning per DRM system. If supplied, expected to be a base64 encoded string. | "SGF2ZSB0byBkZWFsIHdpdGggQmFzZTY0IGZ..." |
| encodingKey | Playready: Optional | String | This field is deprecated; use the **licenseServerURL** field.  <br>  <br>Specifies the PlayReady license acquisition data, in format depending on the EncodingType attribute value specified:  <br><br>*   when encodingType="PlayReadyLicenseAcquisitionUrl", the EncodingKey attribute contains the PlayReady license acquisition URL | "[http://serverName/](http://serverName/) |
| encodingType | Playready: Optional | String | This field is deprecated; use the **licenseServerURL** field.  <br>  <br>Specifies the encoding scheme for PlayReady DRM, by setting to one of the following values:  <br><br>*   "PlayReadyLicenseAcquisitionUrl"<br>*   "PlayReadyLicenseAcquisitionAndChallenge" Note, this is the same value that used to be specified directly in Content Metadata structure | "PlayReadyLicenseAcquisitionAndChallenge" |
| KeySystem | Required for all | String | "playready" or "widevine". This value is case-insensitive. The default is an empty string.  <br>  <br><br>> As of Roku OS 9.3, support for Verimatrix DRM has been removed from the firmware. Make sure that content in your app is protected using one of the following Roku-supported DRMs: Microsoft PlayReady or Widevine. Click [here](/docs/specs/media/content-protection.md) for more information on implementing these DRMs. | "widevine" |
| licenseRenewURL | Widevine: Optional | String | A URL location for sending license renewal requests. If not specified, the Roku OS would send renewal requests to the URL specified in the licenseServerURL. | " [https://host.com/license/wideivne/renew?licenseid=090495867002](https://host.com/license/wideivne/renew?licenseid=090495867002) " |
| licenseServerURL | Playready: Required Widevine: Required | String | A URL location of a license server. This URL may include CGI parameters.  <br>  <br>If this field contains the PlayReady license acquisition URL plus additional custom license acquisition request data in format "URL%%%", the “PlayReadyLicenseAcquisitionAndChallenge" type is used. | "[https://host.com/license/playready?contentid=090495867002](https://host.com/license/playready?contentid=090495867002) " |
| serializationURL | Playready, Widevine: Optional | String | A server address used for device provisioning | "[https://host.com/provision/device?esn=090495867002](https://host.com/provision/device?esn=090495867002) " |
| serviceCert | Widevine: Optional Others: N/R (leave unset) | String | The actual certificate string for Widevine purposes, which must be obtained out-of-band (OOB) by the app. Leave this unset unless Widevine is used for DRM. | Certificate strings are too long to display here. Examples can be fetched from such sources as the Widevine test license server at "[https://proxy.uat.widevine.com/proxy](https://proxy.uat.widevine.com/proxy). " |
| lic\_acq\_window | Widevine: Optional | string | The maximum amount of time (in milliseconds) that an app waits before rotating its Widevine DRM keys. The app can generate a random wait time between 0 and the value specified in the **lic\_acq\_window** field, and use the random wait time to instruct when the Video node should make its next Widevine license request. | 1000 |

*   when encodingType="PlayReadyLicenseAcquisitionAndChallenge", the EncodingKey attribute contains the PlayReady license acquisition URL plus additional custom license acquisition request data in format "URL%%%" Note, this is the same value that used to be specified directly in Content Metadata structure The app just needs to set drmParams.licenseSererUrl.

### Passing custom HTTP headers to licensing requests

Developers looking to pass custom HTTP headers with a licensing request can now set those headers using the [ifHttpAgent](/docs/references/brightscript/interfaces/ifhttpagent.md) interface methods on the [Video](/docs/references/scenegraph/media-playback-nodes/video.md) node.

### Example of configuring a dash stream with Widevine DRM

    contMeta = {
        HDPosterUrl:"pkg:/images/BigBuckBunny.jpg"
        SDPosterUrl:"pkg:/images/BigBuckBunny.jpg"
        ShortDescriptionLine1:"Parking Wars(VOD)"
        ShortDescriptionLine2:"dash | widevine"
        Streamformat:"dash"
        SwitchingStrategy:""
        MinBandwidth:500
        VideoUrl: "http://dev.domain.com/mm/dash/vod/173850/85768039/TG_W_WIFI.mpd"
        drmParams: { ' setting up DRM config
            keySystem: "Widevine"
            licenseServerURL: "http://msfrn-ci-cp-dev.mobitv.com/widevine/get_license"
        }
    }
    

Content classification attributes
---------------------------------

_Available since Roku OS 13.0_

Developers can use the **contentClassifier** content metadata attribute to specify the genre of their content (for example, action, sports, or comedy), and the Roku OS will use this attribute to automatically adjust the sound and picture on Roku TVs (if auto mode is selected for the picture or sound settings).

###### Content classifier value

| Attribute | Type | Values | Example |
| --- | --- | --- | --- |
| contentClassifer | string | *   " "<br>*   "action"<br>*   "animated"<br>*   "black+white" (black and white)<br>*   "comedy"<br>*   "drama"<br>*   "music"<br>*   "music:lyrics"<br>*   "nature"<br>*   "news"<br>*   "podcast" (audio only)<br>*   "reality"<br>*   "sports" | "drama" |

###### Content classifier sound and picture modes

The following table details how the different **contentClassifier** attribute values are mapped to sound and picture modes on Roku TVs.

| Content Classifier | Sound Mode | Picture Mode |
| --- | --- | --- |
| " " | Standard | Standard |
| action | Movie | Movie |
| sports | Standard | Sports |
| comedy | Movie | Movie |
| drama | Movie | Movie |
| music | Music | Standard |
| music:lyrics | Music | Low Power |
| news | Dialog | Vivid |
| podcast (Audio Only ) | Dialog | Low Power |
| animated | Movie | Vivid |
| black+white | Movie | Standard |
| nature | Standard | Vivid |
| reality | Standard | Standard |

Playback configuration attributes
---------------------------------

Playback configuration meta-data attributes are used to configure the playback of the content item.

AttributeTypeValuesExampleLiveBooleanOptional flag indicating video is live. Replaces time remaining in progress bar to display "Live". Default is falseTrueUrlStringStream URL for Scene Graph Video nodemysite.com/img/vacation.jpgSDBifUrlStringBIF URL for SD trick modemysite.com/bif/sd1932.bifHDBifUrlStringBIF URL for HD trick modemysite.com/bif/hd1932.bifFHDBifUrlStringBIF URL for FHD trick modemysite.com/bif/fhd1932.bifStreamroAssociativeArraySupported by roVideoPlayer and roVideoScreen, but not the Roku Scene Graph Video node.  
For the Video node, use the top level url, streamformat, etc. attributes.  
  
The exception is cases where you don't have adaptive streams (typically MP4) and need to specify different bitrate variants separately. For this use case use the Streams attribute. roAssociativeArray that has parameters representing the stream settings that were set as individual roArrays in previous firmware revisions.  
  
The old method is still supported and descriptions of the parameters can be found under those content-meta data entries.  
  
For url please see StreamUrls, for quality it is now a Boolean that is true for HD quality.  

| Key | Type |
| --- | --- |
| url | String |
| stickyredirects | Boolean |
| quality | Boolean |
| contentid | String |
| bitrate | Integer |

{ url : "[http://me.com/big.m3u8"](http://me.com/big.m3u8%22), quality : true, contentid : "big-hls" }StreamsroArray of roAssociativeArraysUsed by roVideoPlayer and roVideoScreen to specify the content metadata for a set of fixed bitrate streams.  
  
Each array item specifies the URL, bitrate, etc. for one stream variant.  
  
Setting stream content metadata using the Streams value is recommended for non-adaptive video (such as MP4 progressive download) only.  
  
For adaptive streaming, use the Stream metadata value.  

| Key | Type |
| --- | --- |
| url | String |
| stickyredirects | Boolean |
| quality | Boolean |
| contentid | String |
| bitrate | Integer |

\[ { url : "http://me.com/x-384.mp4", bitrate : 384, quality : false, contentid : "x-384" }, { url : "http://me.com/x2500.mp4", bitrate : 2500, quality : true, contentid : "x-1500" } \]StreamBitratesroArrayArray of bitrates in kbps for content streams used.  
  
Setting stream bitrates using this value is recommended for non-adaptive video (such as MP4 progressive download) only.  
  
**Must be used in conjunction with StreamUrls and StreamQualities**\[ 384, 500, 1000, 1500 \]StreamUrlsroArrayArray of URLs for content streams.  
  
Setting stream urls using this value is recommended for non-adaptive video (such as MP4 progressive download) only.  
  
**Must be used in conjunction with StreamBitrates and StreamQualities**\[ "mysite.com/vid/1932-1.mp4", "mysite.com/vid/1932-2.mp4", "mysite.com/vid/1932-3.mp4", "mysite.com/vid/1932-4.mp4" \]StreamQualitiesroArrayArray of Strings quality indicators identifying a stream as "SD" or "HD".  
  
**Must be used in conjunction with StreamBitrates and StreamUrls**\[ "SD", "SD", "SD", "HD" \]StreamContentIDsroArrayarray of strings values logged in Roku logs to identify stream and bitrate played\[ "myco-19321-384.mp4", "myco-19321-500.mp4", "myco-19321-1000.mp4", "myco-19321-1500.mp4" \]StreamStickyHttpRedirectsroArrayArray of Boolean values indicating if the HTTP endpoint should be sticky and not subject to change on subsequent requests. Default is false\[ false, false, false, false \]StreamStartTimeOffsetIntegerOptional. Default is 0. The offset into the stream which is considered the beginning of playback. Time in seconds.3600 (one hour)StreamFormatStringType of content

*   Type of content:
    
    *   Default: H.264/AAC in .mp4 Container
*   Valid values:
    
    *   "mp4" (mp4 will also accept .mov and .m4v files)
    *   "wma" (deprecated)
    *   "mp3"
    *   "hls" -"ism" (smooth streaming)
    *   "dash" (MPEG-DASH)
    *   "mkv", "mka", "mks"
*   Deprecated:
    
    *   "wmv"

LengthFloatMovie Length in Seconds; Length zero displays at 0m, Length not set will not display3600 (one hour)PlayStartFloatPlayStart defines the start position of the content, in seconds.  
  
The player is not allowed to move to a position prior to this point. Any seek operation prior to this point will be clipped to PlayStart.  
  
Apps can use PlayStart and PlayDuration to split one content piece into multiple clips and insert these clips with other content (typically advertisements) into one content list.  
  
Starting from Roku OS 8.0, content metadata supports negative PlayStart values. This feature allows the media players to start playbacks distanced from the edge of the live stream0ClosedCaptionsBooleanBoolean indicating if CC icon should be displayedTrueHDBrandedBooleanBoolean indicating if HD branding should be displayedTrueIsHDBooleanBoolean indicating if content is HDTrueSubtitleColorStringTheme metadata attribute that specifies the color to use when rendering subtitle text"#FF0000"SubtitleConfigroAssociativeArray: {TrackName : String}Specifies the caption settings for content playback.  
  
TrackName sets the name of the caption track to render. This string is a concatenation of the track source and track id, separated by a "/".  
  
Valid track sources are: "ism", "mkv", "eia608" and "dvb".  
  
The track id must match the track identifier in the smooth or mkvmanifest. For example, if an mkvfile has a caption track called “english1” the TrackName to select this track is “mkv/english1”.  
  
When the track source is "dvb", the track id is the three-letter language code, with "\_sdh" appended for subtitles for the deaf and hard of hearing. For example, "dvb/eng\_sdh" are English subtitles for the deaf and hard of hearing and "dvb/nor" are normal Norwegian subtitles.  
  
For sideloaded caption tracks, the TrackName is the url from where the caption track can be downloaded.Sideloaded caption formats can include srt,ttml, anddfxp. Specifying eia608/1 will trigger the Roku OS to search for embedded 608/708 captions in the stream. In the 8.0 Roku OS, automatic track selection based on a preferred caption language setting is introduced. Omit setting a URL here to avoid interfering with the automatic track selection. It is sufficient to add the URLs to SubtitleTracks{ TrackName : "mkv/english1" }SubtitleTracksroArray of roAssociativeArray: \[{Language : String, Description : String, TrackName : String},...\]SubtitleTracks sets the list of available caption tracks available to the stream. This list is added to the track list in the closed caption configuration dialog that is displayed during video playback when the user presses the Roku remote control \* button. The captions from the selected track are then displayed on the screen. Language specifies the ISO 639.2B 3 character language code. This string is used to match the proper caption track with the audio language. Description specifies the text that will be shown for the corresponding track in the closed caption configuration dialog. For sideloaded caption tracks the TrackName is the URL from where the caption track can be downloaded. Sideloaded caption formats can include srt, ttml, and dfxp. The SubtitleTracks metadata is generally only used for side loaded captions. the Roku OS detects in-stream captions and thus specifying SubtitleTracks in this case is not necessarySubtitleUrlStringSpecifies the path to an SRT or TTML formatted file used to render subtitles or closed captions, respectively. This is supported on roVideoScreen only. See [Closed Caption Support](/docs/developer-program/media-playback/closed-caption.md) for additional details"mysite.com/vid/1932.srt"; "mysite.com/vid/1932.xml"VideoDisableUIBooleanIf set to true, hides the Scene Graph Video node trick play UI; If set to false (the default) shows the Scene Graph Video node trick play UIvideo = createObject("roSGNode", "Video"); video.content.VideoDisableUI = trueEncodingTypeStringSpecifies the encoding scheme for PlayReady DRM, by setting to one of the following values:

*   "PlayReadyLicenseAcquisitionUrl"
*   "PlayReadyLicenseAcquisitionAndChallenge" Note, this is the same value that used to be specified directly in Content Metadata structure

EncodingKeyStringSpecifies the PlayReady license acquisition URL, and additional custom request data, determined by the EncodingType attribute value specified:

*   when encodingType="PlayReadyLicenseAcquisitionUrl", the EncodingKey attribute contains the PlayReady license acquisition URL

SwitchingStrategyStringroVideoPlayer or roVideoScreen.  
  
Specify different stream switching algorithms to be used in HLS adaptive streaming.  
Only has an effect on HLS streams. "full-adaptation" uses measured bandwidth and buffer fullness to determine when to switch. This strategy requires that segments align across variants as the HLS spec requires. This is the new default"full-adaptation"WatchedBooleanFlag indicating if content has been watchedTrueForwardQueryStringParamsBooleanControls whether query string parameters from initial HLS stream manifest requests are forward to subsequent segment download requests. The default value is set to true for backwards compatibility.TrueForwardDashQueryStringParamsBooleanControls whether query string parameters from initial DASH stream manifest requests are forward to subsequent segment download requests. The default value is set to false for backwards compatibility.FalseIgnoreStreamErrorsBooleanWhen set to true the media player will not stop playback when it runs into a streaming related error for this content. Instead, it will skip to the next item in the content list.  
  
If this was the last item in the content list the media player will send a regular completion event (like isFullResult). Apps are still notified of any errors via an isRequestFailed notification but a new attribute in the event’s GetInfo object tells the app the error was ignored.  
  
See the changes related to isRequestFailed for more information. The default value is false.

    video_details = {
        streamFormat: "mp4"
        ignoreStreamErrors: true
        streams: [{bitrate: 537, height: 360, width: 640, url: “https://..."}]
    }

AdaptiveMinStartBitrateIntegerMinimum startup bitrate specified in kbps. Streaming will start with a variant equal to or greater than this value. If this value is not set or if it's set to zero, any minimum start bitrate will be ignored.5000AdaptiveMaxStartBitrateIntegerMaximum startup bitrate specified in kbps. Streaming will start with a variant less than or equal to this value. If this value is not set, it will default to 2500 kbps.2000filterCodecProfilesBooleanFilters out any video profile/codec level combinations that the specified hardware cannot play. The default value is false, in which case no filtering occurs. **Note that this currently only works for DASH streams.**TrueLiveBoundsPauseBehaviorStringAllows an app to customize Media Player behavior on live streams when playing in the earliest part of a DVR buffer.  
  
The stream remains paused even though it is playing in the earliest part of the buffer of a live stream when the value of the attribute is set to "pause." This enables the Roku OS to distinguish between live streams and live streams that eventually transition to video on demand.  
  
The possible values of this attribute are "resume", "stop", "pause", with resume being the default value.  
  
**Currently, this attribute will work only with Smooth and Dash streams.** (Available since Roku OS 8.1)ResumeClipStartFloatClipStart sets the clip start position of the playback. The unit of ClipStart is seconds (Available since Roku OS 8.1).00.0ClipEndFloatClipEnd sets the clip end position. The unit of ClipEnd is seconds (Available since Roku OS 8.1).00.0PreferredAudioCodecStringSpecifies the audio codec that should be used during playback. The Media Player will select and report to the app only those audio renditions that are encoded with the specified codec. Renditions that are encoded with a different codec are ignored. Possible values of this attribute are "aac", "ac3" and "eac3"."aac"AudioWhitelistStringComma-separated list of audio tracks (based on ISO 639-1 or ISO 639-2 language code) that may be selected from the **Audio track** setting for the content.  
  
"en, spa"AudioBlacklistStringComma-separated list of audio tracks (based on ISO 639-1 or 639-2 language code) that may not be selected from the **Audio track** setting for the content.  
  
(Available since Roku OS 9.4)  
  
If a language is both blacklisted and whitelisted, the blacklisting takes precedence."ita, fr"CaptionWhitelistStringComma-separated list of captioning tracks (based on ISO 639-2 language code) that may be selected from the **Accessibility>Captioning track** setting for the content.  
  
"en, spa"CaptionBlacklistStringComma-separated list of captioning tracks (based on ISO 639-2 language code) that may not be selected from the **Accessibility>Captioning track** setting for the content.  
  
(Available since Roku OS 9.4)  
  
If a language is both blacklisted and whitelisted, the blacklisting takes precedence."deu, dan"

CDN switching
-------------

Content Delivery Networks (CDNs) can be switched during playback to load balance traffic and failover to different servers in order to help optimize performance. The **CdnConfig** attribute can be used for managing load balancing and failovers.

AttributeTypeValuesDescriptioncdnConfigroArray of roAssociativeArrays

| Key | Required/ Optional | Type | Description |
| --- | --- | --- | --- |
| URLFilter | Required | String | A substring that identifies the (base)URL to which these CDN settings apply.  <br>  <br>The Roku media player matches this string against all (base)URLs listed in the manifest and applies the setting to all (base)URLs that contain this substring. |
| ContentFilter | Optional | String | For DASH streams, a substring that filters the period or asset ID to which these CDN settings apply.  <br>  <br>The Roku player only applies these CDN setting to periods with a period ID or asset ID that contains this substring.  <br>  <br>This match is used in addition to the URL filter. |
| Priority | Required | Integer | For configuring failovers, sets the priority for this (base)URL from 1 to x (a priority of 0 or less is invalid).  <br>  <br>A lower value indicates a higher priority. For example, a (base)URL with a priority of 1 is higher than another with a priority of 10.  <br>  <br>If the highest priority server fails, traffic is routed to the server with the next highest priority. If all servers are configured with the same priority, and one fails, no failover will happen. |
| Weight | Required | Integer | For configuring load balancing, sets the relative weight for all (base)URLs with the same priority. This must be a value of 1 or greater (a weight of 0 disables a CDN).  <br>  <br>The weight of a given BaseURL is its weight value divided by the sum of all weight values. This means that to spread the load equally across multiple CDNs with the same priority, set the weight for each to the same value. To configure the weights for two servers to 80% and a third server to 20%, for example, set servers one and two to 8 and server three to 4. |
| ServiceLocation | Optional | String | A blacklist of failed BaseURL locations. |

To use this field, create a child node and use a playlist (even though only one content item will be in the playlist). This field is updated only when **contentIsPlayList** is true.  
  
The **URLFilter**, **Priority**, and **Weight** attributes must be specified to apply these configurations.

**Example**

    this.cur_clip.CDNConfig = [
        {URLFilter:"http://cdn1.xyz.com/abc/", ContentFilter, “testProgram”, priority: 1, weight: 50, serviceLocation: "west"},
        {URLFilter:"http://cdn2.xyz.com/abc/", ContentFilter, “testProgram”, priority: 1, weight: 50, serviceLocation: "east"},
        {URLFilter:"http://cdn1.xyz.com/abc/", ContentFilter, “testProgram”, priority: 2, weight: 50, serviceLocation: "west"},
        {URLFilter:"http://cdn2.xyz.com/abc/", ContentFilter, “testProgram”, priority: 2, weight: 50, serviceLocation: "east"},
    ]
    

SceneGraph certificate attributes
---------------------------------

The SceneGraph certificate meta-data attributes are used to configure the use of HTTP certificates and cookies by the Audio and Video nodes. Please note that when setting any of the following four attributes on a Video or Audio node, you need to be careful that the values are set on the correct HTTPAgent. If the node does not have its own HTTPAgent, set by explicitly calling setHttpAgent() on the node, the Roku OS will traverse up the scene graph hierarchy until it finds the first node in the Video or Audio node's ancestry that has set an HTTPAgent. If none is found, the values will be set on the global HTTPAgent which is always guaranteed to exist. Therefore if you expect the header, etc. values set to only apply to your Audio and Video nodes, create a unique instance of roHttpAgent for them and assign it directly. For example, for a Video node you should do the following:

    'Assume video is a valid Video node instance
    
    httpAgent = CreateObject("roHttpAgent")
    video.setHttpAgent(httpAgent)
    

| Attribute | Type | Values |
| --- | --- | --- |
| HttpCertificatesFile | uri | If set, the Scene Graph Audio or Video node loads this public certificate bundle, to authenticate the server. The protocol must be https for this to have any effect. When used with a Scene Graph Audio or Video node, the node or global HttpAgent is found, as explained elsewhere in this documentation. When playing this content, the agent is updated in the following manner:<br><br>*   If this attribute is defined, the file URI is set into the HttpAgent instance. However, if this attribute is specified and the value is the empty string (""), then no changes will be made to the HttpAgent.<br>*   If this attribute is not defined, the behavior depends upon whether the Content Meta-Data (CMD) contains secure (https) URLs:<br>    <br>    *   If no secure URLs exist in the meta-data, then no certificates file path is set into the agent.<br>    *   If a secure URL does exist, the platform's default certificates are set into the agent. |
| HttpCookies | array of strings | If set, the Scene Graph Audio or Video node send the cookies to the server. Each cookie must have the following syntax: dom=domain;path=path;name=name;val=value; When used with a Scene Graph Audio or Video node, the node or global HttpAgent is found, as explained elsewhere in this documentation. When this Content Meta-Data is played and this attribute is set, all HTTP cookies in the agent are cleared and replaced with the cookies defined by this attribute |
| HttpHeaders | array of strings | If set, the Scene Graph Audio or Video node sends these headers to the server. Each string must be of the format "name:value". When used with a Scene Graph Audio or Video node, the node or global HttpAgent is found, as explained elsewhere in this documentation. When this Content Meta-Data is played and this attribute is set, all HTTP headers in the agent are cleared and replaced with the headers defined by this attribute |
| HttpSendClientCertificate | Boolean | If true, the Scene Graph Audio or Video node sends the client device certificate to the server, for client authentication. The protocol must be https for this to have any effect. When used with a Scene Graph Audio or Video node, the node or global HttpAgent is found, as explained elsewhere in this documentation. When this Content Meta-Data is played and this attribute exists, the value of this attribute (true or false) is set into the HttpAgent |

### drmHttpAgent for handling DRM key/license requests separately

Since Roku OS 9.3, you can create a separate agent to handle DRM key and license requests, apart from other types of requests.

Once you have created your agent, you can set the Video node's `drmHttpAgent` field directly to designate that the special agent is to supersede any currently-set agent in the case of DRM key and license requests. The `drmHttpAgent` field must be configured before setting the content in the Video node.

    ' Configure the DRM HttpAgent before setting content in the Video node
     httpAgent = CreateObject("roHttpAgent")
     httpAgent.AddHeader("DRM-Specific-1", "weqweqweqweqweqweqeqeqeqeqwe")
     httpAgent.AddHeader("DRM-Specific-2", "fgfgfgfgfgfgfgfgfg")
     httpAgent.AddHeader("DRM-Specific-3", "zxzxzxzxxzxzxzxzxzx")
     m.video.drmHttpAgent = httpAgent    
     m.video.content = videocontent
    

If `drmHttpAgent` is not set (the default), uri fetches for video involving the DRM URLs (`serializationURL`, `licenseServerURL`, `licenseRenewURL`) of ContentMetaData will use the video's regular HttpAgent. However, if the `drmHttpAgent` is set, the agent cited in the field will be used for those fetches instead.

> The "SceneGraph Certificate Attributes" mentioned above all have "Drm" versions, with names formed by the prefixing "Drm" to the "regular" names (e.g., `HttpCookies` becomes `DrmHttpCookies`, and so forth). These attributes take precedence over those of the drmHttpAgent.

Playback control attributes
---------------------------

The playback control meta-data attributes are used to control the playback parameters for the content item.

| Attribute | Type | Values | Example |
| --- | --- | --- | --- |
| MinBandwidth | Integer | roVideoPlayer or roVideoScreen: Will only select variant streams with a bandwidth higher than this minimum bandwidth. Units are kbps. By default Wowza servers set streams to 64 kbs, so you might want to set this parameter to something smaller than 64 when first testing Wowza streams. You will eventually want to specify the Wowza bitrates with a smil file (Please see the encoding guide) | 48  |
| MaxBandwidth | Integer | roVideoPlayer or roVideoScreen: Will only select variant streams with a bandwidth less than this maximum bandwidth. Units are kbps | 2500 |
| AudioPIDPref | Integer | **This attribute is deprecated**  <br>  <br>Users can select their preferred audio language on-device in the **Settings > Audio > Audio Preferred Language** screen. | 483 |
| FullHD | Boolean | roVideoPlayer or roVideoScreen: Specify that this stream was encoded at 1080p resolution | true |
| FrameRate | Integer | roVideoPlayer or roVideoScreen: Specify the 1080p stream was encoded at 24 or 30 fps | 24  |

Track ID attributes
-------------------

| Attribute | Type | Values | Example |
| --- | --- | --- | --- |
| TrackIDAudio | String | roVideoPlayer or roVideoScreen: Used in SmoothStreaming (StreamFormat = "ISM") to specify. Set the TrackIDAudio field to the desired track's StreamIndex.Name attribute from the manifest file | "Spanish" |
| TrackIDVideo | String | roVideoPlayer or roVideoScreen: Used in SmoothStreaming (StreamFormat = "ISM") to specify. Set the TrackIDVideo field to the desired track's StreamIndex.Name attribute from the manifest file | "h264video" |
| TrackIDSubtitle | String | roVideoPlayer: Used to specify a closed caption track in a video stream that supports 608/708 captions | "eia608/1" |
| AudioFormat | String | roSpringboardScreen: If set to "dolby-digital", will display a "5.1 ))" icon in the lower left of a movie style springboard screen | "dolby-digital" |
| AudioLanguageSelected | String | **This attribute was deprecated as of the Roku 9.2 OS release.**  <br>  <br>Users can select their preferred audio language on-device in the **Settings > Audio > Audio Preferred Language** screen. | "eng" |

roListScreen attributes
-----------------------

| Attribute | Type | Values | Example |
| --- | --- | --- | --- |
| SDBackgroundImageUrl | String | roListScreen: URL for the SD background image | mysite.com/images/bg1\_sd.jpg |
| HDBackgroundImageUrl | String | roListScreen: URL for the HD background image | mysite.com/images/bg1\_hd.jpg |

Rating attribute icons
----------------------

The Rating attribute contains the MPAA or TV rating stored as a string. At runtime, the ratings are shown with an icon instead of rendering the string as text. The following table shows the list of valid values for the Rating attribute, and the resulting icon that will be displayed for each value.

| Value | Icon |
| --- | --- |
| G   | ![roku815px - G rating](https://image.roku.com/ZHZscHItMTc2/g.png "g-rated") |
| NC-17 | ![roku815px - NC17 rating](https://image.roku.com/ZHZscHItMTc2/contentmetadata2.png "nc17-rated") |
| PG  | ![roku815px - PG rating](https://image.roku.com/ZHZscHItMTc2/contentmetadata3.png "pg-rated") |
| PG-13 | ![roku815px - PG-13 rating](https://image.roku.com/ZHZscHItMTc2/contentmetadata4.png "pg13-rated") |
| R   | ![roku815px - R rating](https://image.roku.com/ZHZscHItMTc2/contentmetadata5.png "r-rated") |
| UR  | ![roku815px - UR](https://image.roku.com/ZHZscHItMTc2/ur.png "ur-rated") |
| UNRATED | ![roku815px - Unrated rating](https://image.roku.com/ZHZscHItMTc2/ur.png "ur-rated") |
| NR  | ![roku815px - Not rated](https://image.roku.com/ZHZscHItMTc2/contentmetadata7.png "nr-rated") |
| TV-Y | ![roku815px - TV-Y rating](https://image.roku.com/ZHZscHItMTc2/contentmetadata8.png "tv-y-rated") |
| TV-Y7 | ![roku815px - TV-Y7 rating](https://image.roku.com/ZHZscHItMTc2/tvy7.png "tv-y7-rated") |
| TV-Y7-FV | ![roku815px - TV-Y7-FV rating](https://image.roku.com/ZHZscHItMTc2/contentmetadata10.png "tv-y7-fv-rated") |
| TV-G | ![roku815px - TV-G rating](https://image.roku.com/ZHZscHItMTc2/tvg.png "tv-g-rated") |
| TV-PG | ![roku815px - TV-PG rating](https://image.roku.com/ZHZscHItMTc2/contentmetadata12.png "tv-pg-rated") |
| TV-14 | ![roku815px - TV-14 rating](https://image.roku.com/ZHZscHItMTc2/contentmetadata13.png "tv-14-rated") |
| TV-MA | ![roku815px - TV-MA rating](https://image.roku.com/ZHZscHItMTc2/contentmetadata14.png "tv-ma-rated") |

Content feed video lesson
-------------------------

You can learn how to link the content metadata in your app's feed to a ContentNode by watching the [Creating the content feed](/videos/courses/rsg/content-feed.md) video lesson in Roku's [SceneGraph: Build a Channel online video course](https://developer.roku.com/videos/courses/rsg/overview.md).