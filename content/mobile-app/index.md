# Mobile App Overview

The Kelowna Islamic Center Mobile App is the primary app used by the community to stay connected with the masjid. Its core function is prayer time visibility, with announcements, alerts, and admin workflows built around that foundation.

This section serves as the entry point for mobile documentation. It links to architecture references and development guides without repeating the implementation details already covered in those pages.

The app is built with Flutter and ships from a single codebase to both Android and iOS.

You can find the full source code for the mobile app on Github:

[:simple-github: Kelowna-Islamic-Center/mobile](https://github.com/Kelowna-Islamic-Center/mobile){ .md-button }

## High-Level Features

The mobile app currently focuses on four core capabilities:

- Prayer time display synchronized with backend prayer data.
- Announcements delivered through Firebase-backed messaging and Firestore updates.
- Prayer alerts for athan and iqamah reminders.
- Admin tools used to create and manage announcement content for both mobile and kiosk consumers.

## Architecture

Architecture details are documented in the dedicated pages below:

- [Mobile App Architecture](./application-architecture.md)
- [Prayer Alerts Architecture](./alert-system-overview.md)
- [Background Services](./background-services.md)

## Development

For setup and release workflows, use the development guides:

- [Installation](./development/installation.md)  
- [Deployment](./development/building.md)  
