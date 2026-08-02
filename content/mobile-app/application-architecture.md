# Mobile App Architecture

The mobile application is built with Flutter and targets both Android and iOS from a single codebase. The architecture favors simplicity, predictable behavior, and clear separation of responsibilities while providing platform-specific implementations only where required.

Most application logic is shared between platforms, with native Android and iOS code used only for functionality that cannot be implemented reliably through Flutter alone.

## Application Structure

The application is divided into four primary layers.

### Presentation Layer

The presentation layer contains the application's screens, navigation, and user interface components. It is responsible for displaying prayer times, announcements, onboarding, and settings while forwarding user interactions to the application layer.

### Application Layer

The application layer coordinates business logic and application state. It handles user actions, manages settings, data refreshes, and coordinates interactions between the UI and underlying services.

### Service Layer

The service layer provides reusable background and foreground services used throughout the application. These include prayer time retrieval, announcement synchronization, notification handling, authentication, and alert scheduling.

### Data Layer

The data layer is responsible for storing and retrieving application data.

* SharedPreferences stores application settings and cached data.
* Firestore stores announcement content.
* Firebase Cloud Messaging delivers push notifications.
* Firebase Authentication secures administrative functionality.

## Cross-Platform Design

The majority of the application is implemented entirely in Flutter. Platform-specific implementations are introduced only where operating system capabilities or platform restrictions require them.

This approach allows the application to maintain consistent behavior across Android and iOS while still taking advantage of native platform features when necessary.

## Admin Features

The mobile application includes admin tools for managing announcements. Authorized admins authenticate with Firebase Authentication and can create, edit, and delete announcements directly within the app.

Announcements can be targeted independently to mobile clients, web clients, or both.

## Supporting Systems

Several subsystems support the application's runtime behavior and are documented separately:

* [Prayer Alerts Architecture](./alert-system-overview.md)
* [Background Services](./background-services.md)

## Design Principles

The application architecture is guided by several principles:

* Maintain a single shared codebase whenever practical.
* Introduce native platform code only when required by platform capabilities.
* Keep responsibilities separated between presentation, application, service, and data layers.
* Prefer local caching to improve responsiveness and reduce unnecessary network requests.
* Keep individual subsystems independently documented to simplify maintenance.
