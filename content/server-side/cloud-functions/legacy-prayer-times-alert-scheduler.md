# prayerTimesAlertScheduler - Legacy

!!! warning
    **This function is deprecated and is no longer included within the cloud-functions repository.**

    This implementation was replaced with the modern local device native alerts system used by the app starting with mobile app v5. It is documented below for archival purposes only. This function remains deployed solely to allow legacy versions of the mobile app to continue functioning.

**Type:** Scheduled Function (onSchedule)

**Schedule:** Every day at 07:00 UTC (12:00 AM PST)

## Description

This function runs daily to:

- Fetch the prayer times from a configured API.
- Build notification payloads for **athan** and **iqamah reminders**.
- Schedule tasks in Firebase Task Queues to send the notifications at the appropriate times.

## Environment Variables

- `ANDROID_CHANNEL_ID_ATHAN_ALERTS`
- `ANDROID_CHANNEL_ID_IQAMAH_ALERTS`
- `PRAYER_TIMES_FETCH_URL`

## Behavior

1. Fetches prayer times for the current day.
2. Excludes **Shurooq**.
3. On **Fridays**:
   - Excludes **Duhr**
   - Includes **Jumuah**
4. Converts times to **America/Chicago** (server timezone).
5. Enqueues payloads into the task queue for `sendPrayerAlert`.

## Example Task Payload

This task payload is later parsed by [sendPrayerAlert](./send-prayer-alert.md) when it is activated by the cloud scheduler. It provides it with context on what notification to send and where to send it when it is activated.

```json
{
  "id": "fajr",
  "type": "iqamah",
  "androidChannel": "ANDROID_CHANNEL_ID_IQAMAH_ALERTS",
  "topic": "iqamah5MinuteAlert",
  "minutes": 5,
  "time": "2025-08-26T05:45:00.000Z"
}
```
