# Codex Memory Workflow Decisions

This file records durable decisions about the Codex memory system itself.

## 2026-06-05: Use A Git-Backed Memory Repo

Decision: Use `sonny-codex-memory` as a Git-backed memory repo for Codex handoff context.

Rationale:

- Codex chat history may be local to a device or scoped to a specific project chat.
- Sonny wants to move between PC and Mac without losing project context.
- Git pull and push provide a simple cross-device sync mechanism.

Rejected alternatives:

- Rely on one device's local chat history.
- Store raw chat transcripts as the primary memory source.

Effect:

- Work starts by pulling the memory repo and reading the relevant context.
- Work ends by updating context files, committing, and pushing.

## 2026-06-05: Separate Memory From Implementation

Decision: Treat the memory repo as a handoff and documentation layer, not the implementation workspace.

Rationale:

- Implementation repos should contain code, tests, and project artifacts.
- The memory repo should contain why, what changed, what was decided, and what should happen next.
- Multiple project contexts can coexist without polluting implementation repos.

Effect:

- BFX implementation happens in `ProjectBFX`.
- BFX EventLog context lives in `contexts/bfx/eventlog/`.

## 2026-06-05: Organize Contexts By Project And Topic

Decision: Use `contexts/<project>/<topic>/` for memory context directories.

Rationale:

- A single project can have several active topics.
- A topic needs its own current state, decisions, timeline, and Notion seeds.

Effect:

- `bfx.eventlog` maps to `contexts/bfx/eventlog/`.
- `codex-memory.workflow` maps to `contexts/codex-memory/workflow/`.

## 2026-06-05: Keep Local Paths Device-Specific

Decision: Do not hard-code one machine's absolute paths as canonical context.

Rationale:

- Sonny will move between PC and Mac.
- Absolute paths such as `/Users/...` only make sense on one device.
- Repo identity should be stable across machines.

Effect:

- Use `context_id`, `repo_id`, and remote URLs as stable identifiers.
- Store per-device checkout paths only in `_index/local-paths.md`.

## 2026-06-05: Keep Sessions Lightweight

Decision: Keep `sessions/` as short source notes rather than exhaustive logs.

Rationale:

- Day-to-day continuation mostly needs `current.md`, `decisions.md`, and `timeline.md`.
- Later Notion documentation benefits from a short record of what happened and why.
- Full chat logs are too noisy and should not be stored by default.

Effect:

- Session notes capture intent, changes, decisions, Notion seeds, and next actions.
