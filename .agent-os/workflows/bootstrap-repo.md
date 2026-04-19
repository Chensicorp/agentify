# Workflow: bootstrap-repo

## Purpose

Install the vendor-neutral agent operating layer (`.agent-os/`) on a repository. Run once when first setting up a new or unfamiliar repo, or as a refresh when the repo structure has changed significantly.

## Inputs

- Repository root directory
- Optional operator notes (scope, constraints, intent)
- Existing agent instruction files (if any) — read before writing

## Steps

1. **Investigate** — scan the workspace; build an evidence-backed inventory of stack, commands, CI, entrypoints, boundaries, and existing instruction files. Write `bootstrap/reports/01-scan.md`.
2. **Plan** — decide what to create, amend, and leave untouched. Check approval gates. Write `bootstrap/reports/02-plan.md`.
3. **Implement** — create the full `.agent-os/` core tree, thin adapters under `.adapters/`, and minimal native shims. Write `bootstrap/reports/03-implement.md`.
   - When copying Agentify from a source repo, **do not carry over** `logs/sessions/*.md` or the contents of `logs/handoffs/current-handoff.md`. Create `logs/sessions/` as an empty directory and reset `current-handoff.md` to the template stub. Source-repo session history is irrelevant to the target and causes untracked-file noise.
4. **Verify** — check scope, file existence, link validity, adapter thinness, and run safe commands if available. Write `bootstrap/reports/04-verify.md`.
5. **Review** — self-review checklist; assemble `bootstrap/reports/bootstrap-report.md`.

## Output artefacts

- `.agent-os/` full tree
- `.adapters/` full tree
- `CLAUDE.md`, `AGENTS.md`, `.amazonq/rules/00-agent-os.md`
- `bootstrap/reports/01-scan.md` through `05-review.md`
- `bootstrap/reports/bootstrap-report.md`

## Stop conditions (approval gates)

Stop and ask if:
- A substantive existing agent instruction file would be overwritten
- Competing build systems exist with no clear primary
- Verification requires side-effect commands
- The repo is unhealthy (unreconciled state, no clear root)
- A write outside the allowed scope is needed

## Handoff

On completion, present the human-facing summary. Suggest the top three next tasks from `bootstrap-report.md`.
