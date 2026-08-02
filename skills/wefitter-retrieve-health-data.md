---
name: Retrieve a profile's health data
description: Read a WeFitter profile's aggregated, deduplicated health data — daily summaries, workouts, sleep, heart rate — and its AI biological-age insight.
api: openapi/wefitter-openapi-original.json
operations: [profile_daily_summary_list, profile_workout_list, profile_sleep_summary_list, profile_heartrate_summary_list, insights_bio_age_read]
---

# Retrieve a profile's health data

WeFitter aggregates and deduplicates health data across all of a profile's connected wearables into a unified model. Read it per profile from `https://api.wefitter.com/api/v1.3`.

## Auth
Use the administrator bearer token (or the profile's own bearer token for that profile's data). Send `Authorization: bearer <token>`.

## Steps
1. **Daily activity** — **`profile_daily_summary_list`** (`GET /profile/{profile_public_id}/daily_summary/`): steps, calories, distance, activity duration per day.
2. **Workouts** — **`profile_workout_list`** (`GET /profile/{profile_public_id}/workout/`): individual activities with type, duration, distance, calories, points.
3. **Sleep** — **`profile_sleep_summary_list`** (`GET /profile/{profile_public_id}/sleep_summary/`).
4. **Heart rate** — **`profile_heartrate_summary_list`** (`GET /profile/{profile_public_id}/heartrate_summary/`).
5. **Biological age** — **`insights_bio_age_read`** (`GET /insights/bio_age`) for the AI-derived bio-age insight.

## Conventions
- **Filtering**: pass `date_start`/`date_end` (or `start_date`/`end_date`, `date_range`) to bound the window, `source` to restrict to one provider, and `deduplicate` to control cross-wearable de-duplication (see `conventions/wefitter-conventions.yml`).
- **Pagination**: list endpoints use DRF pagination — `limit`, `offset`, `cursor`, `page_size`, `ordering`. Follow the cursor to page through large ranges.
- Data freshness depends on the upstream provider's sync (event-driven for Fitbit/Strava/Withings/Polar; interval-pull for Google Fit).
