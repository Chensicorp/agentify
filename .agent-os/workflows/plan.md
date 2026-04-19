# Workflow: plan

## Purpose

Produce a minimal, evidence-based implementation plan before writing any code. Establishes scope, affected paths, and verification strategy.

## Inputs

- Investigation findings
- Task packet (`tasks/_task-packet.md` filled)
- `project/boundaries.md`
- `verification/commands.md`

## Steps

1. State the goal clearly in one sentence.
2. List files and directories that will change — no more, no less.
3. Describe the minimal diff strategy: what is the smallest change that achieves the goal?
4. Note alternatives considered and why they were rejected.
5. Define the verification plan: which commands from `verification/commands.md` will confirm success?
6. Identify any approval needed (boundary zones, dependency changes, deploys).
7. Fill in `tasks/_plan-template.md` (copy first).

## Output artefacts

- Filled plan template
- Updated task packet with verification plan

## Stop conditions

- If the plan requires touching a boundary zone, stop and request human approval.
- If no safe verification commands exist, note this explicitly — do not proceed blind.

## Handoff

Pass the plan to the **implement** workflow. Implementation must not exceed the plan's declared scope without re-planning.
