# announcementAlert

**Type:** Firestore Trigger (onDocumentCreated)

**Path:** `/announcements/{docId}`

---

## Description

Triggered whenever a new document is created in the `announcements` collection.  

This function:

- Adds a `timeStamp` field to the new document with the current server time.
- If the announcement is intended for the **mobile** platform, it sends a push notification to the **announcements** topic via Firebase Cloud Messaging.

---

## Accepted Methods

- **Trigger:** Firestore document creation

---

## Parameters

| Name       | Type     | Required | Description |
|------------|----------|----------|-------------|
| `docId`    | string   | Yes      | The Firestore document ID of the announcement. |
| `platforms`| string[] | No       | Platforms the announcement applies to. Must include atleast `"mobile"`, `"web"`, or both to trigger notifications. |
| `title`    | string   | Yes      | The announcement title. |
| `description` | string | Yes     | Announcement body text. |

---

## Example Notification Payload

This notification payload is sent to Firebase Messaging which delivers it to the mobile apps and the web kiosk app.

```json
{
  "topic": "announcements",
  "notification": {
    "title": "{title} - New Announcement",
    "body": "{description}",
    "android_channel_id": "ANDROID_CHANNEL_ID_ANNOUNCEMENTS"
  },
  "data": {
    "notificationType": "announcements",
    "topic": "announcements"
  }
}
```
