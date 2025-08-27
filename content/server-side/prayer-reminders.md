# Prayer Reminders

The **Prayer Reminders** system delivers sheduled notifications for Athan and Iqamah reminders to the mobile app. These notifications used to be handled locally on device however due to recent software updates on both Android and iOS this has been moved to the cloud to allow for more control over notifications.

It’s implemented with two Cloud Functions that work together:

Every day at **07:00 UTC** (**12:00 AM PST**) the [`prayerTimesAlertScheduler`](./cloud-functions/prayer-times-alert-scheduler.md) function runs and schedules all the notification jobs for the day through Cloud Tasks.

Once a its time for notification to be delivered, the notification job is run in Cloud Tasks by running the [`sendPrayerAlert`](./cloud-functions/send-prayer-alert.md) function which then sends the localized notifications to clients via FCM.

---

## Topics & Subscriptions (FCM)

Clients receive reminders by subscribing to **topics**:

- **Athan:** `athanAlert`
- **Iqamah (reminders):** `iqamah5MinuteAlert`, `iqamah10MinuteAlert`, `iqamah15MinuteAlert`, `iqamah20MinuteAlert`, `iqamah30MinuteAlert`, `iqamah45MinuteAlert`
- **Language filter:** `lang-<locale>` (e.g., `lang-en`, `lang-ar`, …)

> **Note:** Iqamah notifications additionally require users to be on the **`iqamahAlert`** topic (opt-in gate).

---

## Important Notes

- **Friday handling:** When its Friday, the system skips `duhr` and `jumuah` is used instead. The opposite behaviour exists on non-friday days.
- **Shurooq** is skipped in notification scheduling as it is not a mandatory prayer.
- **Locales:** Notification strings are generated per entry in `locale.json`.
- **Time formatting:** All times are in the human-readable format (e.g., `1:30 PM`).

---

## Common Issues & Troubleshooting

**1) No notifications are delivered**

- **Cause:** Devices aren’t subscribed to the expected topics.  
- **Fix:** Ensure the app subscribes users to `athanAlert` and/or the specific `iqamah*MinuteAlert` topics, **plus** `lang-<locale>`. For iqamah, also require `iqamahAlert`.

**2) Iqamah reminders don’t fire**

- **Cause:** Users didn’t opt-in to `iqamahAlert`, or the minute-specific topic name doesn’t match.  
- **Fix:** Verify both `iqamahAlert` **and** the minute topic (e.g., `iqamah15MinuteAlert`) are subscribed.

**3) Scheduler doesn’t run**

- **Cause:** Cloud Scheduler or required Google Cloud APIs are disabled; billing plan insufficient.  
- **Fix:** Enable **Cloud Scheduler** and **Cloud Tasks**, ensure Blaze plan if required, and confirm the function is deployed in the intended region.

**4) Times look off (early/late)**

- **Cause:** Timezone mix-ups between **America/Vancouver** and **America/Chicago** conversions.  
- **Fix:** Check logs for parsed prayer times and scheduled UTC times. Confirm server region and conversion logic.

**5) Missing sounds or channel errors**

- **Cause:** Android channel IDs not set, or iOS sound not bundled.  
- **Fix:** Set `ANDROID_CHANNEL_ID_ATHAN_ALERTS` / `ANDROID_CHANNEL_ID_IQAMAH_ALERTS`. Ensure iOS sound files are included in the app.

**6) FCM errors in logs**

- **Cause:** Cloud Messaging not enabled or service account permissions incorrect.  
- **Fix:** Validate FCM setup in Firebase Console and recheck service account credentials.
