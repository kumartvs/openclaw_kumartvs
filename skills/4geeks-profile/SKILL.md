---
name: 4geeks-profile
description: Get your 4Geeks student profile info, cohort details, and enrollment timeline.
---

# 4geeks-profile

Retrieve your 4Geeks Academy student profile — personal info, cohort name, start/end dates, and enrollment details.

## When to use
Run this when you need to check your cohort details, verify your enrollment information, or see your personal profile data like name, email, and academy location.

## How it works
Reads the student token from OpenClaw configuration and calls the 4Geeks API to fetch your profile and cohort information.

## Steps
1. Read the token from `auth.profiles.fourgeeks:default.apiKey` in `openclaw.json`. Do not accept it from source code.
2. Send a `GET` request to `https://api.breatheco.de/v1/me` (or equivalent user profile endpoint) with header `Authorization: token <token>`.
3. Send a `GET` request to `https://api.breatheco.de/v1/cohort` (or equivalent cohort endpoint) with header `Authorization: token <token>`.
4. Compile:
   - Profile: full name, email, profile picture URL (if any)
   - Cohort: cohort name, academy/bootcamp name, start date, end date, current stage
   - Enrollment status (active, graduated, dropped, etc.)

## Output
- Full name and email
- Cohort name + academy
- Started: date — Ends: date (or ended: date if past)
- Current status: active / graduated / other
- Days elapsed and days remaining in the cohort (if currently active)

## Constraints
- Token from config only, never hardcoded.
- Read-only — no writes.
- Profile data only. Use other 4geeks-* skills for assignments, progress, or submissions.