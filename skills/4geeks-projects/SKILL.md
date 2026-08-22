---
name: 4geeks-projects
description: Retrieve assigned projects with their status (pending, submitted, or graded).
---

# 4geeks-projects

Retrieve all projects assigned to your 4Geeks Academy student account, listed with their current status.

## When to use
Run this to see what projects you've been assigned — which are still pending, what's been submitted, and what's been graded.

## How it works
Reads the student token from OpenClaw configuration and calls the cohort/user endpoint to get your project list with completion tracking. The API provides a complete list of required projects, their status (completed vs. pending), and completion percentages.

## Steps
1. Read the token from `auth.profiles.fourgeeks:default.apiKey` in `openclaw.json`. Do not accept it from source code.
2. Determine your cohort slug from the profile endpoint (`/v1/admissions/user/me` → `cohorts[0].cohort.slug`).
3. Send a `GET` request to `https://breathecode.herokuapp.com/v1/admissions/academy/cohort/user` with headers:
   - `Authorization: token <token>`
   - `Accept: application/json`
   - `Academy: <academy_id>` (get from profile)
   
   Query params: `roles=STUDENT&cohorts=<cohort_slug>`
4. Find your user record in the response array.
5. Parse the `completion` object:
   - `completion.overall.total` — total required projects
   - `completion.overall.completed` — completed count
   - `completion.required.PROJECT.missing` — array of pending project slugs
   - `completion.required.PROJECT.min_percent` — passing threshold
   - `completion.required.PROJECT.is_met` — whether requirement is met

## Example API Response — completion object (from your record)
```json
{
    "overall": { "total": 7, "completed": 0, "percent": 0.0 },
    "required": {
        "PROJECT": {
            "total": 7,
            "completed": 0,
            "percent": 0.0,
            "min_percent": 100.0,
            "is_met": false,
            "only_mandatory": true,
            "missing": [
                "ai-eng-milestone-web-fundamentals",
                "exercise-terminal-challenge",
                "first-collaborative-project-tailwind-css",
                "html-css-artist-landing-seo-access",
                "simple-dashboard-tailwind-css",
                "todo-list-cli-python",
                "typescript-cinema-seat-manager"
            ]
        }
    },
    "pending_required_count": 7
}
```

## Output
- Total required projects (count)
- Completed projects (count + percentage)
- Pending projects (list of project names/slugs)
- Whether the project requirement is currently met
- Summary: "X of Y projects completed (Z%)"

## Constraints
- Token from config only, never hardcoded.
- Do not submit, create, or modify any project.
- This skill reads only — no writes.