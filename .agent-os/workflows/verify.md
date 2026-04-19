# Workflow: verify

## Purpose

Confirm the implementation is correct, complete, and within scope before declaring done.

## Inputs

- Implementation notes
- Verification plan from the task plan
- `verification/commands.md`
- `verification/quality-gates.md`

## Steps

1. Run all applicable commands from the verification plan, drawn from `verification/commands.md`.
2. Only run commands that are safe (no installs, no mutations, no external calls).
3. Check all quality gates in `verification/quality-gates.md` that apply to this task.
4. Confirm the diff is restricted to declared scope — nothing extra crept in.
5. Confirm no boundary zones were touched without approval.
6. If any verification step fails: investigate the root cause before retrying. Do not retry blindly.
7. Record any surprises or new ambiguities in `memory/repo-notes.md`.

## Output artefacts

- Verification results (pass / fail / skipped with reason for each gate)
- Updated `memory/repo-notes.md` if new issues found

## Stop conditions

- A quality gate fails and cannot be resolved within the task's scope → escalate to human.
- A safe command is unavailable (not in `commands.md`) → note as unverified gap, do not invent commands.
- Diff reveals out-of-scope changes → revert extras, re-verify.

## Handoff

Pass verification results to the **review** workflow. Include any unresolved gaps explicitly.
