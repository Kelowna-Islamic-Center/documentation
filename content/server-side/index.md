# Back-end Architecture Overview

The server-side of the Kelowna Islamic Center (KIC) ecosystem is powered by Firebase Cloud Functions, Firebase Cloud Messaging, and Firestore.

These backend services handle the distribution of announcement notifications to all active mobile devices, as well as the base API for prayer information data used by the mobile app and web kiosk.

This section of the documentation provides a reference for how these server-side processes work, how they are structured, and how to extend or troubleshoot them.

## Key Components

### Firestore

Firestore is used exclusively for storing announcements. Prayer data is not stored in the cloud, as it is fetched from external services. Announcement data is fully contained within the `announcements` collection in Firestore.

### Cloud Functions

!!! info
    If you are unfamiliar with Firebase Cloud Functions, you can learn more about them by [reading the official Firebase guide](https://firebase.google.com/docs/functions/).

The current role of Cloud Functions is to handle backend events. The two supported events are:

   1. Fetching Prayer Times

      This is handled by an `onRequest` HTTP endpoint function that, when requested, consolidates prayer time data.

      **Full Documentation:** [Prayer Times API](./prayer-times-api.md)

   2. Announcement Notifications

      Once a new announcement is created by a Masjid Admin, a function adds the required metadata to the announcement and then schedules a notification using Firebase Cloud Messaging.

      **Full Documentation:** [Announcements](./announcements.md)

The Firebase Cloud Functions implementation source code is maintained within the following repository:

[:simple-github: Kelowna-Islamic-Center/cloud-functions](https://github.com/Kelowna-Islamic-Center/cloud-functions){ .md-button }

### Cloud Messaging Notifications

!!! info
      If you are unfamiliar with Firebase Cloud Messaging, you can learn more about them by [reading the official Firebase guide](https://firebase.google.com/docs/cloud-messaging/).

   The cloud function for announcements sends cloud notifications that are delivered to mobile app users over the internet rather than being locally triggered through Firebase Cloud Messaging (FCM).

   Clients receive notifications by subscribing to topics. These topics are agreed beforehand and determine which notifications the client will receive. For example, if a mobile app client is subscribed to the announcements topic, it will receive a notification if a cloud function passes a notification payload to FCM that contains the announcements topic.

## Next Steps

Explore each feature in detail:

* [Prayer Times API](./prayer-times-api.md)
* [Announcements](./announcements.md)

## Developer Notes

* The `announcement` topic is only listened to if a locale identifier topic (eg. `lang-en`) is also provided, giving clients localized notifications within a given topic.
* Timezones are managed differently on the client and server. The client uses the local masjid timezone (`America/Vancouver`), while the server runs in the configured region timezone (`America/Chicago`). All current functions handle this correctly; however, this should be considered when developing new functions.
* Functions are designed to fail gracefully (e.g., no notification if configuration is missing, retries for transient errors).
