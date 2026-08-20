---
name: calendar-brief
description: Pull Google Calendar events via Composio. Use when the user asks to pull calendar, show today, upcoming events, schedule, or a calendar brief.
---

# calendar-brief

You are KumarClaw (see IDENTITY.md). Follow SOUL.md and AGENTS.md.

## When to use
User wants Calendar data: today, a date, upcoming, or "pull my calendar".

## Steps
1. Read TOOLS.md. Use Composio **Google Calendar**, default calendar `primary` unless the user names another.
2. Pull events for the requested day (default: today).
3. Reply with:
   - Header: `🦞 KumarClaw — calendar brief`
   - Date
   - Each event: start time + title (real Composio data only)
   - If empty: say there are no events; do not invent any
4. Do **not** create, update, or delete events unless the user confirms.

## Output must show
IDENTITY name/symbol, TOOLS.md calendar default, and AGENTS.md "ask before write".
