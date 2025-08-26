# sendPrayerAlert

**Type:** Task Queue Function (onTaskDispatched)

---

## Description

Handles queued payloads created by `prayerTimesAlertScheduler` and sends localized push notifications for **athan** and **iqamah reminders**.

---

## Retry and Rate Limits

- **Retries:** max 3 attempts
- **Backoff:** 30 seconds minimum
- **Max concurrent dispatches:** 5

---

## Payload Parameters

| Name          | Type     | Description |
|---------------|----------|-------------|
| `id`          | string   | Prayer ID (`fajr`, `duhr`, `asr`, `maghrib`, `isha`, `jumuah`) |
| `type`        | string   | `"athan"` or `"iqamah"` |
| `androidChannel` | string | Android notification channel ID |
| `topic`       | string   | FCM topic name |
| `minutes`     | number   | Minutes before iqamah (if applicable) |
| `time`        | Date     | Scheduled notification time |

---

## Notification Behavior

- Sends a localized notification for **each supported locale**.
- Uses `locale.json` to translate prayer names and notification text.
- Plays **Athan sound** for athan notifications (`athan_full` on Android, `athan_short.caf` on iOS).

---

## Example Notification Payload

This notification payload is sent to Firebase Messaging which delivers it to the mobile apps and the web kiosk app.

```json
{
  "condition": "'athanAlert' in topics && 'lang-en' in topics",
  "notification": {
    "title": "Fajr - Athan",
    "body": "Time for Fajr prayer"
  },
  "android": {
    "notification": {
      "channelId": "ANDROID_CHANNEL_ID_ATHAN_ALERTS",
      "sound": "athan_full"
    }
  },
  "apns": {
    "payload": {
      "aps": {
        "sound": "athan_short.caf"
      }
    }
  },
  "data": {
    "notificationType": "athan",
    "topic": "athanAlert"
  }
}
```
