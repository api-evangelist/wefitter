---
name: Create and run a gamification challenge
description: Create a WeFitter challenge, enroll profiles, read the leaderboard, and notify participants.
api: openapi/wefitter-openapi-original.json
operations: [challenge_create, challenge_member_create, challenge_leaderboard, notification_send_to_profiles_create]
---

# Create and run a gamification challenge

WeFitter's gamification layer turns wearable data into challenges, leaderboards and points. Base URL `https://api.wefitter.com/api/v1.3`.

## Auth
Use the administrator bearer token (`Authorization: bearer <token>`). Creating and managing challenges is an admin action.

## Steps
1. **Create the challenge** — **`challenge_create`** (`POST /challenge/`) with title, description, `start`/`end`, `type` (e.g. goal, streak, goalbreaker), `goal`/`goal_value`/`goal_type`, `calculation_method`, and `enrollment_method`. The response returns the challenge `public_id`.
2. **Enroll members** — **`challenge_member_create`** (`POST /challenge/{public_id}/member/`) for a single profile (or use `challenge_members_create` for a batch). For team challenges, teams are added via the challenge's team surface.
3. **Read the leaderboard** — **`challenge_leaderboard`** (`GET /challenge/{public_id}/leaderboard/`) for ranked positions and scores; leaderboard results are paginated. Use `challenge_leaderboard_history` for historical standings.
4. **Notify participants** — **`notification_send_to_profiles_create`** (`POST /notification/send_to_profiles/`) to push an app notification to the enrolled profiles.

## Rules
- Scores derive from the profiles' aggregated wearable data — participants must have a connected wearable (see the onboarding skill).
- `max score per day` and `goal progress` are supported on leaderboards/members.
- Errors follow DRF JSON conventions (`errors/wefitter-problem-types.yml`); a 403 usually means a profile token was used for an admin-only action.
