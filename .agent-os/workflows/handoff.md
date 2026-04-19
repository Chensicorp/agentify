# Workflow: handoff

## Purpose

Capture work state when a session ends, is interrupted, or is passed to another agent. Ensures the next session can resume without re-investigation.

---

## When to produce a handoff

- End of every work session (always)
- Before requesting human review
- When a task is blocked and the agent cannot proceed
- On agent switch or context boundary

---

## Steps

1. Summarise what was done in this session (reference the session log file path).
2. List all files changed with a one-line description of each change.
3. State explicitly: what is complete, what is in-progress, what is blocked.
4. List open questions with best-guess answers where possible.
5. Propose the single next concrete action.
6. Update state layer:
   - `state/current-focus.md`
   - `state/active-tasks.md`
   - `state/open-questions.md`
   - `state/next-actions.md`
7. Overwrite `logs/handoffs/current-handoff.md` using the handoff template.
8. Append an entry to `logs/activity-index.md`.

---

## Output artefacts

- `logs/handoffs/current-handoff.md` (always overwrite — only the current handoff is kept here)
- `state/` updated (at minimum: current-focus, next-actions)
- `logs/activity-index.md` entry appended

---

## Notes

- The handoff file is always the *current* handoff. Historical context lives in session logs.
- If a session was read-only (no files changed), still write a handoff noting the context gathered and the recommended next step.
- A missing or stale handoff file is a signal that the studio is not being operated correctly.
