# Changelog

All notable changes to Agentify will be documented here.

Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
Versioning: [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [0.1.0] — 2026-04-19

### Added
- Core `.agent-os/` operating layer: roles, workflows, state, logs, memory, verification
- Vendor adapters: Claude Code, Amazon Q, OpenAI Codex, Cursor
- `activate-studio` workflow — one-time repo scan and studio initialisation
- `bootstrap-repo` workflow — install Agentify into a new repo
- `orchestrate` workflow — tier selection and role sequencing
- Full workflow set: investigate, plan, implement, verify, review, handoff
- Seven role definitions with distinct permissions and output contracts
- Boundary enforcement system (`project/boundaries.md`)
- Command catalog with safe/approval-required split (`verification/commands.md`)
- Quality gates (`verification/quality-gates.md`)
- Session log and handoff templates

### Platform support
- **Tested:** Claude Code (CLI, desktop, IDE extensions)
- **Tested:** OpenAI Codex CLI — full activation + capability checks passed (boundary enforcement, session continuity, architecture detection)
- **Untested:** Amazon Q Developer, Cursor — adapters present but not validated

### Known limitations
- Activation quality depends on repo structure clarity
- No automated test suite — validation is manual (acceptance criteria in `sandbox/acceptance-criteria/`)
- Session logs accumulate with no automatic cleanup
