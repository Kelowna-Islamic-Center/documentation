# announcementAlert

**Type:** Firestore Trigger (onDocumentCreated)

**Path:** `/announcements/{docId}`

## Description

Triggered whenever a new document is created in the `announcements` collection.  

Once a document is created, this function:

1. Adds a `timeStamp` field to the new document with the current server time.
2. If the announcement is intended for the `mobile` platform, it sends multiple push notifications, one per locale, to the `announcements` topic via Firebase Cloud Messaging.

> Currently supported locales are English (en) and Arabic (ar), however this function can scale with an unlimited number of locales.

## Example Notification Payload

This notification payload is sent to Firebase Messaging which delivers it to the mobile apps and the web kiosk app.

```json
// English Payload (Sent as a seperate announcement)
{
  "condition": "'announcements' in topics && 'lang-en' in topics",
  "notification": {
    "title": "This is a sample announcement - New Announcement",
    "body": "This is the detailed announcement description.",
    "android_channel_id": "ANDROID_CHANNEL_ID_ANNOUNCEMENTS"
  },
  "android": {
    "notification": {
     "channelId": "ANDROID_CHANNEL_ID_ANNOUNCEMENTS",
    }
   },
  "data": {
    "notificationType": "announcements",
    "topic": "announcements",
    "locale": "en",
    "announcementId": "243"
  }
}

// Arabic Payload (Sent as a seperate announcement)
{
  "condition": "'announcements' in topics && 'lang-ar' in topics",
  "notification": {
    "title": "هذا إعلان تجريبي - إعلان جديد",
    "body": "هذا هو الوصف التفصيلي للإعلان.",
    "android_channel_id": "ANDROID_CHANNEL_ID_ANNOUNCEMENTS"
  },
  "android": {
    "notification": {
     "channelId": "ANDROID_CHANNEL_ID_ANNOUNCEMENTS",
    }
   },
  "data": {
    "notificationType": "announcements",
    "topic": "announcements",
    "locale": "ar",
    "announcementId": "243"
  }
}
```
