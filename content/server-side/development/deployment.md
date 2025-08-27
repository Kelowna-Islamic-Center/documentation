# Deployment

This guide explains how to deploy Firebase Cloud Functions after you are done making changes.

Ensure you either have access to the actual KIC Firebase project if you are an active maintainer or you have your own Firebase project set up and configured.

!!! danger
    This is a guide on how to deploy to a production environment. Ensure you have properly tested all API routes, functions, and Firestore documents before proceeding, or that you are deploying to a sandbox environment.

## Emulator Testing

Before deployment, ensure your functions run properly. Test functions locally before production.

```bash
npm run serve
```

!!! warning
    Firebase messaging always operates in production. If you send a notification in your emulator while testing to a live production topic, all your clients will receive that notification despite the function running in an emulator.

---

## Prerequisites

Start by ensuring you have the Firebase CLI installed and configured. If you tested your functions, this should already be installed, as the emulator also requires the Firebase CLI to be installed.

```bash
npm install -g firebase-tools
```

Once installed, you will also have to link the project to your Firebase project with:

```bash
firebase use --add
```

---

## Deploy

Build the project.

This also runs the linter, ensuring you do not have syntax or style mistakes within your project.

```bash
npm run build
```

Once built, you can deploy all the functions by running the deploy script:

```bash
npm run deploy
```

or by deploying each function individually:

```bash
firebase deploy --only functions:prayerTimesFetch
```

Once deployed, your changes should be live within your Firebase project. You can verify that everything went smoothly by reading the function logs.

```bash
npm run logs
```

🎉 Congratulations, you have deployed the Cloud Functions successfully.
