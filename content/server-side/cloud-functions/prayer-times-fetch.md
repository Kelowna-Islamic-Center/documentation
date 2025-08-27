# prayerTimesFetch

**Type:** HTTP Function (onRequest)

**Endpoint:** [`prayertimesfetch-ilgk6gl75q-uc.a.run.app/`](https://prayertimesfetch-ilgk6gl75q-uc.a.run.app/)

---

## Description

Provides **daily prayer times** formatted for the app.  
Supports fetching **today's** or **tomorrow's** prayer times and handles Asr time according to the selected madhhab.

---

## Query Parameters

| Name     | Required | Values     | Description |
|----------|----------|------------|-------------|
| `day`    | No       | `today` (default), `tomorrow` | Selects whether to fetch today's or tomorrow's times. |
| `method` | No       | `hanafi`, `hanbali` | If `"hanbali"` is provided, Asr time is fetched from IslamicFinder API. If nothing provided, `hanafi` is automatically assumed. |

---

## Environment Variables

- `API_LINK` – Base URL for BCMA prayer times
- `ISLAMIC_FINDER_API_LINK` – Base URL for IslamicFinder API

---

## Response Format

An array of objects:

| Field  | Type   | Description |
|--------|--------|-------------|
| `id`   | string | Prayer ID (e.g. `fajr`, `isha`) |
| `start`| string | Athan time (HH:mm A) |
| `iqamah`| string| Iqamah time (HH:mm A) |
| `name` | string | English + Arabic prayer name |

---

## Example HTTP Response

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
