# Agentify

This repository uses **Agentify** — a vendor-neutral operating layer for AI assistants.

## First: check activation status

Read `.agent-os/state/current-focus.md`.

- If it contains `pre-activation` → run the activation prompt below **before doing anything else**.
- If it says `post-activation` or `ready` → read `.agent-os/logs/handoffs/current-handoff.md` to resume from the last session.

## Activation prompt (run once per new repo)

```
Activate this repository as an agent studio.
Read and follow workflows/activate-studio.md step by step.
Initialise runtime knowledge, roles, logs, handoff, command catalog, risk map, and state.
Do not modify any product code.
```

## Operating contract (after activation)

1. **Before any task** — read `logs/handoffs/current-handoff.md` and `state/current-focus.md`.
2. **For non-trivial tasks** — follow: investigate → plan → implement → verify → review.
   Workflow definitions: `.agent-os/workflows/`
3. **Before running any command** — check `.agent-os/verification/commands.md`. Only run listed commands.
4. **Before touching any file** — check `.agent-os/project/boundaries.md`. Boundary-sensitive files require explicit human confirmation.
5. **Quality gate** — read `.agent-os/verification/quality-gates.md` before declaring any task complete.
6. **End of session** — run `.agent-os/workflows/handoff.md` and overwrite `logs/handoffs/current-handoff.md`.

## Session safety

**Write state frequently — do not wait until the end of a session.**
After each significant action: append to the session log and overwrite `logs/handoffs/current-handoff.md`.
Session logs (`logs/sessions/`) should be committed to git in this repo so context survives interruptions.

## Where things live

| What | Where |
|---|---|
| Full operating guide | `.agent-os/README.md` |
| Current work state | `.agent-os/state/current-focus.md` |
| Last session handoff | `.agent-os/logs/handoffs/current-handoff.md` |
| Boundary rules | `.agent-os/project/boundaries.md` |
| Approved commands | `.agent-os/verification/commands.md` |
| Workflow definitions | `.agent-os/workflows/` |
