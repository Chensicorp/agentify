---
# Escalation policy
---

## When to escalate

Any agent must stop work and surface to the human when:

| Trigger | Reason |
|---|---|
| **Scope expansion** | Task turns out larger than the current tier allows |
| **Boundary proximity** | A file or system in `project/boundaries.md` would be affected |
| **Contradictory evidence** | Two authoritative sources disagree with no clear resolution |
| **Missing information** | A non-trivial decision has no evidenced basis |
| **Unexpected state** | Files are missing, corrupted, or inconsistent with plan assumptions |
| **Irreversible side-effect** | Next step would have an external effect that cannot be undone |

---

## How to escalate

1. Stop the current phase immediately.
2. Write the blocker to `state/open-questions.md`.
3. Write `logs/handoffs/current-handoff.md` with the blocker clearly described.
4. Surface to the human with:
   - What was found
   - Why it blocks
   - What decision or information is needed
   - What the agent will do once unblocked

---

## What not to escalate

- Routine ambiguity that further investigation can resolve → investigate first.
- Missing stubs or empty template files → fill them from templates.
- Minor formatting or style choices → follow existing conventions.

---

## Tier escalation (intra-task)

If a task started as Tier 1 or 2 but reveals Tier 3 or 4 characteristics:

1. Note the re-tier reason in the session log.
2. Rewind to the plan phase for the new tier.
3. Apply the new tier's role sequence from that point forward.
4. Do not continue on the old tier after discovering the escalation trigger.
