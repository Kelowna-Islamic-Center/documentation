# Admin Tools in the Mobile App

The mobile app includes a set of **administrative tools** that allow authorized masjid administrators to manage community updates.

Include note about how to get authenticaion accessa dn future impormevent to it

---

## Features

- **Add Announcements** → Create new announcements visible to all community members.  
- **Modify Announcements** → Edit existing announcements to correct or update information.  
- **Delete Announcements** → Remove outdated or incorrect announcements.  

---

## Security

- Admin tools are **behind Firebase Authentication**, ensuring that only authorized accounts can access them.  
- Non-admin users cannot view or interact with these tools.

---

## Notifications Integration

When an admin creates or modifies an announcement:  

- The [announcementsAlert](../.) Cloud Function triggers automatically.  
- A notification is sent via **Firebase Cloud Messaging (FCM)** to all app users.  

This ensures instant delivery of important information.  
