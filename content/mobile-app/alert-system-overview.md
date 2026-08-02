# Athan & Iqamah Alerts

The mobile app currently supports two native alert types. Athan alerts send a notification and play athan audio when it is time for the athan. Iqamah alerts send a reminder notification before the iqamah at a user-selected offset in minutes.

Both alert types are implemented through a native scheduling system with platform-specific implementations for Android and iOS. The core scheduler is written in Dart and computes which alerts should be scheduled for today and tomorrow. It then delegates scheduling to the Android native Java implementation or the iOS implementation, depending on the platform and alert type.

## The Alert Scheduler

The scheduler runs as a background service using Workmanager so it can continue performing checks even when the app is not running in the foreground. During each execution, it reads the cached prayer times and user settings, then schedules the appropriate athan and iqamah notifications for the current prayer windows.

## Alert Reconciliation

Whenever a relevant input changes, such as prayer times, language, or alert settings, the scheduler performs reconciliation. It generates a fingerprint from the cached prayer data and the current user settings, then compares it with the previously stored fingerprint. If the fingerprint has changed, all managed alerts are canceled and rebuilt (reconciliation). This ensures that changes to user settings are reflected in scheduled notifications immediately.

## Athan on iOS & Iqamah Notifications

After the Dart scheduler computes the schedule, iqamah notifications on both Android and iOS, as well as athan notifications on iOS, are scheduled using the `local_notifications` Dart package. These notifications are handled entirely through the application's cross-platform Dart code.

On iOS, athan notifications play a shortened athan audio containing only the first two "Allahu Akbar" phrases. This limitation exists because iOS has stricter constraints on notification sounds. These audio files are bundled with the application target as `athan_example_short.caf` resources.

The only exception to this scheduling approach is athan notifications on Android.

## Athan on Android

On Android, athan alerts are handled through a native Java implementation responsible for displaying notifications and managing the background services required for athan audio playback.

Android athan alerts use the native exact alarm APIs. Dart invokes a native method channel, and `MainActivity` routes scheduling requests to the native Android scheduler. The scheduler uses `AlarmManager.setExactAndAllowWhileIdle` on modern Android versions and `setExact` on older versions. When an alarm fires, a broadcast receiver starts a foreground playback service, which requests audio focus, configures alarm-appropriate audio attributes, and plays the selected athan resource.

This scheduling flow is as follows:

1. Dart requests that an athan alert be scheduled.
2. The native scheduler registers an exact alarm with the Android system.
3. When the alarm triggers, the broadcast receiver is invoked.
4. The receiver starts the foreground playback service.
5. The foreground service immediately begins athan audio playback.

### Android Dirty Flag Flow

The Android implementation also includes a separate native broadcast receiver that listens for system events that can invalidate scheduled alerts. Rather than attempting to fully reschedule alerts from native code, it sets a dirty flag in SharedPreferences and allows the Dart scheduler to perform a full reconciliation the next time the application starts or resumes.

The dirty flag is set on:

* Device boot completion
* Application package replacement (app update)
* Manual system time changes
* Time zone changes
* Changes to the exact alarm permission state

On the next startup or resume, the Dart scheduler checks for this flag, clears it, and forces a full reconciliation of all managed alerts.

---

The app also contains additional background services, which are documented separately in [Background Services](./background-services.md).
