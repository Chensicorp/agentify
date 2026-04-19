# Quality gates

Default acceptance criteria for agent-produced work. Check all applicable gates before declaring a task complete.

---

## Pre-work gates

- [ ] A scoped task packet exists at `tasks/_task-packet.md` (copy and fill for the specific task)
- [ ] For non-trivial changes, a plan exists using `tasks/_plan-template.md`
- [ ] Relevant paths identified and boundaries checked against `project/boundaries.md`

---

## During-work gates

- [ ] No changes made outside the task's declared scope
- [ ] No production dependencies or lockfiles modified without explicit instruction
- [ ] No migrations, deploys, or external-system mutations executed
- [ ] Commands used are drawn from `verification/commands.md` — not invented

---

## Pre-completion gates

- [ ] Relevant tests pass (once tests exist — see `verification/commands.md`)
- [ ] Lint passes (once lint is configured)
- [ ] Typecheck passes (once a typed language is in use)
- [ ] Build succeeds for production-affecting changes (once a build step exists)
- [ ] No new boundaries violated
- [ ] Risks summarised and any surprises documented in `memory/repo-notes.md`

---

## Documentation gates

- [ ] Public-facing behaviour changes are reflected in docs if docs exist
- [ ] New entrypoints added to `project/entrypoints.md`
- [ ] Significant architectural decisions recorded in `memory/decisions.md`
- [ ] New domain terms added to `memory/glossary.md`

---

## Review gates

- [ ] Self-review using `tasks/_review-template.md` completed
- [ ] Diff restricted to intended scope only
- [ ] No debug code, commented-out blocks, or temporary stubs left behind
- [ ] If a boundary zone was touched, human review flagged explicitly

---

## Notes

Gates marked as "once X exists" are not blocking until the relevant infrastructure is in place. Do not fail a task on a gate that has no infrastructure behind it yet — but do note the gap in `memory/repo-notes.md`.
