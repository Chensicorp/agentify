# Boundaries

Zones that require extra care, human sign-off, or must not be modified by agents without explicit instruction.

---

## Do-not-touch zones

| Path | Reason |
|---|---|
| `.claude/settings.local.json` | Controls harness permissions. Wrong changes break or over-expose tool access. |
| `.claude/skills/` | Skill definitions — changes here alter agent behaviour globally. |
| `.agent-os/bootstrap/reports/` | Append-only bootstrap records. Do not retroactively modify. |

---

## Generated / vendored code

None yet. Add entries here when codegen, vendored libraries, or mirrored files appear.

---

## Stable public interfaces and contract surfaces

None yet. Add API contracts, published package interfaces, or protocol definitions here when they exist.

---

## Areas requiring human sign-off

| Area | Trigger |
|---|---|
| Permission changes | Any change to `.claude/settings.local.json` |
| Dependency additions | Any new entry in a lockfile or manifest |
| Schema changes | Any migration or schema alteration once a database is added |
| CI changes | Any change to CI workflow files once added |
| Deployment changes | Any change to infra, Dockerfile, or deploy scripts once added |
| Destructive operations | Delete or overwrite of **any product file**. The user's request to delete is **NOT** itself confirmation — the agent must stop, name the file and consequence, and ask a separate explicit yes/no question before issuing any command. Rationalising the request as implicit permission is not permitted. |

---

## Agentify layer write scope

The bootstrap and maintenance layer is permitted to write only:

- `.agent-os/**`
- `.adapters/**`
- `CLAUDE.md`
- `AGENTS.md`
- `.amazonq/rules/**`

Any write outside this scope requires explicit human approval.

---

## Append-only zones

The following paths must never have existing entries modified or deleted:

| Path | Rule |
|---|---|
| `.agent-os/bootstrap/reports/` | Append-only bootstrap records |
| `.agent-os/logs/sessions/` | Append-only session logs |
| `.agent-os/logs/activity-index.md` | Append-only activity index |
