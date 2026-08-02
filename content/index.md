# Kelowna Islamic Center Project Overview

As-salamu alaykum.

Welcome to the Kelowna Islamic Center (KIC) software documentation.

This documentation covers the software ecosystem used by KIC to provide the community with prayer times, announcements, and notifications across multiple platforms. It is intended for developers, contributors, and administrators who want to understand, maintain, or extend the project.

## Project Components

The KIC ecosystem consists of three primary components, each maintained in its own repository.

### 1. [Server-Side Firebase Infrastructure](./server-side/index.md)

The backend is built using Firebase Cloud Functions and Firestore. It provides the server-side functionality shared by all KIC applications, including announcement storage, push notifications, and the prayer times API.

### 2. [Mobile App](./mobile-app/index.md)

The KIC Mobile App is the primary community-facing application for Android and iOS. It provides prayer times, Adhan and Iqamah notifications, community announcements, and administrative tools for authorized users.

### 3. [Kiosk App](./kiosk-app/index.md)

The KIC Kiosk App powers the Masjid display screens, providing real-time prayer times and synchronized announcements. It is built with SvelteKit and deployed as a web application using Firebase Hosting.

## Getting Started

If you are new to the project, the recommended reading order is:

1. Read the [**Application Architecture**](./application-architecture.md) section for a high-level overview of how the system is designed and how its components communicate.
2. Explore the [**Server-Side Firebase Infrastructure**](./server-side/index.md) documentation to understand the backend services, Cloud Functions, and Firestore.
3. Read the [**Mobile App**](./mobile-app/index.md) documentation for the Flutter application's architecture, features, installation, and deployment.
4. Read the [**Kiosk App**](./kiosk-app/index.md) documentation for the SvelteKit application, including local development, configuration, and deployment.

## Contributing

All projects documented here, including this documentation itself, are open source under the GPL v3 license and are available on GitHub.

Contributions are welcome, whether to improve the existing applications, add new features, or adapt the system for use by another Masjid or Islamic organization.
