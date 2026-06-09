# Project-BFX HUD Minimap Decisions

This file records decisions for `bfx.hud-minimap`.

## 2026-06-09: Create HUD minimap context

Decision:
Create a dedicated memory context under `contexts/bfx/hud-minimap` before starting implementation.

Reason:
The HUD minimap feature will likely touch UI layout, rendering strategy, player/target tracking, and asset setup, so it needs a focused place to preserve decisions and working notes.

Rejected alternatives:
- Reuse the existing event log context, which would mix unrelated feature history.

Impact:
Future HUD minimap work can append implementation notes and decisions without scattering context across sessions.
