---
name: 4geeks-submissions
description: View your submitted but ungraded assignments, their submission dates, and expected grading timeline.
---

# 4geeks-submissions

Retrieve all assignments you have submitted to 4Geeks Academy that are awaiting grading, along with their submission dates and status.

## When to use
Run this when you want to check which of your submitted assignments haven't been graded yet, when you submitted them, and what's still awaiting review.

## How it works
Reads the student token from OpenClaw configuration and calls the 4Geeks tasks API to find completed deliverables. Looks at tasks that are DONE vs PENDING to determine what's been submitted.

## Steps
1. Read the token from `auth.profiles.fourgeeks:default.apiKey` in `openclaw.json`. Do not accept it from source code.
2. Send a `GET` request to `https://breathecode.herokuapp.com/v1/assignment/user/me/task` with headers:
   - `Authorization: token <token>`
   - `Accept: application/json`
   - `Academy: <academy_id>`
3. Check the completion counts:
   - Completed (DONE) projects — these are submitted
   - Pending projects — still need submission
4. For submission details and grading status, try the deliverable endpoint at `/v1/assignment/task/<task_id>/deliver`.

## Example API Response
```json
{
    "projects": {
        "total": 77,
        "done": 56,
        "pending": 21
    },
    "all_tasks": {
        "total": 322,
        "done": 259,
        "pending": 63
    }
}
```

## Output
- Project completion: done / pending
- Overall task completion: done / pending
- Count of submitted (DONE) projects
- Count of pending (not yet submitted) projects
- "Nothing awaiting grading" if all done

## Constraints
- Token from config only, never hardcoded.
- Read-only — do not resubmit or modify anything.