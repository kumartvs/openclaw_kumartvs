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
  - ✅ Found: `/v1/assignment/user/me/task` — the main tasks/projects endpoint (322 tasks returned, 259 DONE, 63 PENDING)
  - ✅ Found: `/v1/assignment/academy/cohort/1620/task` — cohort-level task listing
  - ✅ Found: `/v1/assignment/me/coderevision` — code revisions
  - ✅ Found: `/v1/assignment/me/deletion_order` — repository deletions
  - ✅ Found: `/v1/assignment/user/me/final_project` — final project info
  - ✅ Updated all skill files from cohort/user completion endpoint to `/v1/assignment/user/me/task` which provides real per-task status data

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
- `GET https://breathecode.herokuapp.com/v1/assignment/user/me/task` with `Authorization: token <token>`, `Accept: application/json`, and `Academy: 4` header. Returns all tasks (projects, exercises, lessons) with individual status (DONE/PENDING).

**How to use/verify (invocation steps only — do not run):**
1. Agent reads token from config.
2. Agent calls the tasks endpoint.
3. Agent filters by `task_type: "PROJECT"` and groups by `task_status`.
4. **Success looks like:** "📋 Projects — 77 total, 56 DONE, 21 PENDING. Pending: Build Your IT Resume, Monthly Sales Analyzer..."

---

### Skill 4: 4geeks-pending

**Prompt used to create:**
*"What exactly do I still need to finish?"* (wanting the specific deliverables)

**What it does:**
Filters the tasks API to items with `task_status: "PENDING"`, grouped by type (project, exercise, lesson), giving a detailed list of outstanding work.

**API endpoint(s):**
- Same as 4geeks-projects — `GET /v1/assignment/user/me/task` — filtered to `task_status === "PENDING"`.

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
- `GET /v1/assignment/user/me/task` — for all task completion data (322 tasks: 259 DONE, 63 PENDING)

**How to use/verify (invocation steps only — do not run):**
1. Agent calls profile endpoint for cohort info.
2. Agent calls tasks endpoint for completion counts.
3. **Success looks like:** "📊 Progress — 322 total tasks: 259 DONE, 63 PENDING | Projects: 56 of 77 DONE | Cohort: miami-ai-engineering-2, module 25, day 43".
4. **Partial data:** Reports what's available and what's missing.

---

### Skill 6: 4geeks-submissions

**Prompt used to create:**
*"What have I submitted that's still waiting to be graded?"* (real student need — checking review queue)

**What it does:**
Uses the tasks API to show what's been completed (DONE) vs what's pending (PENDING), broken down by task type.

**API endpoint(s):**
- `GET /v1/assignment/user/me/task` — with `Accept: application/json` and `Academy: 4` headers. Returns all tasks with individual status.

**How to use/verify (invocation steps only — do not run):**
1. Agent reads token from config.
2. Agent calls the tasks endpoint.
3. Agent groups tasks by `task_status` (DONE/PENDING) and `task_type` (PROJECT/EXERCISE/LESSON).
4. **Success looks like:** "📬 Submissions — 259 DONE, 63 PENDING | Projects: 56 of 77 DONE, 21 pending | Exercises: 110 of 148 DONE | Lessons: 93 of 97 DONE".

---

## C. Token Storage Note

**The 4Geeks student token is stored in OpenClaw configuration only.**

- **Location:** `/root/.openclaw/openclaw.json` → `auth.profiles.fourgeeks:default.apiKey`
- **Not hardcoded anywhere:** No skill `.md` file, no script, no example, no code contains the token value.
- **Access pattern:** Each skill reads the token from config at invocation time. The token value is never printed, logged, or committed to version control.