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

## Example API Response (HTTP 200)
```json
{
    "id": 19739,
    "email": "vsktimmaraju@gmail.com",
    "first_name": "Venkata",
    "last_name": "Timmaraju",
    "github": { "username": "kumartvs" },
    "profile": { "avatar_url": "https://..." },
    "phone": "7816058186",
    "roles": [{ "role": "student", "academy": { "id": 4, "name": "4Geeks Miami" } }],
    "settings": { "lang": "en" },
    "cohorts": [{
        "cohort": {
            "slug": "miami-ai-engineering-2",
            "name": "miami-ai-engineering-2",
            "kickoff_date": "2026-05-12T00:00:00Z",
            "ending_date": "2026-10-24T00:00:00Z",
            "current_module": 25,
            "current_day": 43,
            "stage": "STARTED",
            "never_ends": false,
            "syllabus_version": { "name": "AI Engineer", "version": 2, "duration_in_days": 72 },
            "academy": { "name": "4Geeks Miami" },
            "micro_cohorts": [23 items covering Tailwind, Git, Python, Docker, etc.]
        }
    }]
}
```

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