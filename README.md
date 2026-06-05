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
  repositories.md        # Stable repo identities and remotes
  local-paths.md         # Device-specific checkout path hints

contexts/
  <project-or-area>/
    <topic>/
      current.md         # Latest working context
      decisions.md       # Durable decisions
      timeline.md        # Important milestones
      notion.md          # Seeds for later Notion documentation

sessions/
  YYYY/
    MM/
      YYYY-MM-DD-<context-id>-<device>.md

templates/
  current.md
  decisions.md
  timeline.md
  session.md
```

## Context Model

Contexts are organized by project and topic:

```text
contexts/
  bfx/
    eventlog/
      current.md
      decisions.md
      timeline.md
      notion.md
```

Use `current.md`, `decisions.md`, and `timeline.md` for day-to-day continuity. Use `sessions/` as lightweight source notes when a work session creates decisions, useful rationale, or future documentation material. Use `notion.md` as a staging area for Notion pages about what was built, why it exists, and how it works.

Do not treat one machine's absolute filesystem path as canonical. PC and Mac checkouts can live in different directories. Prefer stable identifiers such as `context_id`, `repo_id`, and Git remotes; keep local paths in `_index/local-paths.md` as hints only.

## Safety Rules

- Do not store secrets, API keys, tokens, passwords, or private credentials.
- Do not store raw chat transcripts by default.
- Summarize only the context needed to continue work.
- Prefer links and file paths over copied proprietary content.
- Keep project-sensitive details concise and intentional.
