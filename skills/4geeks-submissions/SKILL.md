---
name: 4geeks-submissions
description: View your submitted but ungraded assignments, their submission dates, and expected grading timeline.
---

# 4geeks-submissions

Retrieve all assignments you have submitted to 4Geeks Academy that are awaiting grading, along with their submission dates and status.

## When to use
Run this when you want to check which of your submitted assignments haven't been graded yet, when you submitted them, and what's still awaiting review.

## How it works
Reads the student token from OpenClaw configuration and calls the 4Geeks API to find submitted deliverables. Uses the cohort/user endpoint for project completion status and the deliverables endpoint for submission details.

## Steps
1. Read the token from `auth.profiles.fourgeeks:default.apiKey` in `openclaw.json`. Do not accept it from source code.
2. First, get your cohort slug and academy ID from `/v1/admissions/user/me`.
3. Send a `GET` request to `https://breathecode.herokuapp.com/v1/admissions/academy/cohort/user?roles=STUDENT&cohorts=<cohort_slug>` with headers:
   - `Authorization: token <token>`
   - `Accept: application/json`
   - `Academy: <academy_id>`
4. Find your user record in the response.
5. Parse the `completion` object:
   - Check `overall.completed` and `overall.total`
   - `required.PROJECT.is_met` tells if the requirement is satisfied
   - The difference between `total` and `completed` gives pending count
6. For submission timestamps and details, check if `/v1/admissions/submission` or a deliverable endpoint is available with the user/cohort context.

## Output
- Project completion status (completed vs. required)
- Whether the requirement is currently met
- If the submissions endpoint is unavailable: show what the completion data says

## Constraints
- Token from config only, never hardcoded.
- Read-only — do not resubmit or modify anything.