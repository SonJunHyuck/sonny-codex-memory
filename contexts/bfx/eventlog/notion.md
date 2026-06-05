# Notion Seeds: BFX EventLog

This file collects structured material that can later be turned into Notion documentation.

## Candidate Notion Pages

- Codex Memory Workflow
- BFX EventLog Development Context
- Cross-Device Codex Handoff Process

## System Intent

The memory repo exists to let Codex continue work across devices and project chats without relying on a single local chat history.

The system separates two concerns:

- `sonny-codex-memory`: long-term context, handoff summaries, decisions, and documentation seeds.
- `ProjectBFX`: actual implementation code, tests, and project artifacts.

Repository identity should be stable across devices, while local checkout paths are device-specific. Notion documentation should refer primarily to repo ids, remotes, and context ids, not one machine's absolute filesystem path.

## Why This Exists

Sonny works across devices and may discuss multiple projects or multiple topics within one project. A single chat thread or a single project workspace does not naturally preserve the right context everywhere.

The memory repo gives Codex a durable, Git-backed source of truth that can be pulled at the start of a session and pushed at the end.

## Design Principles

- Keep raw chat transcripts out of the repo by default.
- Store durable context only.
- Organize context by project and topic.
- Keep daily session notes lightweight.
- Use decisions and timeline files as the source of truth for later documentation.
- Use `notion.md` as a bridge from working memory to reader-facing Notion pages.
- Keep local checkout paths in a separate device-specific index.

## Future Notion Structure

Possible sections for a Notion page:

1. What this workflow is
2. Why it was created
3. Repo roles
4. Start-of-session flow
5. End-of-session flow
6. Context file structure
7. Decision log
8. Known limitations
9. Next improvements

## Open Questions

- What is the exact canonical local path and remote URL for `ProjectBFX`?
- What are the Windows PC checkout paths for the memory repo and ProjectBFX?
- Should each mature context get a matching Notion page, or should multiple contexts roll up into project-level pages?
- Should Notion publishing be manual, semi-automated, or fully requested through Codex?
