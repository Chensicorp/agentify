# Agentify — Codex adapter

This repository uses Agentify. The root `AGENTS.md` is your primary entry point.

## Codex-specific notes

- Read files explicitly when instructed — do not assume content from filenames alone.
- `@import` syntax is not used here. All pointers are plain file paths.
- If a workflow step says "write X", create or overwrite that file with real content.
- Session logs go in `.agent-os/logs/sessions/YYYY-MM-DD-NNN-slug.md`.

## Start here

1. Read root `AGENTS.md` (already loaded).
2. Check `.agent-os/state/current-focus.md` for activation status.
3. If pre-activation: run the activation prompt from root `AGENTS.md`.
4. If active: read `.agent-os/logs/handoffs/current-handoff.md` and resume.
