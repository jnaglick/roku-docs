Roku advertising requirements
=============================

This document lists the requirements for displaying video and interactive ads in a channel. These requirements are applicable for both client-side and server-side ad requests. Channels must adhere to these requirements to pass certification, including those related to the Roku Advertising Framework (RAF).

Roku advertising framework (RAF) requirements
---------------------------------------------

### RAF 1 Integration requirements

Channels must integrate the following RAF-related requirements to pass certification:

| Requirement | Description | Documentation |     |
| --- | --- | --- | --- |
| RAF 1.1 | RAF integration | Channels must integrate RAF for all ads without modifying, obstructing, or disabling RAF functionality in any way. Replays of live broadcast streams are exempt from this requirement, unless dynamic ad insertion is used to insert new ads. | [RAF integration guide](/docs/developer-program/advertising/roku-advertising-framework.md) |
| RAF 1.2 | Measurement beacons | Channels must fire all measurement beacons client-side via RAF. This requirement is applicable for both client-side and server-side ad insertion. | [Roku Advertising Watermark integration guide](/docs/developer-program/advertising/ad-watermark.md) |
| RAF 1.3 | Audience measurement | Channels in the U.S. Channel Store only that are not child-directed must support Roku ad tracking by calling the [enableAdMeasurements()](/docs/developer-program/advertising/raf-api.md#enableadmeasurementsenabled) method and passing the required content metadata within the following methods: [setContentGenre()](/docs/developer-program/advertising/raf-api.md#setcontentgenregenres-as-string-kidscontent-as-boolean), [setContentId()](/docs/developer-program/advertising/raf-api.md#setcontentgenregenres-as-string-kidscontent-as-boolean), and [setContentLength()](/docs/developer-program/advertising/raf-api.md#setcontentlengthlength-as-integer). Optionally, apps may use the [setNielsenGenre API](/docs/developer-program/advertising/raf-api.md#setnielsengenregenre-as-string) to pass specific Nielsen Genre granularity and the [setNielsenAppId API](/docs/developer-program/advertising/raf-api.md#setnielsenappidid-as-string) for those who specify a custom Nielsen App ID. The [enableAdMeasurements](/docs/developer-program/advertising/raf-api.md#enableadmeasurementsenabled) method deprecates the [enableNielsenDAR](/docs/developer-program/advertising/raf-api.md#nielsen-dar) API; therefore, do not use the [enableNielsenDAR](/docs/developer-program/advertising/raf-api.md#nielsen-dar) API. | [General Audience Measurement](/docs/developer-program/advertising/raf-api.md#general-audience-measurement) |
| RAF 1.4 | Ad break - numbering | For ads inserted client-side, apps must display the number of ads during ad breaks using the standard Roku-branded label applied by RAF. | [RAF integration guide](/docs/developer-program/advertising/integrating-roku-advertising-framework.md) |

General advertising requirements
--------------------------------

### ADS 1 General integration requirements

| Requirement | Name | Description | Documentation |
| --- | --- | --- | --- |
| ADS 1.1 | SDKs and libraries | Partners must disclose integration/use of all non-Roku SDKs, libraries, or other software systems and external advertising partners (for example, DSPs) that enable video, audio, or banner ad insertion, and Roku has the right to approve or deny such non-Roku SDKs, libraries, or other software systems. | [Roku Advertising Framework overview](/docs/developer-program/advertising/roku-advertising-framework.md) |
| ADS 1.2 | Ad terms | Channels that have an inventory relationship with Roku must meet the advertising terms specified in all applicable agreements. | [Video Advertising](/docs/features/monetization/video-advertisements.md) |
| ADS 1.3 | Ad experience | Channels selling ads exclusively and/or with Roku must comply with ad load, ad frequency, and acceptable ad requirements. | [Roku Advertising Guidelines](http://www.roku.com/adguidelines) |
| ADS 1.4 | Demand API | Channels in the U.S. Channel Store that have both streamed more than an average of 100,000 hours per month and averaged more than 10,000 new installs per month over the last three months may be required to implement the Demand API as part of their integration (this requirement may also be applicable to new apps projected to reach the specified thresholds shortly after launch).  <br>  <br>Channels outside the U.S. Channel Store that have streamed more than an average of 200,000 hours per month over the last three months, and new apps outside the U.S. Channel Store that are projected to reach this threshold, may also be required to implement the Demand API. | [Implementing the Demand API](/docs/developer-program/advertising/demand-api.md) |
| ADS 1.5 | RFI screen for authenticated ad-monetized apps | Authenticated ad-monetized apps must use the [getUserData](/docs/references/scenegraph/control-nodes/channelstore.md#getuserdata) command to display a Request For Information (RFI) screen during the sign-up and sign-in workflows to enable customers to share their Roku account information with the channel. Only if the user declines the request may apps require the customer to manually enter their information. | [Signup requirements and best practices](/docs/developer-program/roku-pay/signup-best-practices.md)  <br>  <br>[Sign-in requirements and best practices](/docs/developer-program/roku-pay/signin-best-practices.md) |

### ADS 2 Privacy requirements

| Requirement | Name | Description | Documentation |
| --- | --- | --- | --- |
| ADS 2.1 | Roku ID for Advertisers (RIDA) identifier Limit Ad Tracking (LAT) flag | Channels must pass Roku's ID for Advertisers (RIDA) and "limit ad tracking" (LAT) value on ad server requests. If the user has opted out, apps must still pass the temporary ID returned by the [rodeviceInfo.GetRida()](/docs/references/brightscript/interfaces/ifdeviceinfo.md#getrida-as-string) function to support frequency capping (this temporary ID is different than the UUID returned if the user has not opted out; it expires after 30 days). | [GetRida()](/docs/references/brightscript/interfaces/ifdeviceinfo.md#getrida-as-string)  <br>  <br>[IsRIDADisabled()asBoolean](/docs/references/brightscript/interfaces/ifdeviceinfo.md#isridadisabled-as-boolean)  <br>  <br>[URL parameter macros](/docs/developer-program/advertising/integrating-roku-advertising-framework.md#url-parameter-macros) |
| ADS 2.2 | Child-directed content | Channels with child-directed content must make ad requests that indicate that content is child-directed when serving ads during child-directed content. | [kidsContent parameter in the setContentGenre() method](/docs/developer-program/advertising/raf-api.md#setcontentgenregenres-as-string-kidscontent-as-boolean)  <br>  <br>[ROKU\_ADS\_KIDS\_CONTENT URL parameter macro](/docs/developer-program/advertising/integrating-roku-advertising-framework.md#url-parameter-macros) |

### ADS 3 Ad request requirements

| Requirement | Name | Description | Documentation |
| --- | --- | --- | --- |
| ADS 3.1 | Channel ID | Channels must pass their Roku channel ID in ad server requests to Roku. | [roChannelInfo.getId() function](/docs/references/brightscript/interfaces/ifappinfo.md#getid-as-string)  <br>  <br>[ROKU\_ADS\_APP\_ID URL parameter macro populated by RAF](/docs/developer-program/advertising/integrating-roku-advertising-framework.md#url-parameter-macros) |
| ADS 3.2 | User agent | Channels must use the Roku-generated device user agent in all server-side ad requests. | [RAF integration guide](/docs/developer-program/advertising/integrating-roku-advertising-framework.md#3-user-agent-requirements) |

### ADS 4 Ad break playback requirements

| Requirement | Name | Description | Documentation |
| --- | --- | --- | --- |
| ADS 4.1 | Ad break - back button behavior | All apps (except those streaming live content or replaying live broadcast streams) must return to the previous screen when the back button is pressed during an ad break (if the channel can't return to the previous screen, the channel displays an exit confirmation dialog).  <br>  <br>Channel s must attempt to initiate an ad break to preserve the previously exited ad experience when playback resumes (with the same or different content). | [RAF integration guide](/docs/developer-program/advertising/integrating-roku-advertising-framework.md) |
| ADS 4.2 | Ad break - FF/REW commands | All apps (except those streaming live content or replaying live broadcast streams) must ignore FF/REW commands received during an ad break (via either key presses or voice commands). | [RAF integration guide](/docs/developer-program/advertising/integrating-roku-advertising-framework.md) |
| ADS 4.3 | Ad break - pause | All apps (except those streaming live content or replaying live broadcast streams) may only display static graphics that do not contain any video (for example, banner ads) when an ad is paused. Channels are prohibited from displaying video ads when an ad break is paused. | [RAF integration guide](/docs/developer-program/advertising/integrating-roku-advertising-framework.md) |