# Back-end Architecture Overview

The **server-side** of the Kelowna Islamic Center (KIC) ecosystem is powered by **Firebase Cloud Functions**, **Firestore**, and **Cloud Tasks**.  

These backend services handle the **automation, scheduling, and delivery** of notifications and content to the mobile app, web kiosk, and other client platforms.

This section of the documentation provides a complete reference for how these server-side processes work, how they are structured, and how to extend or troubleshoot them.

---

## Purpose of Server-Side Functions

The KIC mobile and web platforms rely on accurate, timely, and consistent information.  
Server-side functions ensure that:

- **Prayer reminders** are fetched, scheduled, and delivered without manual intervention.
- **Announcements** are instantly broadcast to the community when new updates are posted.
- Notifications are localized to user preffered languages and properly routed to users who have opted in.
- System complexity (timezones, retries, scheduling, and message formatting) is all handled automatically in the backend.

---

## Key Components

**1. Get Prayer Times**

   - Fetches latest Athan and Iqamah times accurately for each prayer.
   - Resolve prayer times based on selected calculation method. 
   - See: [Prayer Times API](./prayer-times-api.md)

**2. Announcements**

   - Firestore-backed system for storing announcements.  
   - Automatically triggers notifications when new announcements are created.  
   - See: [Announcements](./announcements.md)

**3. Prayer Reminders**

   - Daily scheduler (`prayerTimesAlertScheduler`) fetches and queues reminders.  
   - Task queue processor (`sendPrayerAlert`) dispatches localized push notifications for Athan and Iqamah.  
   - See: [Prayer Reminders](./prayer-reminders.md)

**4. Supporting Infrastructure**

   - **Firestore** – Persistent storage of announcements.  
   - **Cloud Tasks** – Time-precise delivery of prayer notifications.  
   - **Cloud Messaging (FCM)** – Notification delivery to mobile apps.

---

## Developer Notes

- All notifications are **topic-based** (`announcements`, `athanAlert`, `iqamah*MinuteAlert`, etc.), giving clients granular control over what to subscribe to.
- Timezones are managed differently on client where it is local masjid time (`America/Vancouver`) and the server where it is server region (`America/Chicago`).
- Functions are written to **fail gracefully** (e.g., no notification if config is missing, retries for transient errors).
- This system is **event-driven and fully automated** — once deployed and configured, minimal admin intervention is needed.

---

## Next Steps

Explore each feature in detail:

- [Cloud Functions Overview](./cloud-functions.md)
- [Prayer Times API](./prayer-times-api.md)
- [Announcements](./announcements.md)  
- [Prayer Reminders](./prayer-reminders.md)
