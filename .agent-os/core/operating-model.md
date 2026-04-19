---
# Operating model
---

## Design principles

1. **Investigation before action** — no file is changed without an evidenced investigation.
2. **Minimal diff** — only change what the task requires; stop if scope grows.
3. **Explicit confidence labels** — mark findings as `evidenced`, `inferred`, or `provisional`.
4. **Boundary respect** — boundary zones are hard stops without human sign-off.
5. **Session continuity** — every session produces a handoff; every handoff enables cold-start.
6. **Role separation** — roles have distinct permissions and output contracts; do not blend them.

---

## Agent operating sequence

For any non-trivial task:

```
investigate → plan → implement → verify → review
```

Select the tier and role sequence from `workflows/orchestrate.md` before starting.

---

## State management

| Layer | Location | Purpose |
|---|---|---|
| Live state | `state/` | Current focus, active tasks, risks, questions, next actions |
| Session records | `logs/sessions/` | Full record of each session |
| Current handoff | `logs/handoffs/current-handoff.md` | Cold-start packet for next agent/session |
| Activity index | `logs/activity-index.md` | Append-only event index |
| Durable decisions | `memory/decisions.md` | Accepted architectural decisions |
| Transient notes | `memory/repo-notes.md` | Observations and ambiguities |

---

## Studio readiness

The studio is **not ready** until `activate-studio` has been run and `state/current-focus.md` is no longer in `pre-activation` status.

If `state/current-focus.md` reads `pre-activation`, run `workflows/activate-studio.md` before any task work.
