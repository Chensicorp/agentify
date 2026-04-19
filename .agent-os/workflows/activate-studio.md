# Workflow: activate-studio

## Purpose

One-time mandatory activation that transforms the portable wrapper into a live agent studio for a specific repository. Must be completed before any other workflow runs on a new or unfamiliar repo.

## When to run

- First time the wrapper is installed in a repo (`state/current-focus.md` reads `pre-activation`)
- After major repository restructuring
- When `project/repo-profile.md` is missing or clearly stale

---

## Inputs

- Repository root directory
- Operator notes (optional): scope constraints, known tech, team context

---

## Phases

### Crash safety rule

**Write state after every phase.** At minimum, append a progress note to `logs/sessions/YYYY-MM-DD-001-activation.md` and update `logs/handoffs/current-handoff.md` at the end of each phase. If the session is interrupted, the next session must be able to resume from the last completed phase without re-doing work.

---

### Phase A — Verify portable layer

Confirm `.agent-os/` and `.adapters/` are fully installed.
If any core files are missing, stop and install them before continuing.
**After Phase A:** append "Phase A complete" to session log.

### Phase B — Repo scan and profile generation
*(After Phase B: write `project/repo-profile.md` and append "Phase B complete" to session log.)*

1. Enumerate all directories and files at depth ≤ 3.
2. Detect: language stack, frameworks, package managers, CI provider, test runner, linter, build tool.
3. Detect entrypoints: binaries, CLIs, web servers, background jobs.
4. Detect boundary candidates: auto-generated files, vendored code, DB migrations, infra scripts.
5. Enumerate all runnable commands from manifests, `package.json`, `Makefile`, CI files, and docs.
6. Write `project/repo-profile.md` with detected values.
7. Update `project/system-overview.md`, `project/architecture-map.md`, `project/entrypoints.md`.
8. Update `verification/commands.md` with all discovered commands, confidence-labelled.

### Phase C — Role and state initialisation
*(After Phase C: write all state files and overwrite `logs/handoffs/current-handoff.md` with progress so far.)*

1. Confirm all role files exist under `roles/`. Report any that are missing.
2. Initialise state layer:
   - `state/current-focus.md` — set to "post-activation / ready"
   - `state/active-tasks.md` — empty (no tasks yet)
   - `state/current-risks.md` — populate from Phase B scan findings
   - `state/open-questions.md` — populate with gaps and unknowns from Phase B
   - `state/next-actions.md` — list top 3 recommended next tasks
3. Create the first session log: `logs/sessions/YYYY-MM-DD-001-activation.md`.
4. Overwrite `logs/handoffs/current-handoff.md` with the post-activation handoff.
5. Append activation entry to `logs/activity-index.md`.

### Phase D — Adapter sync

1. Verify all adapter files reference `.agent-os/` correctly. Fix broken references if found.
2. Ensure a root `CLAUDE.md` exists in the instance root. If it is absent, create it with exactly this content:
   ```
   @.adapters/claude/CLAUDE.md
   ```
   This file is required so that a new Claude Code session loads the adapter and recognises the studio as activated.

---

## Output artefacts

- `project/repo-profile.md` (created or refreshed)
- `project/system-overview.md`, `project/architecture-map.md`, `project/entrypoints.md` (updated)
- `verification/commands.md` (updated)
- `state/` — all five files initialised with repo-specific content
- `logs/sessions/YYYY-MM-DD-001-activation.md` (first session log)
- `logs/handoffs/current-handoff.md` (post-activation handoff)
- `logs/activity-index.md` (activation entry appended)
- `memory/decisions.md` (activation decision recorded)

---

## Stop conditions

- Phase B returns no recognisable stack and no manifests → note all gaps, mark activation partial, ask operator for context before continuing.
- A write outside `.agent-os/` scope is needed → stop and ask.
- Existing files in `project/` would be destructively overwritten with worse information → merge instead.

---

## Handoff

After activation, present the operator with:
1. Detected stack summary
2. Discovered commands (with confidence levels)
3. Current risks from `state/current-risks.md`
4. Open questions from `state/open-questions.md`
5. Top 3 recommended next actions from `state/next-actions.md`

The studio is now active. All subsequent work must log sessions and update state.
