---
name: Onboard a profile and connect a wearable
description: Authenticate as an administrator, create a WeFitter profile, connect it to a wearable/app provider, and confirm the connection.
api: openapi/wefitter-openapi-original.json
operations: [token_create, profile_create, connection_connect_create, profile_connections]
---

# Onboard a profile and connect a wearable

Use the WeFitter API (`https://api.wefitter.com/api/v1.3`) to onboard an end user and link their wearable so health data starts flowing.

## Auth
1. Exchange your client id / client secret (HTTP Basic) for an administrator JWT bearer token with **`token_create`** (`POST /token/`). Admin tokens are valid 24 hours. Send it as `Authorization: bearer <token>`.

## Steps
1. **Create the profile** — call **`profile_create`** (`POST /profile/`) with the administrator token. The response includes the new profile's `public_id` **and its own profile bearer token**. Store both — profile-specific actions use the profile bearer token, not the admin token.
2. **Start a wearable connection** — call **`connection_connect_create`** (`POST /connection/{connection_slug}/connect/`) with the provider's `connection_slug` (e.g. `fitbit`, `garmin`, `googlefit`, `oura`, `whoop`). Pass a `redirect` (and optional `redirect_on_error`) so the user returns to your app after the provider OAuth handshake. The response gives the URL to send the user to.
3. **Confirm** — after the user authorizes, call **`profile_connections`** (`GET /profile/{public_id}/connections/`) and verify the connection shows `connected: true`.

## Rules
- Admin vs profile tokens: listing/creating profiles needs the **admin** token; per-profile data reads accept the **profile** token. Using the wrong one returns 403 (see `errors/wefitter-problem-types.yml`).
- No idempotency-key contract exists; do not blindly retry `profile_create` — check `profile_list` first to avoid duplicates.
- Data arrives asynchronously via webhooks once the wearable syncs with its provider (see `asyncapi/wefitter-webhooks.yml`).
