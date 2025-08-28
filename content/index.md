# Kelowna Islamic Center Project Overview

As-salamu alaykum.

Welcome to the **Kelowna Islamic Center (KIC) documentation**.  
This project covers all digital systems used by KIC to provide the community with prayer times, announcements, and notifications across multiple platforms.  

The purpose of this documentation is to help developers, contributors, and administrators understand the full ecosystem and each component of the project.

---

## Project Components

The KIC project consists of three main interconnected components, each maintained in its own repository:

### 1. [Server-Side Cloud Functions](./server-side/index.md)

The server-side infrastructure is built using **Firebase Cloud Functions**.  
It manages:

- **Prayer Reminders:** Automated scheduling of Adhan and Iqamah notifications for each prayer time.  
- **Announcements:** Event-driven notifications sent when new announcements are created.
- **Cloud Messaging:** Delivery of notifications to the mobile apps.  

Previous versions relied on a PHP server; the migration to Cloud Functions was made to reduce maintenance and administration overhead, as no traditional managed server is required.

---

### 2. [Mobile App](./mobile-app/index.md)

The KIC Mobile App is the most widely used application and is the community-facing client, available on the Play Store and App Store. It allows users to:

- Receive push notifications for prayer times and announcements.  
- Access community information and upcoming events.  
- Admins can create announcements and manage community engagement.

The mobile app communicates with Firestore and the server-side Cloud Functions to receive real-time updates.

---

### 3. [Kiosk App](./kiosk-app/index.md)

The KIC Kiosk App is deployed on display kiosks in the main hall of the **Masjid**, replacing traditional manual wall prayer clocks.  

It provides:

- Real-time prayer times with Adhan and Iqamah updates, fully synced and requiring no maintenance.
- An announcements carousel that cycles through community announcements synced from Firestore.

The kiosk app is built as a SvelteKit-powered web application that runs in a browser, making it cross-platform and able to run on any display device.

---

## Getting Started

To explore the project, follow these steps:

1. Read the **Architecture** section to understand data flow and notification mechanisms.
2. Read the **Server-Side Functions** section to understand backend workflows.
3. Check the **Mobile App** section for client-facing functionality.
4. Explore the **Kiosk App** section for deployment and installation instructions.

---

## Contributing

All projects documented here, including this documentation itself, are open source under the GPL-v3 license and are available on GitHub.

Contributions are welcome, whether to add new features, create your own version, or adapt it for use in a Masjid outside of Kelowna Islamic Center.

---

This overview serves as your starting point for understanding the full KIC system. Each section contains detailed guides for installation, deployment, and development to help contributors get up and running quickly.
