---
context_id: codex-memory.workflow
project: Codex Memory
topic: Workflow
status: active
memory_repo:
  repo_id: sonny-codex-memory
  remote: git@github.com:SonJunHyuck/sonny-codex-memory.git
project_repos:
  - repo_id: sonny-codex-memory
    name: sonny-codex-memory
    remote: git@github.com:SonJunHyuck/sonny-codex-memory.git
---

# Codex Memory Workflow Current Context

## Purpose

This context captures the design of Sonny's Git-backed Codex memory workflow.

The goal is to let Codex continue work across PC, Mac, devices, project chats, and sessions by using a lightweight repository of durable context rather than relying on raw local chat history.

## Current Model

- `sonny-codex-memory` stores durable handoff context, decisions, timelines, session notes, templates, and future Notion documentation seeds.
- Actual implementation work should happen in the relevant project repo, such as `ProjectBFX`.
- The memory repo is read at the start of a work session and updated at the end.
- Contexts are organized by project and topic under `contexts/<project>/<topic>/`.
- Local checkout paths are device-specific and must not be treated as canonical.

## Standard Context Files

- `current.md`: the latest state needed to continue work.
- `decisions.md`: durable decisions, rationale, alternatives, and effects.
- `timeline.md`: major milestones and direction changes.
- `notion.md`: structured seeds for future Notion pages.
- `sessions/YYYY/MM/*.md`: lightweight source notes from specific work sessions.

## Device-Agnostic Path Model

Stable identifiers:

- `context_id`: identifies the topic context, such as `bfx.eventlog` or `codex-memory.workflow`.
- `repo_id`: identifies a repository across machines, such as `sonny-codex-memory` or `project-bfx`.
- `remote`: identifies the Git repository.

Device-specific hints:

- `_index/local-paths.md` stores local checkout paths for Mac and Windows PC.
- If a path is missing or stale, Codex should locate the repo by remote URL or ask Sonny for the current device path.

## Operating Prompts

Start of work:

```text
<Project/topic> 작업 시작하자.
현재 디바이스의 sonny-codex-memory checkout을 pull하고,
<context_id> context를 읽어서 현재 상태 요약한 뒤 이어서 작업해줘.
```

End of work:

```text
오늘 작업 내용을 sonny-codex-memory의 <context_id> context에 요약하고 commit/push해줘.
```

## Next Actions

- Fill in Windows PC local path hints once known.
- Confirm `ProjectBFX` remote URL and add it to `_index/repositories.md`.
- As new projects/topics emerge, create matching context directories from templates.
- Later, use `notion.md`, `decisions.md`, `timeline.md`, and session notes to create Notion documentation.
