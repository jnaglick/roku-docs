App localization
================

Publishers can distribute their content across the world through the [Streaming Store](https://channelstore.roku.com/). The Streaming Store includes all of North America, many countries in Latin America, and several countries in Europe. The Roku development platform makes it easy to broadly disseminate content; however, publishers should consider localization in the development, distribution, engagement, and monetization of their apps.

Roku has Streaming Stores in the following countries:

| North America | Europe | Latin America | Asia Pacific |
| --- | --- | --- | --- |
| *   United States<br>*   Canada | *   United Kingdom<br>*   Ireland<br>*   Germany | *   Argentina<br>*   Brazil<br>*   Chile<br>*   Colombia<br>*   Costa Rica<br>*   El Salvador<br>*   Guatemala<br>*   Honduras<br>*   Mexico<br>*   Nicaragua<br>*   Panama<br>*   Peru | Australia |

Development
-----------

The Roku platform supports the distribution of a single package file across multiple streaming stores. This means that developers only need to build and maintain one app and then handle localization in the application's code. For example, the app can be programmed to have a localized user experience, including multi-language support for the UI (labels, menus, and dialogs) and content metadata (titles and descriptions), and it can control the availability of content for different regions. In addition, the app UI can link to a privacy policy URL corresponding to the country associated with the user's Roku customer account.

To do this, developers can [get the external IP address of a Roku device](/docs/references/brightscript/interfaces/ifdeviceinfo.md#getexternalip-as-string) client-side, and return it back to the app's backend server. In the backend, the app can get the country ranges mapped to the IP address in order to present a localized experience and determine which content to show and allow access to customers.

Distribution
------------

The [Roku Developer Dashboard](https://developer.roku.com/developer) is the central hub for creating, configuring, certification testing, and distributing apps. The dashboard enables publishers to select in which countries to distribute an app. In addition, the dashboard presents a number of options that enable the app to be localized in the Streaming Store. Aspects of a Streaming Store listing that may be localized include metadata such as the app name and description, the app poster, and screenshots. Providing localized version of this application metadata will offer users of all language preferences the opportunity to learn more about your app before installing it.

Engagement
----------

### Roku Search multi-language and multi-regional support

[Roku Search](/docs/developer-program/discovery/search/implementing-search.md) supports English, Spanish, Portuguese, and German. Apps participating in Roku Search can provide [localized search feeds](/docs/specs/search/search-feed.md#multiregion-and-multilanguage-support) to further increase the discoverability of their content. For example, providing a search feed in Spanish makes the app's content available in queries made on devices with their language set to Spanish, as well as devices located in Streaming Stores where Spanish is the primary language (for example, most countries in Latin America).

### Multi-regional display and video ad targeting

Apps can be further promoted globally with Roku home screen banner ads, Roku screensaver ads, and video ads. Roku's [self-serve promotion tool](/docs/features/engagement/self-serve-promotions.md) lets apps programmatically purchase ads in order to drive installs, increase viewership, and reach more users. Apps may run home screen banner ads and Roku screensaver ads in a gowing number of countries (see the [documentation](/docs/features/engagement/self-serve-promotions.md) for the list of eligible countries).

Monetization
------------

### Regional availability of in-app products

Apps offering subscriptions and one-time purchases (movie rentals, pay-per-views, special events, and so on) can program the app to control the availability of in-app products in different regions. For example, an app may only be able to legally distribute content in a specific set of countries. To do this, apps can use the ChannelStore [**getUserRegionData**](/docs/references/scenegraph/control-nodes/channelstore.md#getuserregiondata) command to determine the country associated with the user's Roku account, and then implement business logic to filter the results of the ChannelStore [**getCatalog** command](/docs/references/scenegraph/control-nodes/channelstore.md#getcatalog) to only display products that should be available for that country.

### Localized in-app product names

Apps can also localize the names of their [in-app products](/docs/developer-program/roku-pay/quickstart/in-channel-products.md) (the name that is displayed to customers in the app's on-device purchasing workflow and in subscription emails sent by Roku). This helps customers identify the subscriptions and content they have purchased and reduces potential refund requests from unrecognizable charges.

### Currency conversions

While localizing product names is a manual process, Roku Pay automatically handles currency conversions and displays prices in the currency associated with the Streaming Store in which the device is located. For example, if a device is located in Brazil, the price of an in-app product that is $9.99 USD is displayed as R$24.9 BRL (Brazilan Real). Apps may also elect to independently handle currency conversion. To do this, apps can create in-app products for each country and filter out products based on the country in which the device is located (using the [ifDeviceInfo.GetCountryCode()](/docs/references/brightscript/interfaces/ifdeviceinfo.md#getcountrycode-as-string) method).