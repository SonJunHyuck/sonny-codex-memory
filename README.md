# sonny-codex-memory

Personal context memory for continuing Codex work across devices, projects, and sessions.

## Purpose

This repository stores handoff summaries, project contexts, decisions, and next actions for work continued with Codex across multiple devices.

It is not a full chat log archive. Store durable context only.

## Workflow

At the start of a session:

```bash
git pull
```

Then ask Codex to load the relevant context.

At the end of a session, ask Codex to update the relevant context, create a session summary, commit, and push.

## Structure

```text
_index/
  active.md              # Active contexts and quick links

contexts/
  <project-or-area>/
    <topic>/
      current.md         # Latest working context
      decisions.md       # Durable decisions
      timeline.md        # Important milestones

sessions/
  YYYY/
    MM/
      YYYY-MM-DD-<context-id>-<device>.md

templates/
  current.md
  session.md
```

## Safety Rules

- Do not store secrets, API keys, tokens, passwords, or private credentials.
- Do not store raw chat transcripts by default.
- Summarize only the context needed to continue work.
- Prefer links and file paths over copied proprietary content.
- Keep project-sensitive details concise and intentional.
