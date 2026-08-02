# Developing Cloud Functions

This guide covers setting up the Cloud Functions project for local development, testing it with the Firebase Emulator, and deploying your changes.

[:simple-github: Kelowna-Islamic-Center/cloud-functions](https://github.com/Kelowna-Islamic-Center/cloud-functions){ .md-button }

## Prerequisites

Before getting started, ensure you have Node.js v20 or later.

If you already have Node.js installed, install the Firebase CLI with:

```bash
npm install -g firebase-tools
```

You'll also need access to either:

* The official KIC Firebase project (for maintainers), or
* Your own Firebase project for local development and testing.

## Initial Setup

Clone the repository:

```bash
git clone https://github.com/Kelowna-Islamic-Center/cloud-functions
cd cloud-functions/functions
```

Install dependencies:

```bash
npm install
```

Associate the project with your Firebase project:

```bash
firebase use --add
```

Several functions rely on Firebase Runtime environment variables that must be set. Configure them by adding them to the `.env.kelownaislamiccenter` file or with:

```bash
firebase functions:config:set \
  API_LINK="https://org.thebcma.com/api/Prayertimes/GetPrayertimeByDate?organizationId=7&dt=" \
  THIRD_PARTY_API_LINK="https://api.aladhan.com/v1/timings/{date}?iso8601=true&method=2&latitude=49.8863348&longitude=-119.4934836" \
  PRAYER_TIMES_FETCH_URL="https://prayertimesfetch-ilgk6gl75q-uc.a.run.app" \
  ANDROID_CHANNEL_ID_ATHAN_ALERTS="athan_alert_channel" \
  ANDROID_CHANNEL_ID_ANNOUNCEMENTS="announcements_channel" \
  ANDROID_CHANNEL_ID_IQAMAH_ALERTS="iqamah_alert_channel"
```

You can verify the configuration at any time with:

```bash
firebase functions:config:get
```

## Running the Emulator

Start the Firebase Emulator Suite with:

```bash
npm run serve
```

This launches the Functions emulator and allows you to test your changes before deploying. If you make any changes to the functions while the emulator is running, you will have to re-run the command above to see your changes emulated.

!!! warning
    Firebase Cloud Messaging is not emulated. Any notification sent by your local functions is still delivered through the production FCM service. If you publish to a production topic while testing, all subscribed devices will receive the notification.

## Testing Functions

### `prayerTimesFetch`

Once the emulator starts, it prints the local URL for the HTTP function.

Open the URL in your browser or call it using `curl` to verify that the endpoint returns the expected response.

### `announcementAlert`

`announcementAlert` is triggered whenever a document is created in the `announcements` Firestore collection.

To test it:

1. Open the Emulator UI (typically `http://127.0.0.1:4000`).
2. Navigate to Firestore.
3. Create a new document inside the `announcements` collection.

The function should execute automatically.

!!! warning
    Configure a development notification topic (for example, `announcementsDev`) before testing. Using the production topic will send notifications to every subscribed client in production.

To inspect the function output:

```bash
npm run logs
```

## Deploying

Once you've verified everything works locally, build the project:

```bash
npm run build
```

The build process also runs the linter to catch any syntax or style issues before deployment.

Deploy all Cloud Functions with:

```bash
npm run deploy
```

!!! danger
    When deploying, the Firebase CLI will ask you if you would like to remove deployed functions that are not included in this repository. Ensure you **DO NOT DELETE** these functions as they are legacy functions needed to allow older versions of the mobile app to work. Deleting those functions will break older versions of the mobile app that are still used by some community members.

Or deploy a single function:

```bash
firebase deploy --only functions:prayerTimesFetch
```

Your updated Cloud Functions are now live in your Firebase project.
