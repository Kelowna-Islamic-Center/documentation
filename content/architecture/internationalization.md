# Internationalization (i18n) Guidelines

Internationalization (i18n) is a requirement across all KIC applications and backend services. Since the community includes a significant number of refugees who primarily speak arabic, every user-facing feature must be designed with localization in mind.

---

## Current State of Internationalization

### ✅ Cloud Functions

All server-side notifications (e.g., athan alerts, iqamah reminders, announcements) are internationalized before being sent to Firebase Cloud Messaging (FCM).  

- Each notification is translated into supported languages using `locale.json`.
- Clients receive the correct localized message depending on their language/topic subscription.

### ✅ Mobile App

The Flutter Mobile App is fully internationalized:

- UI components, labels, and system messages are all language-aware.
- Users can switch languages in-app, with translations loaded dynamically.
- Notifications received from the backend are displayed in the correct language automatically.

### ⚠️ Kiosk App

The SvelteKit Kiosk App is **NOT YET** internationalized:

- Currently, it only displays English text.
- This is an area to focus on in future development:
    - UI strings should be stored in translation files rather than hard-coded.
    - Language selection should be configurable (e.g., default Masjid language or a rotating display).
    - Consistency with mobile app translations where possible.

---

## Guidelines for Developers

1. **Never hardcode strings** in UI or notifications.  
   Store them in translation files (JSON, ARB, or similar) and load dynamically. The Flutter mobile app already utilizes the Flutter Internationlization package to facilitate this.

2. **Follow the existing localization pattern** in your repo.  
   For example:
   - **Flutter:** `.arb` files with `intl` package.
   - **Cloud Functions:** `locale.json` and topic-based FCM notifications.
   - **Future Kiosk App (Not yet implemented):** JSON translation files and `i18n` libraries.

3. **Always add translations** for every new feature:
   - New notification types.
   - New UI components.
   - Error messages and system alerts.

4. **Test with multiple languages** before merging changes.  
   Missing translations or fallback to English should be logged and fixed.
