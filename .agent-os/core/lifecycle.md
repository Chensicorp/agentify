---
# Lifecycle
---

## Studio lifecycle stages

### 1. Portable (pre-activation)

- **State:** `.agent-os/` installed; `state/current-focus.md` reads `pre-activation`; no `project/repo-profile.md`.
- **Capability:** structural templates only; no repo-specific knowledge.
- **Required action:** run `workflows/activate-studio.md`.

### 2. Active (post-activation)

- **State:** `project/repo-profile.md` exists; `state/` initialised; first session log written.
- **Capability:** full workflow suite; roles, orchestration, and session logging operational.
- **Triggers for re-activation:** major repo restructuring, stack change, profile age exceeding the repo's change velocity.

### 3. Stale

- **State:** `project/repo-profile.md` no longer reflects reality; commands in `verification/commands.md` fail.
- **Symptom:** entrypoints or commands do not match the current codebase.
- **Required action:** re-run Phase B of `workflows/activate-studio.md`.

---

## Task lifecycle

1. Task received → select tier (`workflows/orchestrate.md`)
2. Open session log (`logs/sessions/YYYY-MM-DD-NNN-<slug>.md`)
3. Run role sequence for tier
4. Update `state/` files
5. Write `logs/handoffs/current-handoff.md`
6. Append entry to `logs/activity-index.md`

---

## Session lifecycle

Each session:

1. Begins by reading `logs/handoffs/current-handoff.md` and `state/`.
2. Opens a new session log file.
3. Runs the designated phase(s).
4. Ends by updating `state/` and writing a fresh `logs/handoffs/current-handoff.md`.
5. Closes the session log with outcome and next recommended step.
