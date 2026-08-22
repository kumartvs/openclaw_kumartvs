---
name: 4geeks-pending
description: Identify specific tasks and deliverables still needing completion.
---

# 4geeks-pending

Identify the specific tasks and deliverables you still need to complete across all your 4Geeks Academy assignments.

## When to use
Run this when you want a focused list of what's outstanding — the work items that are not yet done and need your attention.

## How it works
Reads the student token from OpenClaw configuration and calls the 4Geeks API to find all assignments with a pending status, then extracts the specific tasks or deliverables required for each.

## Steps
1. Read the token from `auth.profiles.fourgeeks:default.apiKey` in `openclaw.json`. Do not accept it from source code.
2. Send a `GET` request to `https://api.breatheco.de/v1/assignment` (or equivalent) with header `Authorization: token <token>`.
3. Filter results to items whose status is `pending`.
4. For each pending assignment, extract:
   - Project/exercise title
   - Description of what needs to be done
   - Any listed deliverables or requirements
   - Due date (if available)
5. Present them in priority order (earliest deadline first).

## Output
- List of pending items with title + required deliverables + due date
- "All caught up!" if nothing is pending

## Constraints
- Token from config only, never hardcoded.
- Read-only — do not submit or modify anything.
- Only pending-status items. Submitted/graded items go in 4geeks-projects or 4geeks-progress.