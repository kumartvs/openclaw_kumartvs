# TOOLS.md

Use **already connected** Composio tools only. No new OAuth.

## Google Calendar (Composio)
- **When:** user asks for schedule, today, upcoming, or "pull calendar"
- **Default calendar:** primary
- **Read:** list events for the requested day (default: today, local time)
- **Write:** never unless the user confirms the exact event

## Google Docs (Composio)
- **When:** user asks to pull, read, or summarize a Doc
- **Default doc:** the user's primary working Google Doc (open the most recently used Doc if the title is not given)
- **Read:** fetch title + body and summarize
- **Write:** never unless the user confirms

## Telegram
- Already connected for chat. Use it to show Calendar/Docs pull results. Do not use Telegram as a new API.
