Setting up Roku Pay web services
================================

Roku Pay includes [web services](/docs/developer-program/roku-pay/implementation/roku-web-service.md#apis) that developers integrate into their backend system for validating, refunding, and canceling transactions related to subscriptions and one-time purchases. In addition to pulling transactions via the [Roku Pay web services](/docs/developer-program/roku-pay/implementation/roku-web-service.md#apis), publishers can receive the transactions in real-time via [push notifications](/docs/developer-program/roku-pay/implementation/push-notifications.md). These features enable publishers to implement an entitlement service for checking whether to grant users access, issue refunds or service credits, and cancel subscriptions.

The **Roku Pay Web Services** page lets developers manage their Roku Pay API key, whitelist IP addresses that may send requests using the key, and enter the URL for receiving push notifications. You can access this page from the Developer Dashboard, by selecting **Roku Pay Web Services**. You can also select **Roku Pay Web Services** from the drop-down list on the left side of the pages within the Developer Dashboard.

![roku815px - web-api-settings-dashboard-v2](https://image.roku.com/ZHZscHItMTc2/web-api-settings-dashboard-v2.jpg)

Roku Pay API Key
----------------

Your Roku Pay API key enables you to send Roku Pay web service API calls and receive push notifications. Record and secure your key.

![roku815px - roku-pay-api-key](https://image.roku.com/ZHZscHItMTc2/roku-pay-api-key-v4.png)

### Rotating API keys

Developers can periodically rotate their Roku Pay API Key as a security best practice or reset it immediately if it becomes compromised. The **Roku Pay API Key** settings include an **Invalidate** button that generates a new Roku Pay API Key and lets developers either immediately invalidate their old one (in case it has been compromised) or schedule its invalidation for a later date (for regular security maintenance). Scheduling the invalidation of the old API key provides developers with a grace period to implement the new one and thus avoid any downtime.

> Developers may only have a maximum of two Roku Pay API Keys at a time: one active and one expired or expiring.

To generate a new Roku Pay API Key, follow these steps:

1.  On the current active API key, click **Invalidate**. An API key cannot be invalidated if there is already another one in the expired/expiring state.

2.  Select whether to invalidate the API key immediately or at a later date.
    
    *   Click **Invalidate Immediately** to generate a new API key and expire the current one. An expired key cannot be used for making Roku Pay Web Service API calls.
        
    *   Click **Invalidate in Days** to generate a new API key and schedule the expiration of the current one in 1–30 days. The default is **1** day.
        
        ![roku400px - invalidate-key](https://image.roku.com/ZHZscHItMTc2/invalidate-key-v2.png)
        

3.  Click **Submit**.

4.  The new API key is generated and has a status of **Active**. The status of the current API key changes to **Expired** (if invalidated immediately) or **Expiring on \*timestamp\*** (if invalidation is scheduled for a later date).
    
    ![roku600px - rotate-roku-pay-api-key](https://image.roku.com/ZHZscHItMTc2/rotate-roku-pay-api-key-v5.png)
    

#### Changing the grace period for an expiring API key

To change the grace period for invalidating an expiring API key, click **Edit** on the Expiring API key, change the number of days in which it will be invalidated, and then click **Submit**.

#### Reactivating an expired API key

To reactivate an expired API key, click **Edit** on the Expired API key, schedule its expiration in 1–365 days, and then click **Submit**.

#### Deleting an expired or expiring API key

To permanently delete an expired or expiring API key, click **Delete** on the Expired/Expiring API key and then click **Delete** in the confirmation dialog. Once an API key has been deleted, it can no longer be retrieved or used again.

Allowed IP address range
------------------------

Developers can whitelist one or more ranges of IPv4 addresses from where Roku Pay API calls using their API key may originate. This prevents the Roku Pay web services from accepting requests with the developer's API key outside the specified range. If an IP address range is set, a request is only valid if it comes from the specified range. If no range is specified, any request with the developer's API key from any IP address is accepted.

![roku600px - allowed-ip-address-range-blank](https://image.roku.com/ZHZscHItMTc2/allowed-ip-address-range-blank-v3a.png)

To set a range of allowed IP address, follow these steps:

1.  Click **Add Allowed IP Address Range**.

2.  In the **Starting IP** field, enter the starting IPv4 address of the range.

3.  In the **Ending IP**, enter the ending IP address of your IP address range. To only allow requests to be sent from the starting IP address, leave this field empty.

4.  Click **Add**. The starting IP address, ending IP address (if any), and number of IP addresses in the specified range are listed. Repeat steps 1–4 to add another range of IP addresses.
    
    ![roku815px - allowed-ip-address-range](https://image.roku.com/ZHZscHItMTc2/allowed-ip-address-range-v2.jpg)
    

5.  To edit an IP address range, click **Edit**, update the starting and/or ending IP addresses, and then click **Update**. To delete an IP address range, click **Delete** and then click **Delete** in the confirmation dialog.

Push notifications
------------------

Publishers can subscribe to [transaction notification messages from Roku Pay](https://developer.roku.com/docs/developer-program/roku-pay/implementation/push-notifications.md). This enables the publisher to receive purchases, cancellations, and refund/service credit requests in real-time and update their backend system accordingly.

Roku sends push notification messages using [JWT signature authentication](https://datatracker.ietf.org/doc/html/rfc7515#section-3). This enables publishers to verify that the push notification messages received by their endpoint originated from Roku.

> As of February 1, 2024, all developer accounts must use [JWT signature authentication](https://datatracker.ietf.org/doc/html/rfc7515#section-3) to receive Roku Pay push notifications. Apps cannot revert to receiving unauthenticated messages.

To receive JWT/JWS-secured push notifications, follow these steps:

1.  Read the [Roku Pay Push Notification JWT authentication guide](/docs/developer-program/roku-pay/implementation/push-notifications-jwt.md). This document explains how to configure and test your endpoint for receiving JWT/JWS-secured messages.

> The payload for JWT/JWS-secured messages is significantly different than the one used for unauthenticated messages; therefore, you must configure your push notification endpoint properly to avoid disrupting your system

2.  Configure a test endpoint and verify whether it can receive and process the JWT/JWS-secured messages. To do this, go to the [**Test Push Notification URL** settings](#test-push-notification-url) and then provide an HTTPS test notification URL and test end date. JWT/JWS-secured messages will automatically start being sent to the specified test endpoint.
    
3.  Optionally, you can manually send test payloads to your test endpoint in the [**On-demand Test Message** settings](#on-demand-test-message).
    
4.  Enter the URL for your production push notification endpoint.
    

### Test push notification URL

Publishers can automatically send JWT/JWS-secured Roku Pay push notification messages to a test endpoint until a specific end date. This enables publishers to verify that they can receive and process the JWT/JWS-secured Roku Pay push notification messages before sending them to their production endpoint.

![roku815px - img](https://image.roku.com/ZHZscHItMTc2/push-notification-test-url.png)

To automatically send JWT/JWS-secured Roku Pay push notification messages to a test endpoint, follow these steps:

1.  In the **Notification URL** field, enter your test push notification endpoint. The endpoint must use HTTPS.

2.  In the **End Date** field, enter when test notifications are stopped being sent to the test push notification URL.

3.  Click **Save Changes**.

### On-demand test message

Publishers can manually send a test JWT/JWS-secured message with a generic payload to the test push notification endpoint configured in the [**Test Configuration For Push Notification** settings](#test-configuration-for-push-notification). This enables publishers to verify that their test endpoint can receive a JWT/JWS-secured message without generating Roku Pay transactions.

![roku815px - img](https://image.roku.com/ZHZscHItMTc2/push-notification-on-demand-test-message.png)

To manually send a test JWT/JWS-secured notification message to a test endpoint, follow these steps:

1.  In the **Test Message Type** field, enter any non-empty text.

2.  In the **Test Message Payload** field, enter any non-empty text.

3.  Click **Send Test Message**.

### Push notification URL

Once you have configured and tested your push notification integration, you can provide the URL of your push notification production endpoint.

![roku600px - push-notification-endpoint](https://image.roku.com/ZHZscHItMTc2/push-notification-prod-url.png)

To provide the endpoint to receive Roku Pay push notifications, follow these steps:

1.  Enter the secure URL for your production push notification endpoint.
2.  Click **Save Changes**.

> See the [Roku Pay push notifications reference](https://developer.roku.com/docs/developer-program/roku-pay/implementation/push-notifications.md) for more information on the contents of the Roku Pay push notification messages.
> 
> If the endpoint fails for a specific message for three consecutive days (72 hours), Roku stops sending that notification. If the endpoint fails to acknowledge 100 notifications within 10 days, the endpoint is considered invalid and placed on a deny list.

### Replay notifications

Publishers can resend Roku Pay push notification messages for a specific 14-day timeframe within the past 90 days. This enables publishers to receive messages that may have been missed because their endpoint had a misconfiguration, service outage, or other error.

![roku600px - push-notification-endpoint](https://image.roku.com/ZHZscHItMTc2/push-notification-replay-messages.png)

To resend Roku Pay push notifications, follow the steps:

1.  In the **Start Date** and **End Date** boxes select the timeframe for which you want push notification replays to be send to your endpoint.
2.  Click **Replay Notifications**.

> Check the timestamp of replayed messages to ensure they are processed in the correct order. Processing replayed messages out of order can result in entitlement errors (for example, the replayed messages for a subscription placed on hold and then subsequently recovered must be processed in that order or the customer may be denied access to content).