# Mobile App Overview

The Kelowna Islamic Center Mobile App is the most used app and the primary tool the community uses to stay connected with the masjid. Its core functionality is the prayer times clock with all secondary functionality being added on top of this.

This entire section of the documentation explains how the mobile app works, important things to note about it, and how to get started with its development.

The application is built with Flutter allowing it to have a single codebase for both Android and iOS.

You can find the full source code for the mobile app on Github:

[:simple-github: Kelowna-Islamic-Center/mobile](https://github.com/Kelowna-Islamic-Center/mobile){ .md-button }

---

## High-Level Features

- **Prayer Time Clock**
    - Fetched automatically and displayed in an easy-to-use interface emulating the kiosk app.
- **Announcements**
    - Delivered instantly through FCM and managed by administrators.
- **Notifications**
    - Athan, Announcements, and Iqamah reminder notifications delivered through FCM
- **Admin Tools**
    - The mobile app is the only place to modify annoncements on the mobile app and the kiosk app aswell as sending push notifications to all community members.

---

## Architecture

The app follows a Model-View-Controller (MVC) pattern for simplicity and maintainability:  

- **Controllers** - Handle interaction between views and services.  
- **Views** - Flutter UI screens displaying prayer times, announcements, and settings.  
- **Models** - Represent core entities like Prayer Times and Announcements.  
- **Services** - Dedicated files like `cloud_messaging_service.dart` and `api_fetch_service.dart` that manage external API calls and notifications.  

> While unit tests are not yet implemented, they should be considered in future iterations to ensure broken updates are not built for production.  

---

## Developement

The app is is a Flutter application.
For installation and deployment instructions, see:

- [Installation](./development/installation.md)  
- [Deployment](./development/building.md.md)  
