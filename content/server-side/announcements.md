# Announcements

The announcements system is used by KIC to keep the community informed about events, updates, or important notices. Announcements can be found in the announcments tab on the mobile app or on the sliding carousel on the kiosk display.

They are stored in a Firestore database and automatically trigger notifications to client applications whenever a new announcement is added.

Admins can create announcements within the Mobile App's admin tools and specify which platforms (mobile or web) will receive the announcmenet. 

This guide explains how announcements are structured, stored, and delivered through Cloud Functions.

---

## Firestore Storage

All created announcements are stored in a Firestore collection under:

```text
/announcements/{docId}
```

Each announcement document contains the following fields:

| Field         | Type      | Description                                                                 |
| ------------- | --------- | --------------------------------------------------------------------------- |
| `title`       | string    | The title of the announcement                                               |
| `description` | string    | The full content of the announcement                                        |
| `platforms`   | array     | Platforms to notify (e.g., `["mobile"]`, `["web"]`, or `["web", "mobile"]`). If missing or invalid, ignored.   |
| `timeStamp`   | timestamp | Server-generated timestamp automatically set when the announcement is added. Don't set this manually. |

---

## How Announcments are Sent

The `announcementAlert` function manages the full lifecycle of announcements. This works in the following steps:

**1. Create Announcement**

When a new document is added to `/announcements`, the `announcementAlert` Cloud Function triggers.

Example Firestore entry:

```json
{
    "title": "Community Dinner",
    "description": "Join us for a community dinner this Friday after Maghrib.",
    "platforms": ["mobile"]
}
```

**2. Notification Processing**

The function sets a server-side `timeStamp` field with a time stamp of when the announcement is created.

Notifications are only sent if the `platforms` array includes `"mobile"`. If the `platforms` field is missing or not an array, **no notification is sent** (fails silently).

**3. Notification Payload**

A Firebase Cloud Messaging (FCM) payload is generated and sent. This payload is then received by clients that are subscribed to announcements (announcements enabled in settings on mobile).

```json
{
    "topic": "announcements",
    "notification": {
        "title": "Community Dinner - New Announcement",
        "body": "Join us for a community dinner this Friday after Maghrib.",
        "android_channel_id": "announcements_channel"
    },
    "data": {
        "notificationType": "announcements",
        "topic": "announcements"
    }
}
```

---

## Notes

* Announcements are **event-driven**; no manual triggering is required once a document is created.
* The workflow can be extended in the future to include:
    * Scheduled or expiring announcements (e.g., auto-remove old announcements).
    * Rich media (images, links) in notifications.

---

## Common Issues & Troubleshooting

Here are some common problems you may encounter when working with announcements, and how to resolve them:

**1. No Notification Sent**

   * Cause: The `platforms` field is missing or not set to `["mobile"]`.
   * Fix: Ensure your Firestore document includes `"platforms": ["mobile"]`.

**2. Timestamp Not Appearing**

   * Cause: The announcement was created manually with a `timeStamp` field.
   * Fix: Let the function set `Timestamp.now()` automatically. Remove any manually set values.

**3. Invalid Android Channel ID**

   * Cause: The environment variable `ANDROID_CHANNEL_ID_ANNOUNCEMENTS` was not configured.
   * Fix: Set it using:

     ```bash
     firebase functions:config:set ANDROID_CHANNEL_ID_ANNOUNCEMENTS="announcements_channel"
     ```

**4. FCM Send Errors in Logs**

   * Cause: Firebase project may not have Cloud Messaging enabled, or credentials are invalid.
   * Fix:

     * Ensure Firebase Cloud Messaging is enabled in the Firebase Console.
     * Regenerate your service account key if necessary.
     * Check logs with `npm run logs` for specific error codes.
