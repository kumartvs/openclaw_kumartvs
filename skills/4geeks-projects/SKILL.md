---
name: 4geeks-projects
description: Retrieve assigned projects with their status (pending, submitted, or graded).
---

# 4geeks-projects

Retrieve all projects assigned to your 4Geeks Academy student account, listed with their current status.

## When to use
Run this to see what projects you've been assigned — which are still pending (PENDING), what's been completed (DONE), and what's graded.

## How it works
Reads the student token from OpenClaw configuration and calls the 4Geeks tasks API to get all your assignments with their status, type, and associated slugs.

## Steps
1. Read the token from `auth.profiles.fourgeeks:default.apiKey` in `openclaw.json`. Do not accept it from source code.
2. Send a `GET` request to `https://breathecode.herokuapp.com/v1/assignment/user/me/task` with headers:
   - `Authorization: token <token>`
   - `Accept: application/json`
   - `Academy: <academy_id>` (e.g. `4`)
3. Parse the JSON response array. Each task has:
   - `title` — Task/project name
   - `task_status` — `DONE`, `PENDING`, etc.
   - `task_type` — `PROJECT`, `EXERCISE`, `LESSON`
   - `associated_slug` — project slug
   - `created_at` — creation date
4. Group by `task_type` and `task_status`.

## Example API Response — one task object
```json
{
    "title": "Build Your IT Resume",
    "task_status": "PENDING",
    "task_type": "PROJECT",
    "associated_slug": "build-your-it-resume",
    "created_at": "2026-07-27T..."
}
```

## Output
- Total tasks (all types)
- Completed (DONE) vs Pending (PENDING) counts
- Projects breakdown: total projects, completed, pending
- List of pending projects with title and slug
- "All caught up!" if nothing is pending

## Constraints
- Token from config only, never hardcoded.
- Do not submit, create, or modify any task.
- This skill reads only — no writes.