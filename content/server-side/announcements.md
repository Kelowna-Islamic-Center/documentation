# Announcements

The announcements system is used by KIC to keep the community informed about events, updates, or important notices. Announcements can be found in the announcements tab on the mobile app or on the sliding carousel on the kiosk display.

They are stored in a Firestore database and automatically trigger notifications to client applications whenever a new announcement is added.

Admins can create announcements within the Mobile App's admin tools and specify which platforms (mobile or web) will receive the announcement.

This guide explains how announcements are structured, stored, and delivered through Cloud Functions.

## Firestore Storage

All created announcements are stored in a Firestore collection under:

```text
/announcements/{docId}
```

Each announcement document contains the following fields:

| Field         | Type      | Description                                                                                                                           |
| ------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `title`       | string    | The fallback title of the announcement. Used when a localized title is unavailable.                                                   |
| `description` | string    | The fallback full content of the announcement. Used when a localized description is unavailable.                                      |
| `l10n`        | map       | Localized announcement content. Contains language-specific fields (e.g., `en`, `ar`) with localized `title` and `description` values. |
| `platforms`   | array     | Platforms to notify (e.g., `["mobile"]`, `["web"]`, or `["web", "mobile"]`). If missing or invalid, the announcement is ignored.      |
| `timeStamp`   | timestamp | Server-generated timestamp automatically set when the announcement is added. Do not set this manually.                                |

Example announcement document structure:

```json
{
  "title": "Masjid Whatsapp Group",
  "description": "Kelowna Masjid has created their own whatsapp group for information sharing. Please click on this link to join the group.",
  "l10n": {
    "en": {
      "title": "Masjid Whatsapp Group",
      "description": "Kelowna Masjid has created their own whatsapp group for information sharing. Please click on this link to join the group."
    },
    "ar": {
      "title": "مجموعة واتساب المسجد",
      "description": "أنشأ مسجد كيلونا مجموعةً على واتساب لمشاركة الإعلانات والمعلومات. يُرجى الضغط على الرابط التالي للانضمام إلى المجموعة:"
    }
  },
  "platforms": [
    "mobile",
    "web"
  ],
  "timeStamp": "2024-03-29T07:19:52Z"
}
```

## How Announcements are Sent

The `announcementAlert` function manages the full lifecycle of announcements. This process starts when a new document is added to the `/announcements` collection. The newly created document triggers the `announcementAlert` Cloud Function, which then sets the `timeStamp` field based on the server time.

This results in an announcement Firestore document similar to the following example:

```json
{
  "title": "This is a sample announcement",
  "description": "This is the detailed announcement description.",
  "l10n": {
    "en": {
      "title": "This is a sample announcement",
      "description": "This is the detailed announcement description."
    },
    "ar": {
      "title": "هذا إعلان تجريبي",
      "description": "هذا هو الوصف التفصيلي للإعلان."
    }
  },
  "platforms": [
    "mobile",
    "web"
  ],
  "timeStamp": "2024-03-29T07:19:52Z"
}
```

The function then creates multiple Cloud Messaging payloads, one for each locale of the announcement. Each payload is sent to clients subscribed to the `announcements` topic (devices with announcement alerts enabled in the app settings) and the respective locale topic.

These notifications are only sent if the `platforms` array includes `"mobile"`. If the `platforms` field is missing or is not an array, no notification is sent.

The document above would send the following payloads:

```json
// English Payload (Sent as a separate announcement)
{
  "condition": "'announcements' in topics && 'lang-en' in topics",
  "notification": {
    "title": "This is a sample announcement - New Announcement",
    "body": "This is the detailed announcement description.",
    "android_channel_id": "ANDROID_CHANNEL_ID_ANNOUNCEMENTS"
  },
  "android": {
    "notification": {
      "channelId": "ANDROID_CHANNEL_ID_ANNOUNCEMENTS"
    }
  },
  "data": {
    "notificationType": "announcements",
    "topic": "announcements",
    "locale": "en",
    "announcementId": "243"
  }
}

// Arabic Payload (Sent as a separate announcement)
{
  "condition": "'announcements' in topics && 'lang-ar' in topics",
  "notification": {
    "title": "هذا إعلان تجريبي - إعلان جديد",
    "body": "هذا هو الوصف التفصيلي للإعلان.",
    "android_channel_id": "ANDROID_CHANNEL_ID_ANNOUNCEMENTS"
  },
  "android": {
    "notification": {
      "channelId": "ANDROID_CHANNEL_ID_ANNOUNCEMENTS"
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

That's basically it for the backend side of announcements.

[Full documentation of the `announcementAlert` Cloud Function is available](./cloud-functions/announcement-alert.md), with details on the function parameters and the generated output payloads.
