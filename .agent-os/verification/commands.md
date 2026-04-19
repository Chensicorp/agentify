# Verification commands

Command catalogue for agents. Always use commands from this file. Do not invent or guess commands.

---

## High-confidence commands

None. No manifests, CI, or scripts exist yet.

---

## Medium-confidence commands

None. No evidence source available.

---

## Unconfirmed / missing commands

| Purpose | Placeholder | Status |
|---|---|---|
| Run tests | `<unknown — no manifest>` | Unconfirmed |
| Lint | `<unknown — no manifest>` | Unconfirmed |
| Build | `<unknown — no manifest>` | Unconfirmed |
| Typecheck | `<unknown — no manifest>` | Unconfirmed |
| Format | `<unknown — no manifest>` | Unconfirmed |
| Start dev server | `<unknown — no manifest>` | Unconfirmed |

---

## Update instructions

When product code and manifests land:

1. Run the investigate workflow to discover commands from manifests, CI, and docs.
2. Assign each command a confidence level:
   - `high` — evidenced in CI and documented as standard
   - `medium` — in manifest scripts but not seen in CI
   - `low` — inferred from config only
3. Include evidence source for every command.
4. Remove placeholders once real commands are known.

---

## Safe-to-run criteria

A command is safe to run during agent verification if it:
- Does not install or remove packages
- Does not mutate a database or data store
- Does not talk to external services
- Does not deploy or modify infrastructure
- Is read-only or operates on local files only
