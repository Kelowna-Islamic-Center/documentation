# Prayer Times API

Since the Kelowna Islamic Center is part of the **BC Muslim Association (BCMA)**, all prayer times come directly from the BCMA API. However, this data is not usable directly and needs to be further processed into a JSON format that the mobile app and the kiosk app can better handle.

All clients, like the mobile app and the kiosk app, periodically check the [KIC API endpoint](https://prayertimesfetch-ilgk6gl75q-uc.a.run.app/) which is served by the [`prayerTimesFetch`](./cloud-functions/prayer-times-fetch.md) cloud function. This cloud function makes the request to the BCMA API and parses it into a cleaner format that the applications then receive.

!!! note
    The application architecture supports any API if required, not just the BCMA API. All that is required is that the `prayerTimesFetch` function be modified to accommodate the new API.

!!! info
    You can read more about cloud functions and this specific cloud function on the [Cloud Functions page](./cloud-functions.md).

---

## The BCMA API

The BCMA API endpoint for Kelowna Islamic Center (BCMA Organization 7) is:

```text
https://org.thebcma.com/api/Prayertimes/GetPrayertimeByDate?organizationId=7&dt={date}
```

* `{date}` formatted as `MM-DD-YYYY` in Pacific Time. The cloud function, when making its request, is configured to either use today's date or tomorrow's date.

### Response Format

The BCMA API responds with a key-only list of prayer times:

```json
{
  "$id": "1",
  "id": 6444,
  "prayerDate": "08/26/2022",
  "fajr": "4:02:00 AM",
  "fajrIqama": "5:00:00 AM",
  "sunrise": "6:03:00 AM",
  "zawal": "",
  "duhr": "1:04:00 PM",
  "duhrIqama": "1:30:00 PM",
  "asr": "5:48:00 PM",
  "asrIqama": "6:15:00 PM",
  "maghreb": "8:00:00 PM",
  "maghrebIqama": "",
  "isha": "9:35:00 PM",
  "ishaIqama": "9:50:00 PM",
  "organizationID": 7,
  "calendarDate": "2025-08-26T00:00:00",
  "firstJumma": "1:30:00 PM",
  "secondJumma": "",
  "traveeh": "",
  "disclaimer": null,
  "cacheKey": "Prayertime_7_8/26/2025 12:00:00 AM"
}
```

Out of all of these keys, only the essential prayer and iqamah keys are parsed and formatted. The cloud function then also queries the [Islamic Finder API](https://www.islamicfinder.us/index.php/api) for hanbali time, as KIC users use both hanafi and hanbali times (BCMA only provides hanafi).

After all processing is complete, the cloud function outputs the following result which is usable by the mobile and kiosk apps.

```json
[
  {
    "id": "fajr",
    "start": "04:02 AM",
    "iqamah": "05:00 AM",
    "name": "Fajr - الفجر"
  },
  {
    "id": "shurooq",
    "start": "06:03 AM",
    "iqamah": "06:03 AM",
    "name": "Shurooq - الشروق"
  },
  {
    "id": "duhr",
    "start": "01:04 PM",
    "iqamah": "01:30 PM",
    "name": "Duhr - الظهر"
  },
  {
    "id": "asr",
    "start": "05:48 PM",
    "iqamah": "06:15 PM",
    "name": "Asr - العصر"
  },
  {
    "id": "maghrib",
    "start": "08:00 PM",
    "iqamah": "08:00 PM",
    "name": "Maghrib - المغرب"
  },
  {
    "id": "isha",
    "start": "09:35 PM",
    "iqamah": "09:50 PM",
    "name": "Isha - العشاء"
  },
  {
    "id": "jumuah",
    "start": "01:30 PM",
    "iqamah": "01:30 PM",
    "name": "Jumuah - الجمعة"
  }
]
```

Once again, [full documentation of the prayerTimesFetch cloud function is available](./cloud-functions/prayer-times-fetch.md) with details on what parameters the API endpoint accepts and what the output looks like.
