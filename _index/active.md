# Active Contexts

This file is the starting map for active Codex memory contexts.

## Active

| Context ID | Project | Topic | Current | Status |
| --- | --- | --- | --- | --- |
| `bfx.eventlog` | BFX | EventLog | [current.md](../contexts/bfx/eventlog/current.md) | active |
| `codex-memory.workflow` | Codex Memory | Workflow | [current.md](../contexts/codex-memory/workflow/current.md) | active |

## Operating Model

- Work inside the actual project repo chat when implementing code.
- Use this memory repo as the long-term handoff and context layer.
- At the start of work, pull this repo from the current device's local checkout and read the relevant context.
- At the end of work, update the context, commit, and push this repo.
- Local filesystem paths differ between Mac and PC. Use `_index/repositories.md` for stable repo identity and `_index/local-paths.md` only as device-specific path hints.

## Known Project Repos

| Project | Repo ID | Role |
| --- | --- | --- |
| BFX | `project-bfx` | Actual implementation repo |
| sonny-codex-memory | `sonny-codex-memory` | Codex memory and handoff repo |
