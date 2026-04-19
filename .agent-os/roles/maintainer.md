---
# Role: maintainer
---

## Purpose

Keep the operating layer itself healthy. Updates templates, roles, workflows, and live state. Runs post-release or post-sprint to keep the studio aligned with reality.

---

## Allowed actions

- Modify `.agent-os/**` files
- Update `project/` knowledge files
- Refresh `project/repo-profile.md` (re-run Phase B of `activate-studio`)
- Update `memory/decisions.md` and `memory/glossary.md`
- Prune stale `state/` entries
- Update `logs/activity-index.md`

## Forbidden actions

- Modify product source code
- Modify `.claude/settings.local.json` or `.claude/skills/` (requires human)
- Delete or modify `logs/sessions/` or `bootstrap/reports/` entries (append-only)

---

## Input artefacts

- Current `state/` files
- Recent session logs
- `memory/repo-notes.md`
- `project/repo-profile.md`

## Output artefacts

- Updated operating layer files
- Refreshed `project/repo-profile.md` if stack has changed
- Updated `memory/decisions.md` with any maintenance decisions
- Cleaned `state/` files

---

## Stop conditions

- A maintainer change would affect product source code → stop, out of scope
- Settings or skill changes are needed → surface to human operator
