---
name: 4geeks-submissions
description: View your submitted but ungraded assignments, their submission dates, and expected grading timeline.
---

# 4geeks-submissions

Retrieve all assignments you have submitted to 4Geeks Academy that are awaiting grading, along with their submission dates and expected turnaround.

## When to use
Run this when you want to check which of your submitted assignments haven't been graded yet, when you submitted them, and what's still in the queue for review.

## How it works
Reads the student token from OpenClaw configuration and calls the 4Geeks API to find submitted-but-not-yet-graded submissions.

## Steps
1. Read the token from `auth.profiles.fourgeeks:default.apiKey` in `openclaw.json`. Do not accept it from source code.
2. Send a `GET` request to `https://api.breatheco.de/v1/submissions` (or equivalent submissions endpoint) with header `Authorization: token <token>`.
3. Filter to submissions with status `submitted` (or equivalent status indicating awaiting grading).
4. For each:
   - Project/exercise title
   - Submission date and time
   - Expected grading turnaround (if available from the API)
5. Sort by submission date (oldest first — longest wait at top).

## Output
- Submitted items awaiting grading (title + submission date)
- Count of items in queue
- Oldest waiting submission (date)
- Confidence level: "Tracked" vs "Estimated" if turnaround data is unavailable

## Constraints
- Token from config only, never hardcoded.
- Read-only — do not resubmit or modify anything.
- Only submitted/un-graded items. Pending items go in 4geeks-pending; graded items show in 4geeks-projects or 4geeks-progress.