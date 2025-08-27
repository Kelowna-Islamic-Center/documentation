# Installation

This guide explains how to set up the [**cloud-functions**](https://github.com/Kelowna-Islamic-Center/cloud-functions) repository for local development, configure Firebase, and run the project in an emulator before deployment.

---

## Prerequisites

Before starting, make sure you have the following installed and configured on your machine:

**Node.js v20+**

Required for running and building Firebase Cloud Functions.

**Firebase CLI**

  Install it globally with:

```bash
npm install -g firebase-tools
```

**Firebase Project Access**
  
  You need access to either:

  * The official KIC Firebase project (if you are a maintainer), or
  * Your own Firebase project (for testing and development).

---

## Repository Structure

The repository is structured as follows:

```text
functions/
├── src/
│   ├── announcementAlert.ts           # Sends notifications for new announcements
│   ├── prayerTimesAlertScheduler.ts   # Schedules daily prayer notifications
│   ├── sendPrayerAlert.ts             # Dispatches queued notifications
│   ├── prayerTimesFetch.ts            # HTTP endpoint for prayer times
│   └── locale.json                    # Localization strings for notifications
├── package.json                       # NPM dependencies and scripts
└── tsconfig.json                      # TypeScript configuration
```

* All Cloud Functions are written in **TypeScript** and located in the `src/` directory.
* `locale.json` stores internationalized strings used in push notifications.

---

## Setup Steps

### 1. Clone the Repository

Clone the repo from GitHub and navigate into the project folder:

```bash
git clone https://github.com/Kelowna-Islamic-Center/cloud-functions
cd cloud-functions/functions
```

### 2. Install Dependencies

Install all required dependencies:

```bash
npm install
```

### 3. Configure Firebase Environment

The functions depend on environment variables for API endpoints and notification channel IDs. Configure them with:

```bash
firebase functions:config:set \
  API_LINK="https://org.thebcma.com/api/Prayertimes/GetPrayertimeByDate?organizationId=7&dt=" \
  ISLAMIC_FINDER_API_LINK="https://www.islamicfinder.us/index.php/api/prayer_times?latitude=49.8863&longitude=119.4966&timezone=america/vancouver&method=2&juristic=0&time_format=0&date=" \
  PRAYER_TIMES_FETCH_URL="https://prayertimesfetch-ilgk6gl75q-uc.a.run.app" \
  ANDROID_CHANNEL_ID_ATHAN_ALERTS="athan_alert_channel" \
  ANDROID_CHANNEL_ID_ANNOUNCEMENTS="announcements_channel" \
  ANDROID_CHANNEL_ID_IQAMAH_ALERTS="iqamah_alert_channel"
```

Verify the configuration was set correctly:

```bash
firebase functions:config:get
```

### 4. Run Locally (Emulator)

To emulate the functions locally:

```bash
npm run serve
```

This will start the Firebase Emulator Suite, allowing you to test HTTP-triggered and scheduled functions before deploying.

!!! warning
    Firebase messaging always operates in production. If you send a notification while testing in the emulator, **all subscribed clients will still receive it**. Be careful when working with notification-related functions.

---

## Emulator Testing Workflow

The Firebase Emulator allows you to test each function before deploying. Below are examples of how to test the main functions included in this project.

### 1. Testing `prayerTimesFetch` (HTTP Function)

Once the emulator is running, you should see a link in your terminal for testing this function. Open this link in your browser or by querying it through `curl` and you should see the response of the HTTP function.

---

### 2. Testing `announcementAlert` (Firestore Trigger)

This function runs when a new announcement document is created.

1. Open the **Emulator UI** (usually at [http://127.0.0.1:4000](http://127.0.0.1:4000)).
2. Navigate to Firestore → Add Document.
3. Add a new document in the `announcements` collection.

!!! warning
    Ensure you have configured a non-production notification channel (ex. announcmenetsDev) before testing this.

The function will automatically trigger and attempt to send a notification. Check logs:

```bash
npm run logs
```

---

### 3. Testing `prayerTimesAlertScheduler` (Scheduled Function)

Since schedules don’t fire automatically in the emulator, you need to invoke this manually:

```bash
firebase functions:shell
```

Inside the shell, run:

```text
prayerTimesAlertScheduler()
```

This simulates the daily scheduled execution. You should see log output showing that notifications were queued.

---

### 4. Testing `sendPrayerAlert` (Task Queue Function)

This function is normally triggered by the scheduler, but you can also invoke it manually:

```bash
firebase functions:shell
```

```text
sendPrayerAlert({ # You params })
```

The function will simulate sending a localized prayer alert.

---

## Next Steps

Once installation and local testing are complete, follow the [Deployment Guide](./deployment.md) to push your changes to Firebase.
