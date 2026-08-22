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
2. Send a `GET` request to `https://breathecode.herokuapp.com/v1/admissions/user/me` with headers:
   - `Authorization: token <token>`
   - `Accept: application/json`
3. Parse the JSON response and extract:
   - Profile: full name, email, avatar URL, GitHub username, phone
   - Cohort: cohort name, academy, start date (kickoff), end date, current module, current day, stage
   - Syllabus: syllabus name, version, total duration
   - Enrollment status: financial status, educational status

## Output
- Full name and email
- Cohort name + academy
- Started: date — Ends: date
- Syllabus name + version
- Current module + current day
- Status: active / graduated / other
- GitHub: username (if linked)

## Constraints
- Token from config only, never hardcoded.
- Read-only — no writes.
- Profile data only. Use other 4geeks-* skills for assignments, progress, or submissions.