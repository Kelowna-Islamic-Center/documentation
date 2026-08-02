# Background Services

Background services keep app data up to date when the app is minimized, closed, or reopened after extended periods. The mobile app uses background work for prayer time refreshes and announcement synchronization.

!!! note
    Prayer alert scheduling and reconciliation are also part of the app's background services. However, they are documented separately in [Athan & Iqamah Alerts](./alert-system-overview.md).

## API Fetch Service

The API fetch service periodically refreshes prayer times from the [Prayer Times API](../server-side/prayer-times-api.md) and stores the prayer times for today and tomorrow in SharedPreferences. It runs as a periodic background task with a network-required constraint and uses a long execution interval to minimize unnecessary battery and network usage.

This background refresh keeps prayer time data current even if the user does not open the app for an extended period.

## Cloud Messaging Background Handling

The app registers both foreground and background Firebase Cloud Messaging handlers. When announcement-related messages are received through Firebase Messaging, it refreshes the announcement cache from Firestore so the next time the announcements view is opened, it displays the latest content.

## Cached Data Behavior

SharedPreferences serves as a lightweight cache for prayer times and user settings. Background tasks keep this cache up to date, while foreground screens read from it during startup and throughout normal app usage.

This improves application responsiveness and reduces dependence on immediate network availability.

## Platform Notes

Background execution is best-effort on both Android and iOS, and its behavior depends on the operating system and device-specific power management policies. This is why the app combines periodic background work with normal foreground refreshes, allowing cached data to self-correct whenever the user returns to the app.
