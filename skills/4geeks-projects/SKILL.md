---
name: 4geeks-projects
description: Retrieve assigned projects with their status (pending, submitted, or graded).
---

# 4geeks-projects

Retrieve all projects assigned to your 4Geeks Academy student account, categorized by their current status.

## When to use
Run this to see what projects you've been assigned and their current state — whether they're still pending, have been submitted, or have been graded.

## How it works
Reads the student token from OpenClaw configuration and calls the 4Geeks API to list your assigned projects with their status.

## Steps
1. Read the token from `auth.profiles.fourgeeks:default.apiKey` in `openclaw.json`. Do not accept it from source code.
2. Send a `GET` request to `https://api.breatheco.de/v1/assignment` (or equivalent projects endpoint) with header `Authorization: token <token>`.
3. Parse the response. Group projects by status:
   - **Pending** — not yet submitted
   - **Submitted** — awaiting grading
   - **Graded** — marked with a score
4. Return the grouped list with project title, status, and (if graded) score.

## Output
- Pending projects (title + deadline if available)
- Submitted projects awaiting grading (title + submission date)
- Graded projects (title + score/grade)
- Summary counts (X pending, Y submitted, Z graded)

## Constraints
- Token from config only, never hardcoded.
- Do not submit, create, or modify any project.
- This skill reads only — no writes.