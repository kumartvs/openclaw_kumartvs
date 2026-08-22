---
name: 4geeks-authenticate
description: Verify that the stored 4Geeks student token is valid and the session is active.
---

# 4geeks-authenticate

Verify your 4Geeks Academy API token is valid and your session is active.

## When to use
Run this first when you're unsure if authentication is working, or before calling any other 4Geeks skill.

## How it works
Reads the student token from OpenClaw configuration (`auth.profiles.fourgeeks:default.apiKey`) and calls the 4Geeks API to validate it.

## Steps
1. Read the token from `auth.profiles.fourgeeks:default.apiKey` in `openclaw.json`. Do not accept it from any other source. Do not log or print the token value.
2. Send a `GET` request to `https://breathecode.herokuapp.com/v1/auth/user/me` with headers:
   - `Authorization: token <token>`
   - `Accept: application/json`
3. Check the response:
   - If HTTP 200 with a valid user JSON: the token is active. Return the user name and email from the response.
   - If HTTP 403 with `"Expired or invalid token"`: the token is expired or invalid. Report this to the user.
   - If HTTP 404 or connection error: report the failure.

## Example API Response (HTTP 200)
```json
{
    "id": 19739,
    "email": "vsktimmaraju@gmail.com",
    "first_name": "Venkata",
    "last_name": "Timmaraju",
    "github": { "username": "kumartvs" },
    "roles": [
        { "role": "student", "academy": { "name": "4Geeks Miami", "id": 4 } },
        { "role": "student", "academy": { "name": "4Geeks.com", "id": 47 } }
    ]
}
```

## Output
- ✅ Token status: valid
- Full name and email of authenticated user
- List of roles and academies (e.g. "student at 4Geeks Miami")
- If invalid: message suggesting the token needs updating

## Constraints
- Never hardcode the token in this file or any script.
- Never print or log the token value.
- Do not call any other API endpoint — this skill is for authentication only.