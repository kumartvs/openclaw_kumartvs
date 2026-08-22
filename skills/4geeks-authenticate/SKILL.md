---
name: 4geeks-authenticate
description: Verify that the stored 4Geeks student token is valid and the session is active.
---

# 4geeks-authenticate

Verify your 4Geeks Academy API token is valid and your session is active.

## When to use
Run this first when you're unsure if authentication is working, or before calling any other 4Geeks skill.

## How it works
This skill reads the student token from OpenClaw configuration (`auth.profiles.fourgeeks:default.apiKey`) — never from source code — and calls the 4Geeks API to validate it.

## Steps
1. Read the token from `openclaw.json` at path `auth.profiles.fourgeeks:default.apiKey`. Do not accept it from any other source. Do not log or print the token value.
2. Send a `GET` request to `https://api.breatheco.de/v1/auth/me` (or equivalent validation endpoint) with header `Authorization: token <token>`.
3. Check the response:
   - If HTTP 200 with a valid user JSON: the token is active. Return the user name and email from the response.
   - If HTTP 401: the token is expired or invalid. Report this to the user.
   - If HTTP 404 or connection error: report the failure.

## Output
- Token status (valid / invalid / error)
- If valid: authenticated user name + email
- If invalid: message suggesting the token needs updating

## Constraints
- Never hardcode the token in this file or any script.
- Never print or log the token value.
- Do not call any other API endpoint — this skill is for authentication only.