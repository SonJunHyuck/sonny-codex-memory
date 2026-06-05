---
context_id: bfx.eventlog
project: BFX
topic: EventLog
status: active
memory_repo: /Users/sonny/Workspace/Dev/sonny-codex-memory
project_repos:
  - name: ProjectBFX
    local_path: /Users/sonny/Workspace/Dev/ProjectBFX
---

# BFX EventLog Current Context

## Purpose

This context exists so Codex can continue BFX EventLog work across devices, project chats, and sessions without relying on a single local chat history.

The memory repo is not the implementation workspace. It stores durable context: what was decided, where the work currently stands, and what should happen next.

## Operating Flow

Start BFX work from the BFX project chat with a prompt like:

```text
BFX 작업 시작하자.
먼저 /Users/sonny/Workspace/Dev/sonny-codex-memory pull하고,
bfx.eventlog context 읽어서 현재 상태 요약한 뒤
이 BFX 프로젝트에서 이어서 작업해줘.
```

Expected Codex flow:

1. Pull `/Users/sonny/Workspace/Dev/sonny-codex-memory`.
2. Read `contexts/bfx/eventlog/current.md`, `decisions.md`, and `timeline.md`.
3. Read the actual BFX project repo.
4. Implement, test, and commit in the project repo when requested.
5. At the end, update this memory context and push the memory repo.

## Current Understanding

- `sonny-codex-memory` is the long-term memory and handoff repo.
- `ProjectBFX` is the actual implementation repo.
- Contexts are organized by project and topic.
- For BFX EventLog, the canonical context path is `contexts/bfx/eventlog/`.
- The core working files are `current.md`, `decisions.md`, `timeline.md`, and `notion.md`.

## Important Files

- `current.md`: current state and next handoff summary.
- `decisions.md`: durable decisions, rationale, alternatives, and effects.
- `timeline.md`: major changes and milestones in chronological order.
- `notion.md`: structured seeds for later Notion documentation.
- `sessions/YYYY/MM/*.md`: lightweight source notes for individual work sessions.

## Next Actions

- Confirm the actual local path and remote URL for `ProjectBFX`.
- When BFX EventLog implementation work begins, read the project code and replace this planning context with real implementation context.
- Keep session notes short and useful, especially when a session creates decisions or Notion-worthy rationale.
