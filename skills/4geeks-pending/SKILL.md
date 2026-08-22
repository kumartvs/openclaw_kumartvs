---
name: 4geeks-pending
description: Identify specific tasks and deliverables still needing completion.
---

# 4geeks-pending

Identify the specific projects and deliverables you still need to complete — the items that are not yet done and need your attention.

## When to use
Run this when you want a focused list of what's outstanding: the exact project slugs and deliverables that haven't been submitted yet.

## How it works
Reads the student token from OpenClaw configuration and calls the cohort/user endpoint, then extracts the `completion.required.PROJECT.missing` array — which contains every project slug that is still pending.

## Steps
1. Read the token from `auth.profiles.fourgeeks:default.apiKey` in `openclaw.json`. Do not accept it from source code.
2. Determine your cohort slug and academy ID from the profile endpoint.
3. Send a `GET` request to `https://breathecode.herokuapp.com/v1/admissions/academy/cohort/user?roles=STUDENT&cohorts=<cohort_slug>` with headers:
   - `Authorization: token <token>`
   - `Accept: application/json`
   - `Academy: <academy_id>`
4. Find your user record and extract the `completion.required.PROJECT.missing` array.
5. Present each pending project slug clearly — these are the deliverables you haven't completed.

## Example API Response — missing array (from your record)
```json
[
    "ai-eng-milestone-web-fundamentals",
    "exercise-terminal-challenge",
    "first-collaborative-project-tailwind-css",
    "html-css-artist-landing-seo-access",
    "simple-dashboard-tailwind-css",
    "todo-list-cli-python",
    "typescript-cinema-seat-manager"
]
```
The full completion object also includes `pending_required_count: 7` and `pending_required_slugs.PROJECT` with the same array.

## Output
- Count of pending items
- List of pending project slugs/names
- "All caught up!" if nothing is pending

## Constraints
- Token from config only, never hardcoded.
- Read-only — do not submit or modify anything.
- Only pending (not completed) items.