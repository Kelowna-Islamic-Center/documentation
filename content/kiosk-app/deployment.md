# Deployment

The KIC Kiosk App is deployed using **Firebase Hosting** with **Server-Side Rendering (SSR)** via SvelteKit’s Firebase adapter.

This guide explains how to deploy the app, best practices for managing Firebase config files, and how to regenerate them if needed.

---

## Prerequisites

Before deploying, ensure you have the following:

- Firebase project created in the [Firebase Console](https://console.firebase.google.com/)
- Firebase CLI installed globally:

```bash
npm install -g firebase-tools
```

- You are logged into Firebase with your Google account:

```bash
firebase login
```

---

## Regenerating Config Files

!!! note
    This section can be skipped if you are deploying to the production Firebase project as a maintainer and not your own Firebase project.

If you need to deploy this repo to a **new Firebase project**, you must regenerate the Firebase configuration files. These files are part of the repository and have the default values for the production Firebase project.

1. Initialize Firebase in your project:

   `firebase init hosting`

2. Choose the correct Firebase project from the list (or create a new one).

3. When prompted select **Configure as a single-page app** = **Yes** (important for SvelteKit SSR rewrites) and overwrite `firebase.json` and `.firebaserc` for the new config files.

This will generate new `firebase.json` and `.firebaserc` files linked to your selected Firebase project.

---

## Environment Variables

!!! note
    This section can be skipped if you are deploying to the production Firebase project as a maintainer and not your own Firebase project.

The `.env` file values created during [Installation](./installation.md) must also be modified if you are for your own Firebase project.

If you set them to the correct Firebase project values during installation then this part can be skipped. Otherwise the following values must be updated to match the project you initialized in the previous step.

```env
PUBLIC_FB_API_KEY="..."
PUBLIC_FB_AUTH_DOMAIN="..."
PUBLIC_FB_PROJECT_ID="..."
PUBLIC_FB_STORAGE_BUCKET="..."
PUBLIC_FB_MESSAGING_SENDER_ID="..."
PUBLIC_FB_APP_ID="..."
```

---

## Deployment Steps

**1. Build the project**

Compile the SvelteKit app for production:

```bash
npm run build
```

**2. Deploy to Firebase Hosting**

```bash
firebase deploy
```

If you are deploying to the production Firebase project then the app should be live at [kelownaislamiccenter.web.app](https://kelownaislamiccenter.web.app/). If you deployed to your own project, you can find the deployment URL in the Firebase console.

🎉 Congratulations, the kiosk app should be deployed. You can now refresh any Masjid display screens configured to use it.
