# Notion Seeds: Codex Memory Workflow

This file collects structured material for a future Notion page about Sonny's Codex memory workflow.

## Candidate Notion Page

Codex Memory Workflow

## Audience

Sonny, future Codex sessions, and possibly collaborators who need to understand how project context is carried across devices.

## Core Explanation

The Codex memory workflow is a Git-backed handoff system. It lets Codex continue work across devices and project chats by reading a small set of durable Markdown files instead of depending on local chat history.

## Why It Exists

Sonny moves between PC and Mac and may work on multiple projects or multiple topics within one project. Codex project chats are useful for actual work, but they are not enough by themselves as a cross-device, cross-project memory layer.

## Key Concepts

- Memory repo: `sonny-codex-memory`
- Context id: a stable project/topic key, such as `bfx.eventlog`
- Repo id: a stable repository identity, such as `project-bfx`
- Local path hint: a device-specific checkout path
- Current context: the latest handoff state
- Decisions: durable rationale
- Timeline: milestone history
- Sessions: lightweight source notes

## System Shape

```text
_index/
  active.md
  repositories.md
  local-paths.md

contexts/
  <project>/
    <topic>/
      current.md
      decisions.md
      timeline.md
      notion.md

sessions/
  YYYY/
    MM/
      YYYY-MM-DD-<context-id>.md

templates/
```

## Documentation Angle

The eventual Notion page should explain:

1. The continuity problem
2. Why Git was chosen
3. How memory and implementation repos differ
4. How to start a session
5. How to end a session
6. How PC/Mac local paths are handled
7. How context files become future documentation

## Open Questions

- Should Notion pages be generated per context, per project, or both?
- Should session notes be automatically rolled up into `notion.md`?
- Should there be a script or checklist for creating a new context?
