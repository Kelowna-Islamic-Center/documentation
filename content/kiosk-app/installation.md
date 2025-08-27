# Installation

The KIC Kiosk App is built with [SvelteKit](https://kit.svelte.dev/) and [Vite](https://vitejs.dev/).  
This guide walks you through setting up the development environment and running the app locally.

Before starting, ensure you have [Node.js](https://nodejs.org/) **18+** installed.

---

## Setup

**1. Clone the Repository**

```bash
git clone https://github.com/Kelowna-Islamic-Center/kiosk
```

**2. Install Dependencies**

```bash
npm install
```

**3. Configure Environment Variables**

Copy the `.env.example` file and rename it to `.env` in the project root. This file stores all your environment variables such as API and Firebase configuration values that the app needs to run.

Example `.env`:

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
    In SvelteKit, only variables starting with `PUBLIC_` are exposed to client-side code.  
    Do not put private keys (like Firebase service account credentials) in `.env`.

---

## Development

To start the development server with hot-reloading:

```bash
npm run dev
```

By default, the app will be available at:

```
http://localhost:5173
```

---

## Build for Production

To create an optimized production build:

```bash
npm run build
```

Preview the production build locally with:

```bash
npm run preview
```

---

## Linting & Type Checking

Run static analysis and type checks to catch issues early:

```bash
npm run check
```

Or run in watch mode:

```bash
npm run check:watch
```

---

## Notes

- The app uses **SvelteKit + Vite** with **TypeScript** enabled.
- Styles are built using **Sass** (`.scss` files supported).
- Ensure your `.env` file is properly configured before starting development or building for production.
- The kiosk fetches **prayer times** from the configured `PUBLIC_API_LINK` and integrates with Firebase services for data and messaging.
