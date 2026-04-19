# Claude adapter — Agentify

This file is the Claude / Claude Code adapter. All substantive documentation lives in `.agent-os/`.

## Start here

1. Read `.agent-os/README.md`.
2. Read `state/current-focus.md`. If it reads `pre-activation`, **run `activate-studio` before any task work**. This is mandatory.
3. Read `logs/handoffs/current-handoff.md` for the latest work state.
4. Read `.agent-os/project/boundaries.md` before touching anything.

## Activation requirement

The studio is **not ready** until `workflows/activate-studio.md` has been run and `state/current-focus.md` no longer reads `pre-activation`.

To activate:
```
Activate this repository as an agent studio.
Run workflows/activate-studio.md.
Initialise runtime knowledge, roles, logs, handoff, command catalog, risk map, and state.
Do not modify product logic.
```

## Operating contract

- Use the **investigate → plan → implement → verify → review** sequence for any non-trivial task.
- Select the tier and role sequence from `workflows/orchestrate.md`.
- Open a session log in `logs/sessions/` for every task session.
- Write `logs/handoffs/current-handoff.md` at the end of every session.
- Select commands only from `verification/commands.md`. Do not invent commands.
- Check `verification/quality-gates.md` before declaring work complete.
- Record durable decisions in `memory/decisions.md`.
- Record transient findings and ambiguities in `memory/repo-notes.md`.
- Respect all boundaries listed in `project/boundaries.md`.

## Claude-specific notes

- `@` imports are supported in `CLAUDE.md`. The root `CLAUDE.md` imports this file.
- Session logs in the target repo should be committed to git — they are the persistence mechanism for context across sessions.
