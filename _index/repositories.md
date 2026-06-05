# Repository Map

This file records stable repository identities. Local checkout paths are device-specific hints, not canonical truth.

## Rule

- Use `context_id`, `repo_id`, and `remote` as stable identifiers.
- Treat `local_paths` as optional hints for the current device.
- If a path does not exist on a device, locate the repo by remote URL or ask Sonny for that device's checkout path.

## Repositories

| Repo ID | Role | Remote | Local Path Hints |
| --- | --- | --- | --- |
| `sonny-codex-memory` | Codex memory and handoff repo | `git@github.com:SonJunHyuck/sonny-codex-memory.git` | See `_index/local-paths.md` |
| `project-bfx` | BFX implementation repo | TBD | See `_index/local-paths.md` |
