# Internationalization (i18n)

Supporting multiple languages is a core requirement across all KIC applications and backend services. The community includes many members who primarily speak Arabic, so every user-facing feature should be designed with localization in mind from the beginning rather than added later.

The platform currently supports English and Arabic, but the architecture should remain flexible enough to accommodate additional languages in the future.

## Current State

### Mobile App

The Flutter mobile application is fully internationalized.

* All UI components, labels, and system messages are localized.
* Users can switch languages directly within the app.
* Local notifications, such as athan and iqamah reminders, are generated using the user's selected language.
* Push notifications sent from Firebase Cloud Messaging are automatically delivered in the correct language through language-specific topic subscriptions.

### Cloud Functions

Cloud Functions are responsible for sending localized push notifications.

Announcement documents stored in Firestore contain both English and Arabic content. When an announcement notification is sent through Firebase Cloud Messaging, clients receive the appropriate language based on the notification topic they are subscribed to.

This means localization is handled before the notification reaches the device, ensuring users only receive messages in their selected language.

### Kiosk App

The SvelteKit kiosk application currently has basic multilingual support but is not fully internationalized.

At present:

* Announcement slides rotate between English and Arabic.
* Some UI text is hardcoded in both languages.
* Prayer names are displayed as combined bilingual strings.

While this works for the current kiosk deployment, it is not a scalable approach. UI strings should eventually be moved into dedicated translation files so the interface can switch languages in a consistent way, just as the mobile application does. Potential ways this could be done is rotating text as the application cycles through languages every few minutes.

## Development Guidelines

When adding new features, keep the following principles in mind:

1. **Never hardcode user-facing text.**

      Store strings in the project's localization files and load them dynamically. This keeps translations consistent and makes adding future languages significantly easier.

2. **Follow the localization approach used by the project you're working on.**

      For example:

      * **Flutter:** Use `.arb` files together with the `intl` package.
      * **Cloud Functions:** Send localized notifications using language-specific FCM topics.
      * **Kiosk App:** Migrate hardcoded strings into a proper translation system as the application evolves.

3. **Test every feature in multiple languages.**

      Before merging changes, verify that:

      * All user-facing text is translated.
      * Layouts remain usable in both English and Arabic.
      * Missing translations are identified and fixed instead of silently falling back to English whenever possible.

Internationalization should be considered part of feature development rather than a final polishing step. Any new UI, notification, or user-facing message should be designed with localization in mind from the outset.
