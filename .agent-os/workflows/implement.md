# Workflow: implement

## Purpose

Execute the plan. Make the minimal, targeted change and nothing else.

## Inputs

- Filled plan (from plan workflow)
- Filled task packet
- `project/boundaries.md`
- `verification/commands.md`

## Steps

1. Re-read the plan. Confirm scope before touching any file.
2. Make changes in the order specified by the plan.
3. Stay within the declared scope — if you discover the plan needs expanding, stop and re-plan rather than improvising.
4. Do not add features, refactors, or improvements beyond the task.
5. Do not modify boundary zones without explicit human approval.
6. Do not add, remove, or upgrade dependencies unless that is the explicit task.
7. When done, note what changed and what was intentionally left unchanged.

## Output artefacts

- Changed files (within declared scope only)
- Implementation notes for the verify step

## Stop conditions

- Scope creep detected → stop, re-plan, re-confirm.
- Unexpected file state (e.g. a file that should exist doesn't, or contains conflicting content) → stop, investigate, ask.
- A change would touch a boundary zone not covered by the plan → stop, flag.

## Handoff

Pass implementation notes to the **verify** workflow. Do not declare work done before verification.
