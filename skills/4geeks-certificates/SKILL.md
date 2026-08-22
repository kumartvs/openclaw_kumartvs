---
name: 4geeks-certificates
description: View your earned certificates, their status, preview URLs, and issuing instructors.
---

# 4geeks-certificates

Retrieve all certificates you have earned at 4Geeks Academy, including specialty name, issuing instructor, status, and preview PDF URL.

## When to use
Run this when you want to check your earned certificates, see if any are pending generation, or access certificate preview/download links.

## How it works
Reads the student token from OpenClaw configuration and calls the 4Geeks certificates API to fetch your certificate list.

## Steps
1. Read the token from `auth.profiles.fourgeeks:default.apiKey` in `openclaw.json`. Do not accept it from source code.
2. Send a `GET` request to `https://breathecode.herokuapp.com/v1/certificate/me` with headers:
   - `Authorization: token <token>`
   - `Accept: application/json`
3. Parse the JSON response array. Each certificate has:
   - `specialty.name` — Certificate/specialty name
   - `status` — e.g. `PERSISTED` (queued for PDF generation)
   - `status_text` — Human-readable status
   - `signed_by` — Instructor name
   - `signed_by_role` — Instructor role (e.g. "Main Instructor")
   - `preview_url` — Google Cloud Storage URL to view/download PDF
   - `academy.name` — Academy name
   - `cohort.name` — Cohort name

## Example API Response — one certificate
```json
{
    "specialty": { "name": "Backend Development with Coding Agents Miami" },
    "status": "PERSISTED",
    "status_text": "Certificate successfully queued for PDF generation",
    "signed_by": "Ryan Castanier",
    "signed_by_role": "Main Instructor",
    "preview_url": "https://storage.googleapis.com/certificates-breathecode/...",
    "academy": { "name": "4Geeks Miami" },
    "cohort": { "name": "Backend Development with Coding Agents Miami" }
}
```

## Output
- Total certificates count
- For each: specialty name, instructor, status, preview URL
- If none: "No certificates found"

## Constraints
- Token from config only, never hardcoded.
- Read-only — no modifications.
- Certificates data only. Use other 4geeks-* skills for assignments, progress, or submissions.

## Postman Test

| Field | Value |
|-------|-------|
| **Method** | `GET` |
| **URL** | `{{base_url}}/v1/certificate/me` |
| **Headers** | `Authorization: token {{token}}` |
| | `Accept: application/json` |