# SKILL_LOG.md

## A. Discovery Conversation

**Ven's request:**
*"I want to give you the ability to connect to my 4Geeks account using my student token, without me having to write any code. What do we need to do?"*

**What I suggested:**
The setup was mostly done — the student token was already stored in OpenClaw configuration under `auth.profiles.fourgeeks:default.apiKey`. The next step was verifying the token works and discovering the correct API endpoints by probing the BreatheCode API.

**What I asked for and how I resolved it:**
- **API base URL:** Found `https://breathecode.herokuapp.com` after trying several domains. The production `api.breatheco.de` was unreachable from the VPS.
  - ✅ Found: `/v1/auth/user/me` — token validation + user profile
  - ✅ Found: `/v1/admissions/user/me` — full profile with cohort details and enrollment
  - ✅ Found: `/v1/admissions/academy` — list of academies
  - ✅ Found: `/v1/certificate/me` — certificate list
  - ✅ Found: `/v1/admissions/academy/cohort/user?roles=STUDENT&cohorts=<slug>` — cohort member data with completion tracking, pending projects, and progress
  - ❌ Not found: submissions/deliverables endpoint — `/v1/admissions/deliverable`, `/v1/admissions/submission`, `/v1/deliverable` all return 404. The submission service at `4geeks-project-submission.learn-pack.com` returns AWS AccessDenied without frontend session auth. The completion/project data is available through the cohort/user endpoint instead (pending_required_count, missing slugs)

- **Auth method:** All calls require `Authorization: token <token>` header plus `Accept: application/json`. Some endpoints require `Academy: <id>` header.

- **User data collected:** Venkata Timmaraju, student at 4Geeks Miami, cohort miami-ai-engineering-2, days 0–72, currently on day 43 of the AI Engineer syllabus.

---

## B. Skill Definitions

### Skill 1: 4geeks-authenticate

**Prompt used to create:**
*"I want to give you the ability to connect to my 4Geeks account using my student token, without me having to write any code. What do we need to do?"*

**What it does:**
Verifies the 4Geeks API token stored in OpenClaw config is valid by calling the authentication endpoint. Returns the user's name, email, and roles if valid.

**API endpoint(s):**
- `GET https://breathecode.herokuapp.com/v1/auth/user/me` with `Authorization: token <token>` and `Accept: application/json`

**How to use/verify (invocation steps only — do not run):**
1. Agent reads the token from `auth.profiles.fourgeeks:default.apiKey` in config.
2. Agent calls the auth endpoint with the token.
3. **Success looks like:** HTTP 200 with JSON containing `first_name`, `last_name`, `email`, `roles`. Agent reports "✅ Token is valid! Name: Venkata Timmaraju, Email: vsktimmaraju@gmail.com, Role: student at 4Geeks Miami".
4. **Failure looks like:** HTTP 403 with `"Expired or invalid token"` — agent reports the token needs updating.

---

### Skill 2: 4geeks-profile

**Prompt used to create:**
*"What's my profile and cohort info?"* (implied by wanting to see account details after auth)

**What it does:**
Retrieves the student's full profile (name, email, GitHub, phone, avatar) and cohort enrollment details (cohort name, academy, kickoff/end dates, current module, current day, syllabus name).

**API endpoint(s):**
- `GET https://breathecode.herokuapp.com/v1/admissions/user/me` with `Authorization: token <token>` and `Accept: application/json`

**How to use/verify (invocation steps only — do not run):**
1. Agent reads token from config.
2. Agent calls the profile endpoint.
3. **Success looks like:** "👤 Venkata Timmaraju | vsktimmaraju@gmail.com | GitHub: kumartvs | Cohort: miami-ai-engineering-2 at 4Geeks Miami | Started: 2026-05-12 — Ends: 2026-10-24 | Syllabus: AI Engineer v2 | Current: module 25, day 43 of 72".
4. **Failure looks like:** Token expired or API unreachable.

---

### Skill 3: 4geeks-projects

**Prompt used to create:**
*"What specific projects or assignments are pending for me?"* (natural next question after seeing profile)

**What it does:**
Lists all assigned projects with their completion status — total required, completed count, percentage done, and which specific project slugs are still pending.

**API endpoint(s):**
- `GET https://breathecode.herokuapp.com/v1/admissions/academy/cohort/user?roles=STUDENT&cohorts=<cohort_slug>` with `Authorization: token <token>`, `Accept: application/json`, and `Academy: <academy_id>` header. Finds the user's record by ID, extracts `completion.required.PROJECT`.

**How to use/verify (invocation steps only — do not run):**
1. Agent gets cohort slug from profile endpoint.
2. Agent calls cohort/user endpoint with Academy header.
3. Agent finds the user record (id 19739) and parses `completion`.
4. **Success looks like:** "📋 Projects — 7 total required, 0 completed (0%). Requirement met? ❌ No. Pending projects:\n   - ai-eng-milestone-web-fundamentals\n   - exercise-terminal-challenge\n   - first-collaborative-project-tailwind-css\n   - html-css-artist-landing-seo-access\n   - simple-dashboard-tailwind-css\n   - todo-list-cli-python\n   - typescript-cinema-seat-manager"

---

### Skill 4: 4geeks-pending

**Prompt used to create:**
*"What exactly do I still need to finish?"* (wanting the specific deliverables)

**What it does:**
Extracts only the pending (not completed) project slugs from the completion data, giving a clean focused list of what still needs to be done.

**API endpoint(s):**
- Same as 4geeks-projects — `GET /v1/admissions/academy/cohort/user` — but filtered to only the `completion.required.PROJECT.missing` array.

**How to use/verify (invocation steps only — do not run):**
1. Agent gets cohort slug and academy ID from profile.
2. Agent calls the cohort/user endpoint.
3. Agent extracts the `missing` array from the completion data.
4. **Success looks like:** "⚠️ 7 pending items:\n1. ai-eng-milestone-web-fundamentals\n2. exercise-terminal-challenge\n3. first-collaborative-project-tailwind-css\n4. html-css-artist-landing-seo-access\n5. simple-dashboard-tailwind-css\n6. todo-list-cli-python\n7. typescript-cinema-seat-manager".
5. **Empty looks like:** "All caught up! No pending items."

---

### Skill 5: 4geeks-progress

**Prompt used to create:**
*"How am I doing overall in the course?"* (wanting a big-picture summary)

**What it does:**
Aggregates profile and completion data to present a high-level progress summary — project completion (completed/total, percentage), requirement status (met/not met), current module, current day, and cohort timeline.

**API endpoint(s):**
- `GET /v1/admissions/user/me` — for cohort context (module, day, dates)
- `GET /v1/admissions/academy/cohort/user` — for completion data (overall + required)

**How to use/verify (invocation steps only — do not run):**
1. Agent calls both endpoints with the token.
2. Agent compiles the overview from both responses.
3. **Success looks like:** "📊 Progress — 0 of 7 projects completed (0%) | Requirement: not met | Current: module 25, day 43 | Cohort: May 12 → Oct 24, 2026".
4. **Partial data:** Reports what's available and what's missing.

---

### Skill 6: 4geeks-submissions

**Prompt used to create:**
*"What have I submitted that's still waiting to be graded?"* (real student need — checking review queue)

**What it does:**
Checks the completion data to show what's been completed vs. what's pending. Attempts to find submission timestamps from the available API endpoints.

**API endpoint(s):**
- `GET /v1/admissions/academy/cohort/user?roles=STUDENT&cohorts=<cohort_slug>` (with Academy header) — primary endpoint
- Attempts to access deliverable/submission-specific endpoints if available

**Note:** The dedicated submissions/deliverables endpoint was not found through blind API probing. The following endpoints all returned 404:
  - `/v1/admissions/deliverable`
  - `/v1/admissions/submission`
  - `/v1/deliverable`
  - `/v1/submission`
  - `/v1/admissions/user/19739/submissions`
  - `/v1/admissions/user/19739/deliverables`

The submission service at `4geeks-project-submission.learn-pack.com` returns AWS AccessDenied without frontend session context.

Without these endpoints, the skill uses the completion data from the cohort/user endpoint instead. The completion data shows project counts and pending slugs but lacks submission timestamps and grading status.

**How to use/verify (invocation steps only — do not run):**
1. Agent gets cohort slug and academy ID from profile.
2. Agent calls the cohort/user endpoint.
3. **Success looks like:** "📬 Submissions: 0 submitted, 7 pending. No items awaiting grading."
4. **If items are completed:** shows the completed project slugs and their status.

---

## C. Token Storage Note

**The 4Geeks student token is stored in OpenClaw configuration only.**

- **Location:** `/root/.openclaw/openclaw.json` → `auth.profiles.fourgeeks:default.apiKey`
- **Not hardcoded anywhere:** No skill `.md` file, no script, no example, no code contains the token value.
- **Access pattern:** Each skill reads the token from config at invocation time. The token value is never printed, logged, or committed to version control.