# Developing the Kiosk App

The KIC Kiosk App is built with SvelteKit and Vite, and is deployed using Firebase Hosting with Server-Side Rendering (SSR) via SvelteKit's Firebase adapter.

This guide walks through setting up a local development environment, configuring Firebase, and deploying the application.

## Prerequisites

Before getting started, ensure you have Node.js v20 or later.

If you already have Node.js installed, install the Firebase CLI with:

```bash
npm install -g firebase-tools
```

Log in to Firebase:

```bash
firebase login
```

## Setup

Clone the Repository:

```bash
git clone https://github.com/Kelowna-Islamic-Center/kiosk
```

Install Dependencies:

```bash
npm install
```

Once installed, configure Environment Variables. This can be done by copying `.env.example` as `.env` in the project root.

This file stores the API endpoint and Firebase configuration required by the application. Update the values to match your Firebase project.

Example:

```env
PUBLIC_API_LINK="https://prayertimesfetch-ilgk6gl75q-uc.a.run.app"

PUBLIC_FB_API_KEY="myapikey"
PUBLIC_FB_AUTH_DOMAIN="myproject.firebaseapp.com"
PUBLIC_FB_PROJECT_ID="myproject"
PUBLIC_FB_STORAGE_BUCKET="myproject.appspot.com"
PUBLIC_FB_MESSAGING_SENDER_ID="mysenderid"
PUBLIC_FB_APP_ID="myfbappid"
```

!!! danger
    In SvelteKit, only variables beginning with `PUBLIC_` are exposed to client-side code.
    Never store private credentials, such as Firebase service account keys, in `.env`.

## Running Locally

Start the development server with hot reloading:

```bash
npm run dev
```

The application will be available at:

```text
http://localhost:5173
```

You can now make whatever changes are required.

## Deploying

### Production Firebase Project

If you are a maintainer deploying to the existing production Firebase project, no additional Firebase configuration is required. Build and deploy the project:

```bash
npm run build
```

```bash
firebase deploy
```

The application will be deployed to:

<https://kelownaislamiccenter.web.app/>

### Deploying to a New Firebase Project

!!! note
    This section is only required if you are deploying to your own Firebase project.

#### Regenerate Firebase Configuration

Initialize Firebase in the project:

```bash
firebase init hosting
```

When prompted:

* Select your Firebase project (or create a new one).
* Choose **Yes** when asked to configure as a Single-Page Application.
* Allow `firebase.json` and `.firebaserc` to be overwritten.

This regenerates the Firebase configuration files for your project.

Ensure your `.env` file contains the Firebase configuration values for the project you just initialized.

```env
PUBLIC_FB_API_KEY="..."
PUBLIC_FB_AUTH_DOMAIN="..."
PUBLIC_FB_PROJECT_ID="..."
PUBLIC_FB_STORAGE_BUCKET="..."
PUBLIC_FB_MESSAGING_SENDER_ID="..."
PUBLIC_FB_APP_ID="..."
```

#### Build and Deploy

```bash
npm run build
```

```bash
firebase deploy
```

Firebase will output the hosting URL for your project once deployment completes.

## Next Steps

Once deployed, refresh any Masjid display screens configured to use the kiosk application so they begin serving the latest version.
