# Cloud Functions Overview

This application makes use of Firebase Cloud Functions which is a complete serverless solution that runs on Google Cloud, requiring no server maintenance.

The functions are simple JavaScript (JS) functions that are run on the server based on triggers.

You can find the full source code for these functions along with all the server-side logic that powers the **mobile app**, **kiosk app**, and the backend services for **prayer times**, **announcements**, and **notifications** here:

[:simple-github: Kelowna-Islamic-Center/cloud-functions](https://github.com/Kelowna-Islamic-Center/cloud-functions){ .md-button }

!!! info
    To learn more about Firebase Cloud Functions [read the official Firebase guide](https://firebase.google.com/docs/functions/).

---

## Purpose of Cloud Functions

The current cloud functions are responsible for handling all server-side logic such as:

* Fetching daily prayer times from The BCMA and Islamic Finder APIs.
* Scheduling and sending notifications for prayer times (athan and iqamah) and announcements.
* Serving prayer times via HTTP endpoints to clients.
* Handling any other server-side logic.

The cloud functions do not handle authentication as Firebase provides the managed Firebase Authentication service which already handles all authentication requests, tokens, etc.

## Cloud Messaging Notifications

Cloud functions can also send cloud notifications that are delivered over the internet rather than being locally triggered. These "cloud notifications" are handled through Firebase Cloud Messaging (FCM).

Clients receive notifications by subscribing to **topics**. These topics are agreed beforehand and determine which notifications the client will receive. For example, if a mobile app client is subscribed to the `announcements` topic, it will receive a notification if a cloud function passes a notification payload to FCM that contains the `announcements` topic.

!!! info
    To learn more about Firebase Cloud Messaging [read the official Firebase guide](https://firebase.google.com/docs/cloud-messaging/).

## Active Functions Summary

The following functions are currently active.

Detailed documentation for each function can be found under the "Cloud Functions Reference" category or by clicking the links below.

| Function                                                                         | Type       | Trigger                 | Purpose                              |
| -------------------------------------------------------------------------------- | ---------- | ----------------------- | ------------------------------------ |
| [`announcementAlert`](./cloud-functions/announcement-alert.md)                   | Firestore  | Document created        | Sends announcement notifications     |
| [`prayerTimesAlertScheduler`](./cloud-functions/prayer-times-alert-scheduler.md) | Scheduled  | Daily at 07:00 UTC (00:00 PST)     | Queues prayer notifications          |
| [`sendPrayerAlert`](./cloud-functions/send-prayer-alert.md)                      | Task Queue | Dispatched by scheduler (when prayer reminder notification should be sent) | Sends localized prayer notifications |
| [`prayerTimesFetch`](./cloud-functions/prayer-times-fetch.md)                    | HTTP       | GET request             | Returns structured prayer times      |
