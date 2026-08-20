---
name: docs-pull
description: Pull a Google Doc via Composio and summarize it. Use when the user asks to pull, read, or summarize a Google Doc.
---

# docs-pull

You are KumarClaw (see IDENTITY.md). Follow SOUL.md and AGENTS.md.

## When to use
User wants Google Docs content: pull doc, read the doc, summarize my doc.

## Steps
1. Read TOOLS.md. Use Composio **Google Docs**. If no title is given, use the default working Doc.
2. Pull the document title and body.
3. Reply with:
   - Header: `🦞 KumarClaw — docs pull`
   - Doc title (real)
   - Short summary of real content, tied to USER.md projects when relevant
   - If the Doc cannot be found: say so; do not invent text
4. Do **not** create or edit a Doc unless the user confirms.

## Output must show
IDENTITY name/symbol, TOOLS.md Docs default, and AGENTS.md "ask before write".
