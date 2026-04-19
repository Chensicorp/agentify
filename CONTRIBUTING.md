# Contributing to Agentify

Thank you for your interest in contributing. Agentify is a small, focused project — contributions that stay within that scope are most likely to be accepted.

---

## What we welcome

- Bug reports and fixes
- New or improved platform adapters (Amazon Q, Cursor, Codex, etc.)
- Documentation improvements
- Activation quality improvements (better profile detection, risk identification)
- Real-world activation test reports

## What we are cautious about

- Large structural changes to `.agent-os/` core — open an issue first
- Adding dependencies (Agentify intentionally has none)
- Platform-specific logic in core files (use adapters instead)

---

## Reporting bugs

Open a GitHub issue. Include:
- What you did (exact prompt or workflow step)
- What happened
- What you expected
- Platform and model (e.g. Claude Code, claude-sonnet-4-6)
- Relevant state files if applicable (`repo-profile.md`, `current-risks.md`)

---

## Submitting a pull request

1. Fork the repo and create a branch: `fix/short-description` or `feat/short-description`
2. Make your changes
3. Test by running the activation workflow on a real repo and checking results
4. Submit the PR with a description of what changed and why

### Commit style

We use [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add Amazon Q adapter support
fix: boundaries.md now requires explicit confirmation for destructive ops
docs: improve quick start in README
chore: update .gitignore to exclude session logs
```

---

## Adding a new platform adapter

Adapters live in `.adapters/<platform>/`. They must:

1. Be thin — point to `.agent-os/README.md`, do not duplicate content
2. Follow the operating contract (investigate → plan → implement → verify → review)
3. Reference `.agent-os/verification/commands.md` and `project/boundaries.md`

Copy an existing adapter as a starting point. Test by running an activation on a real repo with that platform.

---

## Testing your changes

Agentify has no automated test suite. The acceptance criteria in `sandbox/acceptance-criteria/activate-studio.md` describe how to manually validate an activation. Run them against a real target repo before submitting changes that touch activation logic.

---

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
