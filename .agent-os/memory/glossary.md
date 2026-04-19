# Glossary

Domain terms, acronyms, and repo-specific jargon. Add entries as the product vocabulary becomes clear.

---

## Agentify

- Meaning: The vendor-neutral operating layer installed under `.agent-os/`. Governs how coding agents investigate, plan, implement, verify, and review changes to this repository.
- Where it appears: `.agent-os/README.md`, adapter files, native shims.
- Notes: Distinct from Claude Code, Amazon Q, Codex, or Cursor — those are vendors. Agentify is the portable contract they all point to.

---

## Bootstrap

- Meaning: The initial process of installing the `.agent-os/` layer on a repository. Performed by the `bootstrap-repo` skill.
- Where it appears: `.agent-os/bootstrap/`, `.claude/skills/bootstrap-repo/`.
- Notes: Bootstrap is a one-time (or refresh) operation. It does not touch product code.

---

## Adapter

- Meaning: A thin, vendor-specific file under `.adapters/` that points agents to `.agent-os/`. Contains no substantive documentation of its own.
- Where it appears: `.adapters/claude/`, `.adapters/amazonq/`, `.adapters/codex/`, `.adapters/cursor/`.
- Notes: Adapters are kept short (under ~30 lines). All substance lives in `.agent-os/`.

---

## Native shim

- Meaning: A root-level file (`CLAUDE.md`, `AGENTS.md`, `.amazonq/rules/00-agent-os.md`) that vendor tooling reads automatically. Kept minimal — imports or points to the adapter or `.agent-os/README.md`.
- Where it appears: Repository root and `.amazonq/rules/`.
- Notes: If existing shims have project-specific content, preserve it and add a small "Agentify" pointer section.

---

