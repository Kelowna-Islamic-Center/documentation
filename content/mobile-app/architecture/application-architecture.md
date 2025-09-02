# Mobile App Architecture

The Kelowna Islamic Center Mobile App is built with Flutter and follows a simplified **Model–View–Controller (MVC)** architecture. This approach keeps the project maintainable, easy to understand, and scalable for future features.

---

## Architecture Overview

### 1. **Model**

The Model represents the data structures of the app, including:

   - Prayer Times (fetched from the API)  
   - Announcements (fetched in real-time from Firestore)  
   - Notification Payloads (from Cloud Messaging)  

Models are typically simple Dart classes or data objects.

---

### 2. **View**

The View is just the main Flutter UI Widget for a specific screen:

   - Prayer timetable screens  
   - Announcements feed screen
   - Settings screen
   - Admin tools and Authentication screens

Views are reactive and update automatically when data from the controller or services changes.

---

### 3. **Controller**

Controllers manage user interactions and business logic. Flutter does let you combine controllers and views by having all the logic with the widgets but this shoud be avoided. With this seperated approach, files remain short, easy to maiontaine, and you can share logic with other controllers (ex. settings controller is used by other controllers to see what the user selected).

Controllers do things like:

- Handling API responses
- Managing Firestore streams
- Coordinating between services and updating views

This layer ensures that views remain focused only on the UI and are not bloated with logic.

---

### 4. **Services**

Background processes are referred to as services. These files are similar to controllers however they are background processes and do not run in the foreground. Periodic tasks and cloud messaging are handled through services.

There are currently two active services:

   - `cloud_messaging_service.dart` → Handles Firebase Cloud Messaging subscription, notification parsing, and topic management.  
   - `api_fetch_service.dart` → Handles periodic HTTP requests to the [Prayer Times API](../../server-side/prayer-times-api.md) so the app works for devices without mobile data.

Read more about services on the [Background Services](./background-services.md) page.

---

## Future Additions

**Unit Testing** - Currently, unit testing is not implemented. In the future, adding tests for controllers and services will:

   - Ensure correctness of data parsing.  
   - Prevent regressions in notifications and API calls.  
   - Improve overall maintainability.
