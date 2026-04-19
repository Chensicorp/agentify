# Workflow: orchestrate

## Purpose

Define how roles collaborate on a task. Selects the correct role sequence based on task complexity and risk profile.

---

## Pre-condition

Studio must be active (`state/current-focus.md` must not read `pre-activation`).
If it does, run `activate-studio` first.

---

## Task tiers

### Tier 1 — Trivial

**Criteria:** single-file change, no boundary proximity, no architectural impact, unambiguously scoped.

**Role sequence:**
```
investigator → implementer
```

**Minimum artefacts:** investigation note, implementation note.

---

### Tier 2 — Standard

**Criteria:** multi-file change, OR touches a stable interface, OR has more than one risk factor.

**Role sequence:**
```
investigator → planner → implementer → verifier
```

**Minimum artefacts:** investigation report, plan, implementation notes, verification report.

---

### Tier 3 — Non-trivial

**Criteria:** cross-module change, new dependency, schema change, API contract change, CI/infra modification, or any change an investigator flags as structurally significant.

**Role sequence:**
```
investigator → planner → architect (review plan) → implementer → verifier → reviewer
```

**Minimum artefacts:** all Tier 2 artefacts plus architect sign-off and reviewer report.

---

### Tier 4 — High-risk / boundary-sensitive

**Criteria:** touches a boundary zone listed in `project/boundaries.md`, requires human sign-off, or affects production data or external systems.

**Role sequence:**
```
investigator → planner → architect → [HUMAN SIGN-OFF] → implementer → verifier → reviewer → maintainer
```

**Human gate is mandatory** — agent must stop and present the full plan to the operator before any implementation begins.

---

## Escalation

Any agent in any tier may escalate to a higher tier if:
- Scope turns out larger than the current tier allows
- A boundary zone is found to be involved
- Contradictory evidence surfaces
- A risk materialises not covered by the plan

Escalation always rewinds to the planning phase of the new tier. See `core/escalation-policy.md` for full rules.

---

## Session logging (required for all tiers)

Every orchestration run must:

1. Open a session log at `logs/sessions/YYYY-MM-DD-NNN-<slug>.md`.
2. Record each role transition in the log.
3. Update `logs/activity-index.md` at the end.
4. Write `logs/handoffs/current-handoff.md` on completion or suspension.
5. Update `state/` files as appropriate.
