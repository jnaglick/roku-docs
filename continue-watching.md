Continue Watching
=================

Continue Watching is a content category row within the **What to Watch** home screen navigation on Roku devices and on the Home screen of the Roku mobile app. It displays content from participating apps that customers have already started watching, which empowers customers with the speed and convenience of a single location from which they can resume content from different apps on any Roku device linked to their account. Publishers can integrate into this feature to make their content more accessible to customers, drive users to their apps, and increase engagement. Overall, this helps publishers promote their content in order to retain customers and reduce churn.

![roku815px - continue watching row](https://image.roku.com/ZHZscHItMTc2/continue-watching-ui-v2.png)

> The Continue Watching feature is available on all Roku devices running Roku OS 11.0 or higher in the United States, Canda, United Kingdom, Germany, Mexico, Chile, Argentina, and Colombia.
> 
> Apps in the U.S. Streaming Store that have streamed more than an average of 5 million hours per month over the last three months must participate in Roku’s Continue Watching program to pass [certification](/docs/developer-program/certification/certification.md#4-channel-operation). This requirement also applies to new apps projected to reach the specified streaming hours threshold shortly after launch. TVOD, live linear, and made-for-kids apps are excluded from this requirement.
> 
> Continue Watching only supports long-form content such as movies and television episodes. Short-form content (standalone content that is 15 minutes or less that is not a movie or TV show) is not supported.

Overview
--------

From the Roku home screen, customers can scroll down to the **What to Watch** screen. This screen features a **Continue Watching** row with content from participating apps, including movies and TV shows that the customer needs to finish, live linear apps that the customer was watching, and the next episodes in a television series. The Continue Watching row contains a maximum of 40 tiles, which are ordered based on Roku-proprietary algorithms that use various signals, including recency.

> The Continue Watching row is also available on the Home screen of the Roku mobile app.

As customers browse content, metadata for the item with focus is displayed. This includes the publisher's logo, title, release date, rating, and duration. In addition, if the app supports user profiles, a label indicates the user that was watching that content item. A progress bar indicates the approximate playback position of the content item.

When the customer selects a movie, TV show, or TV episode, it launches directly into playback (apps may not launch into a profile selection screen, content details screen, or any other screen when content is selected from the Continue Watching row). For customers with multiple Roku devices linked to their account, Continue Watching resumes playback at the bookmarked position on any of their devices.

Prerequisites
-------------

Apps must have completed the following integrations to participate in Roku Continue Watching:

1.  [Roku Search](/docs/developer-program/discovery/search/implementing-search.md). Enables customers to find content on your app.
2.  [Deep linking](/docs/developer-program/discovery/implementing-deep-linking.md). Enables the requested content to be launched directly into playback on your app.
3.  [Bookmarking](/docs/developer-program/media-playback/bookmarking.md). Resumes playback of the requested content at its last watched position.

Getting started
---------------

To get started with the Continue Watching integration, follow these steps:

1.  Verify that your app meets the listed [prerequisites](#prerequisites).

2.  Contact the [Roku Partner Success team](https://developer.roku.com/contact). They will determine whether your app is eligible for Continue Watching and enable your app to access the Continue Watching service endpoints.

3.  Request device tokens for testing the Continue Watching integration in a sideloaded environment. Provide Roku Partner Engineering with the serial numbers of the Roku devices to be used for testing during development. Upon receiving the device tokens, install them on their respective test devices by entering the following cURL command in a terminal application:
    
    `curl --data-binary @<token-file> http://<Roku_IP>:8060/token/install`
    
    The response from the terminal application should be as follows:
    
         <TokenCmdResponse>
             <Command>Install</Command>
             <Response>Success</Response>
         </TokenCmdResponse>
        
    
4.  Once development has been completed, request authentication tokens for testing the Continue Watching integration in a beta environment. This enables you to complete QA testing before releasing your updated app to production. Provide Roku Partner Engineering with the list of the app IDs to be used for QA testing. Upon receiving the app token, add it to the [manifest](/docs/developer-program/getting-started/architecture/channel-manifest.md) (channel\_token=). The **channel authentication token** will also be used in your production application.
    

Integrating into Continue Watching
----------------------------------

Integrating into Continue Watching entails calling the Roku Continue Watching APIs when a playback event occurs. Playback events occur when the customer exits the video player or finishes watching content. Sending events identifies which content the customers can keep watching and where to resume playback. Publishers can use the Roku Continue Watching APIs to add, update, and remove content items. The workflow is illustrated and summarized as follows:

![roku815px - cw-api-flow](https://image.roku.com/ZHZscHItMTc2/continue-watching-api-flow-v1.jpeg)

| Step | API | Description |
| --- | --- | --- |
| **Retrieve bookmarks when app is launched** | Publisher backend system | The publisher maintains the playback position of content. Roku does not maintain bookmarks because content may be watched across multiple platforms (for example, web and Roku). This ensures that deep links from the Continue Watching row return the customer to the actual playback position. |
| **Update bookmark** | PUT request to Continue Watching API | Once the publisher retrieves the current playback position from their backend system, the app makes a **PUT** request to update the Continue Watching row with that bookmark. |
| **Add content to Continue Watching row when content playback starts** | POST request to Continue Watching API | The publisher controls how long content has been watched (for example, one minute) before it is added to the Continue Watching row. Once the publisher-configured interval has been reached, the app makes a POST request to add the content to the Continue Watching row.  <br><br>During playback, do not make Continue Watching API calls to update the playback position. The main purpose of the Continue Watching user experience is to aggregate in-progress content and streamline resumption. The progress bar used to reflect the current bookmark in the Continue Watching row is an approximation. If the customer presses the Home button after the POST request has been sent, the content will still be listed in the Continue Watching row, which is the primary goal of the feature. |
| **Update content playback position when content playback ends** | POST request to Continue Watching API | Once the customer stops content playback, the app makes a **POST** request to update the Continue Watching row the current bookmark for that content. |
| **Remove content from Continue Watching row when content has been completed** | DELETE request to Continue Watching API | The publisher controls what constitutes the completion of content (for example, end credits are shown). Once content has been completed, the app makes a DELETE request to remove the content from the Continue Watching row. |

### API Reference

The following table summarizes the basic information for the Continue Watching RESTful web services:

| Item | Description |
| --- | --- |
| **Endpoint** | The base URLs for the Continue Watching APIs are as follows:  <br><br>*   [https://userdata.sr.roku.com/user-data/v1/content/continueWatching](https://userdata.sr.roku.com/user-data/v1/content/continueWatching)<br>*   [https://userdata.sr.roku.com/user-data/v1/profile/{**profileId**}/content/continueWatching](https://userdata.sr.roku.com/user-data/v1/profile/%7B**profileId**%7D/content/continueWatching) (use this endpoint if your app has a profile selection screen and the content being passed is specific to the provided **profileId** (the unique user ID \[UUID\] of the user profile). The **profileId** is passed back to the app in a deep link request from the Continue Watching feature. |
| **Protocol** | Continue Watching API calls may only be sent using HTTPS. |
| **Methods** | The Continue Watching APIs support the following REST methods for adding, retrieving, updating, and deleting content items:  <br><br>*   **POST**. Add one or more new content items; update existing items.<br>*   **GET**. Retrieve the existing list of content items.<br>*   **PUT**. Replace the entire existing list of content items. When making this request, include all the content that should remain in the Continue Watching row (for example, a PUT request with a single item replaces the current list with that one item). Passing an empty body removes all content from the list.<br>*   **DELETE**. Remove one or more content items. |
| **Header** | Requests to the Continue Watching APIs require the following headers (the Roku OS automatically populates the headers with empty string values):<br><br>*   **Content-Type:** application/json<br>*   **x-roku-reserved-jwt**: ""<br>*   **x-roku-reserved-channel-id**: "" (the production app ID is required to sideload and test the app during development because it is linked to the search feed. In production, the Roku OS will override this value, which means you can continue passing it after development has been completed).<br>*   **x-roku-reserved-channel-store-code**: ""<br>*   **x-roku-reserved-virtual-user-id**: ""<br>*   **x-roku-reserved-device-id**: ""<br>*   **x-roku-reserved-serial-number**: ""<br><br>> See [Appendix A](#appendix-a-sample-brightscript-code-for-adding-http-headers) for sample BrightScript code that demonstrates how to add these headers to your app application. Do not use the [roHttpAgent.setHeaders()](/docs/references/brightscript/interfaces/ifhttpagent.md#setheadersnamevaluemap-as-object-as-boolean) function to pass the headers. |
| **Response** | The Continue Watching APIs return one of the following response codes:  <br><br>*   **200**: OK<br>*   **204**: No content (DELETE requests only)<br>*   **400**: Bad request (required fields are missing from the payload; a description of the error is returned)<br>*   **401**: Unauthorized (DELETE requests only)<br>*   **403**: Forbidden (if an invalid partner) |

### Add API

To add new content items and update existing ones to the Continue Watching row, send a **POST** request to the Continue Watching API with the following parameters in the JSON body:

ParameterTypeRequired/OptionalDescriptioncontentIdStringRequiredThe ASCII string (maximum 255 characters) used to uniquely identify the content in your app.  
  
This maps directly to the playID (contentId) field in the [Roku Search feed specification](/docs/specs/search/search-feed.md) or contentId for any search implementations using externalId providers.  
  
For a TV series, the seriesId maps to the corresponding seriesId field in Search feed spec.  

> For a TV series, you must pass (1) the seriesId (the ID of series asset) in the **contentId** field and (2) the playID of the episode in the **episodeId** field.

episodeIdStringOptionalIf the content is a TV episode that is part of a series, pass the following:  

*   contentId: Pass the **seriesId** in this field. This should be the same as the seriesId in the app's Roku Search feed.
*   episodeId: Pass the **episodeId** in this field. This should be the same as the "playId" in the app's Roku Search Feed, or "contentId" in externalID implementations

  
This enables Roku to enhance the UX presentation of the series resume point.  
  
If the **waitForNextEpisodeAvailability** field is set to true, the series will only be shown in the Continue Watching row after the subsequent episode is available in the app's search feed.waitForNextEpisodeAvailabilityBooleanOptionalThis field is used for episodic content. Set it to **true** when an episode has been completed and the next episode has not been released yet; otherwise, set it to **false**.  
  
Roku can use this information to show the content in the Continue Watching row whenever the next episode becomes available.  
  
This feature requires a [search feed](https://developer.roku.com/docs/specs/search/search-feed.md) that lists "serial" and "episode" assets.  
  
The following matrix demonstrates how to use this field. In this example, E1 and E2 are available, but E3 has not been released yet.  

| Event | episodeId value | waitForNextEpisodeAvailability flag | Episode shown in Continue Watching row |
| --- | --- | --- | --- |
| Start E1 | E1  | false | E1  |
| Complete E1 | E1  | true | E2  |
| Start E2 | E2  | false | E2  |
| Complete E2 | E2  | true | none |
| E3 becomes available later | \-  | \-  | E3  |
| Start E3 | E3  | false | E3  |
| Complete E3 | E3  | true | none |

profileLabelStringOptionalEnables Roku to label content in the Continue Watching row to identify which profile was watching the content item (this is different than the profileId in the endpoint URL, which is the UUID of the profile used to watch the content).lastInteractionTimeIntegerRequiredThe unix timestamp of the playback event. This is used to help determine the ordering of items in the Continue Watching row. If data is unavailable, use the current epoch time.positionIntegerOptionalThe timestamp of the content item (in seconds) when the playback event occurred.  
  
Providing the **position** and **duration** enables a progress bar that approximates the playback position to be displayed on the content thumbnail in the Continue Watching row (as long as playback has started, but not completed).durationIntegerOptionalTotal running time of the content (in seconds).

#### Example

**URL**:

*   POST [https://userdata.sr.roku.com/user-data/v1/content/continueWatching](https://userdata.sr.roku.com/user-data/v1/content/continueWatching)
*   POST [https://userdata.sr.roku.com/user-data/v1/profile/{profileId}/content/continueWatching](https://userdata.sr.roku.com/user-data/v1/profile/%7BprofileId%7D/content/continueWatching) (app has a profile selection screen)

**JSON body**:

    {
      "items": [
        {
          "contentId": "abc123",
          "episodeId": "def123",
          "lastInteractionTime": 123456,
          "position": 854,
          "duration": 1678,
          "waitForNextEpisodeAvailability": true
        }
      ]
    }
    

### Retrieve API

To retrieve the list of content items in the Continue Watching row, send a **GET** request to the Continue Watching API:

**URL**:

*   GET [https://userdata.sr.roku.com](https://userdata.sr.roku.com/)/user-data/v1/content/continueWatching
*   GET [https://userdata.sr.roku.com](https://userdata.sr.roku.com/)/user-data/v1/profile/{profileId}/content/continueWatching (app has a profile selection screen)

### Update API

To replace the list of content items in the Continue Watching row with a new list, send a **PUT** request to the Continue Watching API with a JSON body containing the same parameters listed in the [Add API section](#add-api):

### Delete API

To remove content items from the Continue Watching row, send a **DELETE** request to the Continue Watching API with a JSON body containing the **contentId** of the item to be removed.

#### Example

**URL**:

*   DELETE [https://userdata.sr.roku.com/user-data/v1/content/continueWatching](https://userdata.sr.roku.com/user-data/v1/content/continueWatching)
*   DELETE [https://userdata.sr.roku.com/user-data/v1/profile/{profileId}/content/continueWatching](https://userdata.sr.roku.com/user-data/v1/profile/%7BprofileId%7D/content/continueWatching) (app has a profile selection screen)

**JSON body**:

    {
      "items": [
        {
          "contentId": "abc123"
        }
      ]
    }
    

Managing user consent
---------------------

If publishers require explicit consent from customers before adding their watched content to the Continue Watching row, it is the publisher's responsibility to implement this. Roku does not provide any mechanisms or APIs for handling user consent for the Continue Watching integration.

Handling deep links from Continue Watching
------------------------------------------

Handling deep links sent to your app from the Continue Watching row is essentially the same as documented [here](/docs/developer-program/discovery/implementing-deep-linking.md). The deep links sent to your app include a **contentId** field with your unique ID for the content to be played, the **mediaType**, which dictates the playback experience, and the user's **profileId** (if your app has a profile selection screen).

**Syntax:**

    http://<roku-device-ip-address>:8060/launch|input/<channelId>?contentId=<contentIdValue>&mediaType=<mediaTypeValue>&profileId=<profileIdValue>
    

**Example:**

    http://192.168.1.4:8060/input/581251?contentId=dev-summit-21-keynote&mediaType=movie&profileId=12345
    

Appendix A: Sample BrightScript code for adding HTTP headers
------------------------------------------------------------

To call the Continue Watching APIs, the app must include BrightScript code that adds the following HTTP headers (see the [**ifHttpAgent.addHeader()** function](/docs/references/brightscript/interfaces/ifhttpagent.md#addheadername-as-string-value-as-string-as-boolean) for more information). You must set the endpoint first before providing the headers.

    'SetUrl needs to be called first
    request.SetUrl("https://userdata.sr.roku.com/user-data/v1/content/continueWatching");
    request.AddHeader("Content-Type","application/json")
    request.AddHeader("x-roku-reserved-jwt", "")
    request.AddHeader("x-roku-reserved-channel-id", "<production app ID>") 'pass the production app ID
    request.AddHeader("x-roku-reserved-channel-store-code", "")
    request.AddHeader("x-roku-reserved-virtual-user-id", "")
    request.AddHeader("x-roku-reserved-device-id", "")
    request.AddHeader("x-roku-reserved-serial-number", "")
    

Appendix B: Integration testing notes
-------------------------------------

Developers can test the Continue Watching integration in both sideloaded and beta environments. Testing should verify that each step in the [integration workflow](#integrating-into-continue-watching) is completed successfully.

#### End-to-end testing

When doing end-to-end testing, launching content from the Continue Watching row will always launch the production app instead of the sideloaded or beta version—even if the production version is not currently integrated with Continue Watching. This is because the app's search feed, which is used by Roku to load content into the Continue Watching row, is always associated with the production app. Testing therefore should focus on adding content to the Continue Watching row when playback starts, updating bookmarks as users stop and resume watching, and removing content when it has been completed.

#### Activating the Continue Watching row on new test devices

When using a Roku device that has not previously been used for testing the Continue Watching integration, the Continue Watching row is not displayed on the **What to Watch** screen until content on the app has been watched. The required watch time for adding the first content item to the Continue Watching row and therefore activating the feature on a new test device is as follows:

*   If the app has a pixel-sharing agreement with Roku, you can watch a few minutes of content. The Continue Watching row should be active after 24 hours.

*   If the app does not have a pixel-sharing agreement with Roku, you should watch two hours of content. The Continue Watching row should be active after 24 hours.

Once the Continue Watching row has displayed the first content item on that device, it will subsequently be updated as different content is watched.