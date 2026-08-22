---
name: 4geeks-progress
description: Overview of overall course progress — completed items, grades, and advancement.
---

# 4geeks-progress

Get a high-level overview of your progress in the 4Geeks Academy course: completed project counts, current module, day, and overall completion percentage.

## When to use
Run this when you want the big picture — not just what's pending, but your overall standing including completed vs. total requirements and where you are in the syllabus timeline.

## How it works
Reads the student token from OpenClaw configuration and calls both the profile and cohort/user endpoints to aggregate completion data.

## Steps
1. Read the token from `auth.profiles.fourgeeks:default.apiKey` in `openclaw.json`. Do not accept it from source code.
2. Call `/v1/admissions/user/me` to get cohort info (slug, academy ID, current module, current day).
3. Call `/v1/admissions/academy/cohort/user?roles=STUDENT&cohorts=<cohort_slug>` (with Academy header) to get completion data for your user record.
4. Compile the overview:
   - **Project completion:** `overall.completed` of `overall.total` done (`overall.percent`%)
   - **Requirement met?** `required.PROJECT.is_met`
   - **Course progress:** current module and current day
   - **Cohort timeline:** start date to end date
5. Present as a clean summary with a text-based progress indicator.

## Example Data (from your account)

**Profile endpoint** (`/v1/admissions/user/me`) returns:
- Cohort: miami-ai-engineering-2
- Kickoff: 2026-05-12 → Ending: 2026-10-24
- Syllabus: AI Engineer v2, 72 days total
- Current module: 25, Current day: 43

**Cohort/user endpoint** returns completion:
```json
{
    "overall": { "total": 7, "completed": 0, "percent": 0.0 },
    "required": {
        "PROJECT": {
            "total": 7,
            "completed": 0,
            "percent": 0.0,
            "min_percent": 100.0,
            "is_met": false
        }
    },
    "pending_required_count": 7,
    "is_complete": false
}
```

## Output
- X of Y projects completed (Z%)
- Requirement status: met / not met
- Current module + current day
- Cohort date range
- Overall completion percentage

## Constraints
- Token from config only, never hardcoded.
- Read-only — no modifications.
- This is a summary view. Use 4geeks-projects for per-project details and 4geeks-pending for outstanding work.