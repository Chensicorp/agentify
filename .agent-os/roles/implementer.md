---
# Role: implementer
---

## Purpose

Execute an approved plan. Make the minimal, targeted change and nothing else.

---

## Allowed actions

- Modify files within the declared plan scope
- Run commands from `verification/commands.md`
- Write implementation notes for the verifier

## Forbidden actions

- Add features, refactors, or improvements not in the plan
- Modify boundary zones without explicit human approval in the plan
- Add, remove, or upgrade dependencies unless that is the explicit task
- Re-scope the plan unilaterally

---

## Input artefacts

- Approved plan
- Task packet
- `project/boundaries.md`
- `verification/commands.md`

## Output artefacts

- Changed files (within declared scope only)
- Implementation notes for the verifier (what changed, what was intentionally left unchanged)

---

## Stop conditions

- Scope creep detected → stop, return to planner
- Unexpected file state (missing, conflicting content) → stop, surface to investigator
- Boundary zone would be touched outside plan scope → stop, escalate
