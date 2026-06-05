# BFX EventLog Decisions

This file records durable decisions for the `bfx.eventlog` context. It should stay compact and explain why the current structure exists.

## 2026-06-05: Separate Memory Repo From Project Repo

Decision: Use `sonny-codex-memory` as a long-term memory repo and keep actual implementation work in `ProjectBFX`.

Rationale:

- Codex project chats are scoped to individual projects, but Sonny wants context to continue across devices and projects.
- The memory repo can act as the start and end point for context handoff.
- The project repo should remain focused on code, tests, and implementation artifacts.

Rejected alternative:

- Store all Codex chat context inside the implementation repo. This would mix personal handoff notes with project code and would not scale cleanly across multiple projects or topics.

Effect:

- BFX project chats should read from and write to this memory repo when continuity is needed.
- Implementation commits remain in the actual project repo.

## 2026-06-05: Organize Memory By Project And Topic

Decision: Store context under `contexts/<project>/<topic>/`.

For BFX EventLog:

```text
contexts/
  bfx/
    eventlog/
      current.md
      decisions.md
      timeline.md
      notion.md
```

Rationale:

- A single project can have multiple independent topics.
- A topic needs a current state, durable decisions, and a chronological timeline.
- This makes it easy to start work with a phrase like "A project topic 1 어디까지 했는지 pull 받고 이어가자."

Effect:

- Codex can resolve `bfx.eventlog` to `contexts/bfx/eventlog/`.

## 2026-06-05: Keep Sessions Lightweight

Decision: Keep `sessions/` as optional, lightweight source notes rather than full chat logs.

Rationale:

- `current.md`, `decisions.md`, and `timeline.md` are enough for day-to-day continuity.
- Later Notion documentation benefits from knowing what happened during a specific work session.
- Full transcripts are too noisy and unnecessary.

Effect:

- Session notes should capture intent, changes, decisions, Notion seeds, and next steps.
- Do not store raw chat logs by default.

## 2026-06-05: Add Notion Seeds

Decision: Add `notion.md` per topic as a staging area for future Notion documentation.

Rationale:

- Sonny plans to later document what systems were built, why they were built, and how they work in Notion.
- `current.md` describes the current state, but Notion pages need audience-facing structure and rationale.
- `sessions/` provides raw source notes; `notion.md` distills them into documentation seeds.

Effect:

- When a topic matures, Codex can use `notion.md`, `decisions.md`, and `timeline.md` to create or update Notion pages.
