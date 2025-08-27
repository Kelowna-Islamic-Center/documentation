# Kiosk App Overview

The **KIC Kiosk App** is a SvelteKit-based web application deployed on dedicated display kiosks inside the masjid.

Its purpose is to provide the community with real-time athan and iqamah times aswell as announcements all in a modern and automated format that syncs with the mobile app.

The previous solution to prayer time displays at the masjid was with a manual digital clock that would require adjustment every week or so. The kiosk app replaces this system with a centrally managed, cloud solution that automatically syncs prayer times and announcmeents from the server.

You can find the full source code for the kiosk app on Github:

[:simple-github: Kelowna-Islamic-Center/kiosk](https://github.com/Kelowna-Islamic-Center/kiosk){ .md-button }

---

## Key Features

- **Prayer Times Display**  
  Shows both adhan and iqamah times, updated daily via the Cloud Functions scheduler.

- **Announcements Carousel**  
  Displays community announcements synced from Firestore.

- **Live Updates**  
  No need for daily adjustments—times and messages are synced automatically.

- **Cross-Platform**  
  Since the kiosk app is a web application it can be deployed on windows and linux powered kiosk computers allowing Deployed via Firebase Hosting with SSR, but optimized for **kiosk displays** in the masjid.

---

## Developement

The app is deployed using **Firebase Hosting with SSR**.  
For installation and deployment instructions, see:

- [Installation](./installation.md)  
- [Deployment](./deployment.md)  
