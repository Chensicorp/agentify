# Workflow: review

## Purpose

Final self-check before handing work to a human. Confirm correctness, scope, safety, and documentation completeness.

## Inputs

- Verification results
- Task packet
- Original plan
- `tasks/_review-template.md`

## Steps

1. Fill in `tasks/_review-template.md` (copy first).
2. Answer each review question honestly:
   - What changed? (be specific — file names and line ranges if useful)
   - What was verified? (commands run and outcomes)
   - Were boundaries respected?
   - Is the change backwards-compatible with public contracts (if any)?
   - What risks remain?
   - What follow-up tasks should be logged?
3. Log durable decisions (architectural or workflow changes) in `memory/decisions.md`.
4. Log transient observations or unresolved questions in `memory/repo-notes.md`.
5. Add new domain terms to `memory/glossary.md`.
6. Produce a concise human-facing summary: what changed, what was verified, what remains open.

## Output artefacts

- Filled review template
- Updated `memory/decisions.md` (if applicable)
- Updated `memory/repo-notes.md` (if applicable)
- Human-facing summary

## Stop conditions

- If the review reveals a risk that was not captured in the plan → flag it explicitly before handing off.
- If an unresolved open question could affect correctness → do not mark complete; escalate.

## Handoff

Present the human-facing summary. Mark the task complete only when all applicable quality gates in `verification/quality-gates.md` are satisfied.
