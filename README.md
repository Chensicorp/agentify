# Agentify

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Ko-fi](https://img.shields.io/badge/Support-Ko--fi-FF5E5B?logo=ko-fi&logoColor=white)](https://ko-fi.com/chensicorp)

> **A repo-local operating layer for AI assistants.**
> A structured set of markdown files and workflow definitions that gives your AI persistent memory, a consistent workflow, boundary rules, and session continuity — without touching your product code.
>
> **Status: v0.1.0 — tested on Claude Code and OpenAI Codex CLI. Use as a reference pattern, adapt to your needs.**

---

## What it does

When you drop Agentify into a repository and activate it, your AI assistant goes from "smart autocomplete that forgets everything" to a structured development partner that:

- **Knows your codebase** — language stack, frameworks, entrypoints, dependencies, CI setup, all detected automatically on activation
- **Remembers between sessions** — every session ends with a written handoff; the next session starts by reading it ("Where did we leave off?" actually works)
- **Follows a structured workflow** — investigate → plan → implement → verify → review, instead of jumping straight to writing code
- **Respects boundaries** — will not silently delete or overwrite sensitive files (migrations, lock files, `.env`, product contracts); asks for explicit confirmation first
- **Catalogs what commands are safe to run** — distinguishes read-only commands from ones that require approval
- **Surfaces risks and open questions** — flags things like missing CI, exposed secrets patterns, and unresolved architecture ambiguities

### Example: what activation produces for a mid-size Python repo

After a single activation run, the agent correctly identified:
- Stack: Python 3.10+, async agent framework, pip + secondary package manager, pytest, Docker
- 7 risks including API key exposure (critical), no CI pipeline (high), live API calls in test suite (high)
- All entrypoints: CLI, demo script, Dockerfile, docker-compose
- 13 runnable commands, split into "safe to run" and "require approval"
- 5 genuine open questions: package manager authority, ambiguous test script, optional service dependency, missing CI
- Full architecture map with dependency flow and hotspot table

---

## What to expect

**Activation (first run):** 5–15 minutes depending on repo size. The agent reads your entire structure, writes a profile, and sets up the state layer. You do not need to intervene — just send the activation prompt and wait.

**Day-to-day:** No special commands needed. The agent reads its own state at the start of each session. For non-trivial tasks it will propose a plan before touching anything. For sensitive files it will stop and ask.

**What changes in your repo:** Only `.agent-os/` and `.adapters/` directories, plus a thin `CLAUDE.md` (or `AGENTS.md`) stub at the root. Your product code is never touched during activation.

**What does NOT change:** Your code, tests, configs, git history, CI pipelines. Agentify is a layer on top, not a modification of what you have.

---

## Quick start

### 1. Copy the files

Copy `.agent-os/` and `.adapters/` into the root of your target repository.

```
your-repo/
  .agent-os/       ← copy this
  .adapters/       ← copy this
```

> **Do not copy** `logs/sessions/*.md` or `logs/handoffs/current-handoff.md` — those are session history from this repo and will create noise in yours.

### 2. Wire up the adapter (Claude Code)

If your repo doesn't have a `CLAUDE.md` yet, copy the one from this repo too.

If you already have a `CLAUDE.md`, add this line to it:

```
@.adapters/claude/CLAUDE.md
```

### 3. Activate

Open the repo in Claude Code and send this prompt:

```
Activate this repository as an agent studio.
Run workflows/activate-studio.md.
Initialise runtime knowledge, roles, logs, handoff, command catalog, risk map, and state.
Do not modify product logic.
```

### 4. Done

The agent will present a summary of what it found. From here, work normally.

---

## Platform support

| Platform | Status | Notes |
|---|---|---|
| **Claude Code** (CLI / desktop / IDE) | **Tested** | Full activation + capability checks passed |
| Amazon Q Developer | Adapter exists, **not tested** | Uses Claude under the hood — behavior expected to be equivalent |
| **OpenAI Codex CLI** | **Tested** | Full activation + capability checks passed |
| Cursor | Adapter exists, **not tested** | Supports Claude + GPT models — behavior expected to be equivalent |

Agentify has been validated on **Claude Code** and **OpenAI Codex CLI**. Amazon Q and Cursor adapters are structurally present but not yet tested as standalone platforms — however, both support Claude and GPT models under the hood, the same models validated here, so results should be equivalent. If you try them, please report results via a GitHub issue.

---

## FAQ

**Q: Will it modify my code?**
No. During activation it only writes inside `.agent-os/`, `.adapters/`, and the root adapter stub (`CLAUDE.md`). Product code is never touched unless you explicitly ask the agent to work on a task.

**Q: Will it delete or break anything?**
Activation is designed to be non-destructive. In internal testing: all tests passed before and after activation, no tracked files were modified, no product code was touched. That said — run your own baseline tests before and after if you want to be sure.

**Q: What if activation goes wrong or produces a bad profile?**
The worst case is a low-quality `repo-profile.md` that misses some of your stack. The agent will note gaps and unknowns in `state/open-questions.md`. You can correct them manually or re-run activation. Nothing is irreversible.

**Q: Does it send my code anywhere?**
Agentify itself is just files. Whether your code is sent anywhere depends entirely on the AI platform you are using (Claude, Codex, etc.) — not on Agentify.

**Q: How does "session continuity" work?**
At the end of each session, the agent writes a structured handoff to `logs/handoffs/current-handoff.md`. At the start of the next session, it reads that file. If you ask "where did we leave off?", it will answer accurately from the handoff — not from model memory, which is unreliable across sessions.

**Q: What does "boundary enforcement" mean in practice?**
Certain files are marked as boundary-sensitive in `project/boundaries.md` (`.env`, lock files, migrations, product API contracts, etc.). Before deleting or overwriting them, the agent must ask you a separate yes/no confirmation — it cannot treat your original request as implicit permission.

**Q: Can I use it on multiple repos at the same time?**
Yes. Each repo gets its own independent copy of `.agent-os/`. They do not share state.

**Q: Does it work on a private repo?**
Yes. There is nothing network-dependent in Agentify. The files are local.

**Q: My repo has an unusual structure / monorepo / generated code — will activation work?**
Probably partially. The agent will detect what it can and flag the rest as open questions. Heavily generated or minified directories may produce lower-quality profiles. Monorepos are not specifically tested.

**Q: How do I update Agentify after you release a new version?**
Copy the updated `.agent-os/` and `.adapters/` over your existing ones. Your `state/`, `logs/`, `memory/`, and `project/` directories contain repo-specific data and should not be overwritten. Re-run activation if the new version adds new state files or changed the profile format.

**Q: The agent ignored a boundary rule and touched something it shouldn't have — what do I do?**
This has happened in testing (we found and fixed one case). If it happens to you: undo the change with git, and open an issue describing what command you gave and what the agent did. Include the relevant section of `project/boundaries.md`.

---

## Key files

| File | What it tells you |
|---|---|
| `logs/handoffs/current-handoff.md` | What was done last session and what is pending |
| `state/current-focus.md` | What the studio is currently working on |
| `state/current-risks.md` | Known risks in the repo |
| `state/next-actions.md` | Recommended next tasks |
| `project/repo-profile.md` | Detected stack, CI, package managers |
| `project/boundaries.md` | Files the agent must not touch silently |
| `verification/commands.md` | All approved runnable commands (safe vs approval-required) |

---

## Validation

After activation, run the checklist in [`docs/validation.md`](docs/validation.md) to verify everything worked correctly. It covers 8 structural checks, 4 capability checks, and 5 quality indicators — takes about 10 minutes.

---

## Known limitations

- Validated on Claude Code and OpenAI Codex CLI. Amazon Q and Cursor adapters exist but are not independently tested.
- Activation quality scales with repo clarity — well-structured repos get better profiles.
- Session logs accumulate in `logs/sessions/` with no automatic cleanup.
- No support yet for multi-agent or parallel sessions in the same repo.

---

## Support

Agentify is free, open source, and has no company behind it — just a tool built to solve a real problem.

If it saves you time, eliminates the "where did we leave off?" friction, or helps your AI actually understand your codebase — a coffee goes a long way.

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/chensicorp)
