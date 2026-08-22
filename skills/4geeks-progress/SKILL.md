---
name: 4geeks-progress
description: Overview of overall course progress — completed items, grades, and advancement.
---

# 4geeks-progress

Get a high-level overview of your progress in the 4Geeks Academy course: what you've completed, your current grades, and how far along you are.

## When to use
Run this when you want the big picture — not just what's pending, but a summary of your overall standing including completions, scores, and advancement toward course milestones.

## How it works
Reads the student token from OpenClaw configuration and calls the 4Geeks API to aggregate your course progress data.

## Steps
1. Read the token from `auth.profiles.fourgeeks:default.apiKey` in `openclaw.json`. Do not accept it from source code.
2. Send a `GET` request to `https://api.breatheco.de/v1/assignment` and `https://api.breatheco.de/v1/cohort/me` (or equivalent progress/status endpoints) with header `Authorization: token <token>`.
3. Compile the overview:
   - Total assignments completed vs total assigned (X of Y done)
   - Average grade across all graded items
   - Current phase/module of the course
   - Overall completion percentage
4. Present as a clean summary.

## Output
- X of Y assignments completed
- Average grade (if graded items exist)
- Current course phase/module
- Overall completion percentage
- Progress bar or visual indicator (plain text)

## Constraints
- Token from config only, never hardcoded.
- Read-only — no modifications.
- This is a summary view. Use 4geeks-projects for per-project details and 4geeks-pending for outstanding work.