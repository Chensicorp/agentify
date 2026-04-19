# Conventions

> Status: **stub** — no product code to derive conventions from. Update when source files and CI are added.

---

## Agentify layer conventions

These apply now, regardless of product stack:

- All agent operating files live under `.agent-os/`. Do not put agent guidance in root-level `CLAUDE.md` or `AGENTS.md` beyond a thin pointer.
- Adapter files under `.adapters/` are thin — under ~30 lines each. Do not duplicate system docs there.
- Memory files use the formats defined in `memory/decisions.md`, `memory/repo-notes.md`, and `memory/glossary.md`.
- Task templates under `tasks/` are starting points. Copy and fill; do not edit the templates themselves.
- Report files under `bootstrap/reports/` are append-only records. Do not retroactively edit them.

---

## Product conventions

To be filled in when product code lands. Expected areas:

- Naming patterns (files, functions, variables, modules)
- Test conventions (location, naming, framework)
- Import and module conventions
- Branching and PR conventions
- Linting and formatting rules
- Documentation expectations

---

## Non-obvious constraints

- `.claude/settings.local.json` grants `Bash(claude:*)` — this allows running `claude` CLI subcommands. Do not expand these permissions without deliberate review.
