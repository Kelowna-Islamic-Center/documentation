# Background Services

As explained in the [Application Architecture Section](./application-architecture.md), background processes in the mobile app are referred to as services.

They share similarities with controllers but are distinct in that they run independently of the UI and handle tasks that must continue in the background. This separation ensures the app remains responsive while offloading recurring or asynchronous work to background layers.

---

## Active Services

Currently, the app uses the following two core services:

### 1. `cloud_messaging_service.dart`

- Manages Firebase Cloud Messaging (FCM) integration.  
- Handles topic subscriptions, such as `athanAlert` or `announcements`.  
- Parses notification payloads received from the server and prepares them for display.  
- Ensures users receive alerts even when the app is terminated. 

---

### 2. `api_fetch_service.dart`

- Periodically performs HTTP requests to the [Prayer Times API](../../server-side/prayer-times-api.md) and caches them.
- Updates local prayer times so the app functions correctly even on devices that do not have consistent internet access or mobile data.  
- Provides redundancy in case cloud messaging fails, since prayer times are always available locally.

---

## Experimental Background Work

In addition to the two stable services, there is ongoing experimentation with device-native athan and iqamah scheduling using the [`workmanager`](https://pub.dev/packages/workmanager) plugin.

This was actually the previous implimentation before it was replaced with cloud messaging method usign the `prayerSchedulingAlert` and `sendPrayerAlert` Cloud functions. The reason for attempting to go back to workmanager is to gain more control over how notifications are delivered and to get more control over Athan Audio.

The goal of this experimentation is to:  

- Move athan and iqamah reminders to on-device background tasks.  
- Reduce reliance on server-side scheduling and notifications.  
- Allow reminders to trigger even when there is no active internet connection.  

At this stage, this work is considered **experimental and not production-ready at all due to how aggressive iOS and Android are to background services**. This area requires significant work to find the best way to get notifications delivered reliably especially when it's closed.
