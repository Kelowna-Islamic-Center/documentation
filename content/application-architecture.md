# Kelowna Islamic Center Application Architecture

This page provides a high-level overview of how the KIC software ecosystem is structured and how its components communicate. Understanding this architecture will make the implementation details in the remaining documentation much easier to follow.

## Design Goals

The KIC ecosystem was designed around a few core principles:

* **Serverless infrastructure** to minimize maintenance and operational overhead.
* **Real-time synchronization** so community information is updated across all applications automatically.
* **Shared backend services** used by both the Mobile App and Kiosk App.
* **Cross-platform clients** built with technologies that are easy to maintain and deploy.
* **Modular components** that can be developed independently while sharing common backend infrastructure.

## High-Level Components

The architecture consists of four primary components.

### Firebase Cloud Functions

Firebase Cloud Functions provide the backend business logic for the ecosystem. They are responsible for:

* Processing announcement events.
* Delivering push notifications through Firebase Cloud Messaging (FCM).
* Providing the prayer times API.
* Integrating with external services.

### Firestore

Firestore stores shared application data, primarily announcements. Clients subscribe to Firestore updates to receive new announcements in real time.

### Client Applications

Two client applications consume the backend services:

* **Flutter Mobile App** for Android and iOS community members.
* **SvelteKit Kiosk App** for Masjid display screens.

Both clients communicate through Firebase services rather than directly with one another.

### External Services

The backend integrates with external providers, such as BCMA and Islamic Finder, to retrieve prayer time data before presenting it in a consistent format to client applications.

## System Communication

At a high level:

1. Administrators create announcements from the Mobile App.
2. Announcements are stored in Firestore.
3. Cloud Functions process the new announcement and send localized push notifications.
4. Mobile devices receive notifications through Firebase Cloud Messaging.
5. Both the Mobile App and Kiosk App automatically synchronize announcement content from Firestore.
6. Prayer time requests are served through Cloud Functions, which aggregate data from external providers.

## Architecture Diagram

The overall system architecture is illustrated below.

![KIC Application Architecture](../assets/architecture_diagram.png)

Although the diagram contains several interacting services, the communication follows a relatively simple pattern: both client applications rely on Firebase as the central backend, while Cloud Functions coordinate business logic, external API integration, and notification delivery.

The remainder of this documentation explores each of these components in detail.
