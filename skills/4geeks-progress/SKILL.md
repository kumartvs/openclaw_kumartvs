---
name: 4geeks-progress
description: Overview of overall course progress — completed items, grades, and advancement.
---

# 4geeks-progress

Get a high-level overview of your progress in the 4Geeks Academy course: completed task counts per type (projects, exercises, lessons), current module and day, and overall completion ratio.

## When to use
Run this when you want the big picture — not just what's pending, but your overall standing: how many tasks completed out of total, grouped by type, plus your cohort timeline.

## How it works
Reads the student token from OpenClaw configuration and calls both the tasks API and profile API to aggregate completion data.

## Steps
1. Read the token from `auth.profiles.fourgeeks:default.apiKey` in `openclaw.json`. Do not accept it from source code.
2. Call `/v1/admissions/user/me` to get cohort context (current module, current day, date range).
3. Call `/v1/assignment/user/me/task` (with `Accept: application/json` and `Academy` headers) to get all tasks.
4. Count by status (`DONE` vs `PENDING`) and by type (`PROJECT`, `EXERCISE`, `LESSON`).
5. Present as a clean summary.

## Example Data (your account)
- **Tasks:** 259 DONE, 63 PENDING (322 total)
- **Projects:** 56 DONE, 21 PENDING (77 total)
- **Exercises:** 110 DONE, 38 PENDING (148 total)
- **Lessons:** 93 DONE, 4 PENDING (97 total)
- **Cohort:** miami-ai-engineering-2, module 25, day 43

## Output
- Total tasks: done / pending
- Project breakdown: X of Y done
- Exercise breakdown: X of Y done
- Lesson breakdown: X of Y done
- Overall completion percentage
- Current module and day

## Constraints
- Token from config only, never hardcoded.
- Read-only — no modifications.
- This is a summary view. Use 4geeks-projects for per-project details and 4geeks-pending for outstanding work.