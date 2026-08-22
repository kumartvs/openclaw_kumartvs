---
name: 4geeks-pending
description: Identify specific tasks and deliverables still needing completion.
---

# 4geeks-pending

Identify the specific projects, exercises, and lessons you still need to complete — with their titles, types, and deadlines.

## When to use
Run this when you want a focused list of what's outstanding: the exact tasks that are still PENDING and need your attention.

## How it works
Reads the student token from OpenClaw configuration and calls the 4Geeks tasks API, then filters to items with `task_status: "PENDING"`.

## Steps
1. Read the token from `auth.profiles.fourgeeks:default.apiKey` in `openclaw.json`. Do not accept it from source code.
2. Send a `GET` request to `https://breathecode.herokuapp.com/v1/assignment/user/me/task` with headers:
   - `Authorization: token <token>`
   - `Accept: application/json`
   - `Academy: <academy_id>`
3. Filter results to items where `task_status === "PENDING"`.
4. Group by `task_type` and present each with title, slug, and creation date.

## Example API Response — filtered pending items
```json
{
    "projects_pending": [
        { "title": "Build Your IT Resume", "associated_slug": "build-your-it-resume" },
        { "title": "Monthly Sales Analyzer", "associated_slug": "monthly-sales-analyzer" }
    ],
    "exercises_pending": 38,
    "lessons_pending": 4
}
```

## Output
- Total pending items count
- Grouped by type: pending projects, exercises, lessons
- List of pending project titles and slugs
- "All caught up!" if nothing is pending

## Constraints
- Token from config only, never hardcoded.
- Read-only — do not submit or modify anything.
- Only PENDING status items.