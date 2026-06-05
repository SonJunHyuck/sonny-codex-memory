# 2026-06-05 codex-memory.workflow

## Intent

Capture the design conversation for Sonny's Codex memory repo as its own project/topic context.

## Changed

- Added `codex-memory.workflow` as an active context.
- Created `contexts/codex-memory/workflow/`.
- Captured the memory repo's purpose, device-agnostic repo identity model, decisions, timeline, and Notion seeds.

## Decisions

- Treat the memory workflow itself as a first-class project context.
- Use `context_id: codex-memory.workflow`.
- Keep path handling device-agnostic through `repo_id`, remote URLs, and `_index/local-paths.md`.

## Notion Seeds

- Future page title: Codex Memory Workflow.
- Explain the system as a Git-backed handoff layer for Codex context.
- Emphasize the separation between memory repo and implementation repo.
- Explain why local paths are hints rather than canonical truth.

## Next

- Add Windows PC local path hints once known.
- Add `ProjectBFX` remote URL when confirmed.
- Consider a new-context checklist or script if this pattern repeats often.
