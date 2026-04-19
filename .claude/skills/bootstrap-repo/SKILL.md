---
name: bootstrap-repo
description: Analyse the current repository and create a vendor-neutral agent operating system under .agent-os plus thin adapters under .adapters and minimal native entry files for Claude, Amazon Q, Codex, and Cursor. Use when opening a new or unfamiliar repository and you want a safe, repeatable agent-first wrapper without changing product logic.
disable-model-invocation: true
---

Bootstrap this repository into a vendor-neutral agent workspace. Scan the codebase, infer how it is built, tested, linted, verified, and shipped, then create `.agent-os/` as the portable source of truth and `.adapters/` as thin vendor mappings. Work through investigate → plan → implement → verify → review. Do not change product logic, runtime behaviour, dependencies, lockfiles, generated artefacts, deployment state, or data. Default to best-effort unattended execution. Stop only at explicit approval gates.

# bootstrap-repo

## Mission

Your job is to wrap the current repository with an agent operating layer that future coding agents can use consistently, regardless of model or platform.

Use this operating contract:

- The durable operating model lives in `.agent-os/` as plain Markdown.
- Vendor-specific adapter files live in `.adapters/`.
- Native entry files such as `CLAUDE.md`, `AGENTS.md`, and `.amazonq/rules/*.md` are only thin shims.
- The bootstrap must be evidence-based, concise, and repo-specific.
- If `$ARGUMENTS` is present, treat it as optional scope, notes, or operator intent. Record it in the report and apply it where relevant.

## Hard rules

These rules are mandatory.

- Do not change product logic, application behaviour, schemas, migrations, infrastructure state, CI behaviour, or deployment behaviour during bootstrap.
- Do not add, remove, upgrade, install, or update production dependencies or lockfiles.
- Do not run package installs, database migrations, seeders, deploys, Terraform applies, cloud mutations, or scripts that touch external systems.
- Do not overwrite existing agent instruction files blindly. Preserve, merge minimally, and document what you changed.
- Do not invent commands. Every build, test, lint, typecheck, format, run, or CI command must be supported by evidence from manifests, scripts, configs, CI, docs, or existing instructions.
- Do not treat build output, vendored code, generated code, caches, coverage folders, or dependency directories as authoritative sources.
- Do not ask early clarifying questions. Continue best-effort unless an approval gate is triggered.
- Prefer tracked files and top-level evidence. Use `git ls-files` before broad tree walks.
- Allowed write scope is limited to:
  - `.agent-os/**`
  - `.adapters/**`
  - `CLAUDE.md`
  - `AGENTS.md`
  - `.amazonq/rules/**`
- If any other write seems necessary, stop and ask.
- If the repository already contains `.agent-os/`, treat this task as a merge-or-refresh, not a rewrite.

## Approval gates

Proceed unattended unless one of these conditions occurs.

Stop and ask for human confirmation only if:

- a substantive `CLAUDE.md`, `AGENTS.md`, `.amazonq/rules/`, or `.agent-os/` already exists and your change would replace or materially reinterpret it
- the repo has multiple competing build systems or command sets and there is no clear primary one from CI or lockfiles
- verification would require a command with possible side effects or external access
- the repo looks unhealthy in a way that makes bootstrap misleading, such as unreconciled generated files, no clear root, or obvious partial migration
- you would need to write outside the allowed write scope

If you must stop, ask one concise question, give one recommended default, and explain the consequence of proceeding without an answer.

## Lifecycle

Work in five phases and write a report file for each phase under `.agent-os/bootstrap/reports/`.

The five report files are:

- `01-scan.md`
- `02-plan.md`
- `03-implement.md`
- `04-verify.md`
- `05-review.md`

Also produce a final summary report:

- `bootstrap-report.md`

At the start of each phase, state the phase in one sentence.

### Investigate

Use this internal prompt for yourself:

Scan only. Do not change product files. Build an evidence-backed inventory of stack, commands, CI, entrypoints, boundaries, existing instruction files, and likely risk areas. Label uncertain items as inferred or provisional.

Perform the investigation in this order.

#### Workspace check

Run or emulate the following in a safe way:

- `pwd`
- `git rev-parse --show-toplevel`
- `git status --short`
- `git ls-files`

Determine:

- repository root
- whether this is a monorepo, multi-service repo, library, app, infra repo, or mixed repo
- whether the working tree is clean or dirty
- whether existing agent files already exist

#### Existing guidance check

Inspect, if present:

- `README*`
- `CONTRIBUTING*`
- `docs/**`
- `CLAUDE.md`
- `AGENTS.md`
- `.amazonq/rules/**`
- `.cursor/**`
- `.claude/**`
- `.codex/**`
- `Makefile`, `justfile`, `Taskfile*`
- CI directories and workflow files

Extract:

- repo purpose
- local developer workflow
- quality expectations
- non-obvious conventions
- existing constraints or no-go areas

#### Manifest and lockfile scan

Look for common manifests and lockfiles, including but not limited to:

- JavaScript and TypeScript: `package.json`, `pnpm-lock.yaml`, `package-lock.json`, `yarn.lock`, `tsconfig*.json`
- Python: `pyproject.toml`, `poetry.lock`, `uv.lock`, `requirements*.txt`, `setup.py`, `Pipfile`, `tox.ini`, `noxfile.py`
- Java and Kotlin: `pom.xml`, `build.gradle*`, `settings.gradle*`, `gradlew`
- Go: `go.mod`, `go.sum`
- Rust: `Cargo.toml`, `Cargo.lock`
- Ruby: `Gemfile`, `Gemfile.lock`
- PHP: `composer.json`, `composer.lock`
- .NET: `*.csproj`, `*.sln`, `Directory.Build.props`
- Elixir: `mix.exs`, `mix.lock`
- Scala: `build.sbt`, `project/**`
- C and C++: `CMakeLists.txt`, `meson.build`, `Makefile`
- Containers and infra: `Dockerfile*`, `compose*.yml`, `.terraform.lock.hcl`, `*.tf`, `Chart.yaml`, `helmfile*`

Infer:

- primary language(s)
- secondary language(s)
- package managers
- runtimes and toolchains
- likely primary build graph

Do not declare a language “primary” from a stray file alone.

#### Command discovery

Infer build, test, lint, format, typecheck, dev-run, and CI commands from evidence, in this priority order:

- explicit commands in existing instruction files
- CI workflows
- manifest scripts and task runners
- Make or just targets
- test and lint config files
- README or docs examples

For each detected command, assign confidence:

- high: directly evidenced and used in CI or documented as standard
- medium: directly evidenced in scripts or manifests, but not seen in CI
- low: inferred from tool config only

Never promote a low-confidence command to high without evidence.

#### CI discovery

Inspect common CI locations:

- `.github/workflows/**`
- `.gitlab-ci.yml`
- `Jenkinsfile`
- `.circleci/**`
- `azure-pipelines*.yml`
- `buildkite*`
- `drone*`

Record:

- the canonical test or verification path
- matrix dimensions if obvious
- important environment assumptions
- whether the repo validates lint, tests, typechecks, builds, docs, or packaging

#### Entrypoint discovery

Identify likely entrypoints such as:

- service binaries
- app startup modules
- CLI commands
- server handlers
- job runners
- schedulers
- cron tasks
- worker processes
- notebooks or data pipeline entry modules
- compose or deployment entry scripts

Prefer evidence from:

- manifest scripts
- framework conventions
- routing or main files
- container entrypoints
- CLI registration code

#### Risk scan

Mark likely risk areas such as:

- authentication, authorisation, secrets, permissions
- billing, payments, compliance, PII
- public API contracts and SDKs
- database schemas and migrations
- data pipelines, ETL, backfills, destructive jobs
- shared libraries or core packages used across services
- concurrency, queues, retries, messaging
- generated code, protocol buffers, OpenAPI, codegen
- infrastructure, deploy, environment, feature flags
- security-sensitive config or cryptography

Explain why each area is risky in repo-specific terms.

#### Deliverable for investigation

Write `.agent-os/bootstrap/reports/01-scan.md` with these sections:

- Repository profile
- Languages and confidence
- Manifests and lockfiles
- Build, test, lint, typecheck, format, and run commands with confidence and evidence
- CI overview
- Entrypoints
- Existing instruction files
- Likely risk areas
- Open ambiguities
- Recommended bootstrap scope

Use explicit labels:

- `evidenced`
- `inferred`
- `provisional`

### Plan

Before writing any bootstrap files, create `.agent-os/bootstrap/reports/02-plan.md`.

Use this planning template:

- Goal
- Bootstrap scope
- Allowed write paths
- Files to create
- Files to amend
- Files to leave untouched
- Merge strategy for existing agent files
- Native shim strategy
- Verification strategy
- Known ambiguities
- Approval gates triggered or not triggered

Planning rules:

- Keep the diff minimal.
- Keep durable knowledge in `.agent-os/`.
- Keep adapters thin and vendor-specific only where required.
- Prefer one canonical source of truth and many small pointers.
- If `CLAUDE.md`, `AGENTS.md`, or `.amazonq/rules/**` already exists, merge by adding a short “Agentify” section or import/pointer instead of replacing existing content.
- If root native files do not exist, create minimal ones.
- If the repo is a monorepo, produce one root map plus package or service subsections inside the same core docs. Do not explode the tree into dozens of files unless the repo structure obviously requires it.
- If no CI or tests exist, say so explicitly and plan around documentation-only verification.

If no approval gate is triggered, continue without waiting.

### Implement

Create the agent operating layer. Generate repo-specific content, not filler. Every generated file should be concise, accurate, and based on observed evidence.

#### Required core tree

Create or refresh this structure:

- `.agent-os/README.md`
- `.agent-os/project/system-overview.md`
- `.agent-os/project/architecture-map.md`
- `.agent-os/project/conventions.md`
- `.agent-os/project/boundaries.md`
- `.agent-os/project/entrypoints.md`
- `.agent-os/workflows/bootstrap-repo.md`
- `.agent-os/workflows/investigate.md`
- `.agent-os/workflows/plan.md`
- `.agent-os/workflows/implement.md`
- `.agent-os/workflows/verify.md`
- `.agent-os/workflows/review.md`
- `.agent-os/tasks/_task-packet.md`
- `.agent-os/tasks/_investigation-template.md`
- `.agent-os/tasks/_plan-template.md`
- `.agent-os/tasks/_review-template.md`
- `.agent-os/verification/commands.md`
- `.agent-os/verification/quality-gates.md`
- `.agent-os/memory/decisions.md`
- `.agent-os/memory/repo-notes.md`
- `.agent-os/memory/glossary.md`
- `.agent-os/bootstrap/reports/03-implement.md`

#### Core file content requirements

`.agent-os/README.md`

- one-page entrypoint for future agents and humans
- what this operating layer is
- where to start
- which files govern workflows, commands, boundaries, and memory
- repo type and primary stack summary
- a short “how to work here” sequence

`.agent-os/project/system-overview.md`

- purpose of the repo or system
- major runtime units, apps, services, packages, or jobs
- key data flows or request flows
- major external systems or integrations if evidenced
- critical invariants and assumptions

`.agent-os/project/architecture-map.md`

- top-level component map
- important directories and what they own
- dependency flow or layer flow
- monorepo package relationships if relevant
- hotspots and change-sensitive areas

`.agent-os/project/conventions.md`

- naming patterns
- code style only when repo-specific
- test conventions
- package and import conventions
- branching or PR conventions if evidenced
- documentation expectations
- do not restate generic language conventions unless the repo deviates from defaults

`.agent-os/project/boundaries.md`

- do-not-touch zones
- generated, vendored, or mirrored code
- migrations, infra, or externally owned directories
- stable public interfaces and contract surfaces
- areas requiring extra review or human sign-off

`.agent-os/project/entrypoints.md`

- startup entrypoints
- service launch paths
- CLI commands
- workers, jobs, schedulers, handlers
- compose or container entry commands if relevant
- note which entrypoints are high confidence versus inferred

`.agent-os/workflows/*.md`

Each workflow file must contain:

- Purpose
- Inputs
- Steps
- Output artefacts
- Stop conditions
- Handoff to the next workflow

Bootstrap the following workflows:

- `bootstrap-repo.md`
- `investigate.md`
- `plan.md`
- `implement.md`
- `verify.md`
- `review.md`

These workflows must align to the same lifecycle used by this skill.

`.agent-os/verification/commands.md`

Create a concise command catalogue grouped by confidence:

- High-confidence commands
- Medium-confidence commands
- Unconfirmed or missing commands

For each command, include:

- Purpose
- Command
- Scope or package if relevant
- Confidence
- Evidence source
- Notes or caveats

`.agent-os/verification/quality-gates.md`

Define default acceptance gates for future agent work, such as:

- scoped task packet exists
- plan exists for non-trivial changes
- commands selected from `commands.md`
- relevant tests, lint, typecheck, or build verification completed
- boundaries checked
- risks summarised
- docs updated if public behaviour changes
- decisions recorded if architecture or workflow changed

`.agent-os/memory/decisions.md`

Use this stable-decision format only for decisions that are durable and sufficiently evidenced:

    # Decisions

    ## YYYY-MM-DD — Short title
    - Status: proposed | accepted | superseded
    - Context:
    - Decision:
    - Evidence:
    - Consequences:
    - Review trigger:

Do not log transient guesses here.

`.agent-os/memory/repo-notes.md`

Use for working notes, ambiguities, and non-durable observations:

    # Repo notes

    ## YYYY-MM-DD
    - Observation:
    - Evidence:
    - Impact:
    - Follow-up:

`.agent-os/memory/glossary.md`

Use for domain language and important repo-specific terms:

    # Glossary

    ## Term
    - Meaning:
    - Where it appears:
    - Notes:

#### Task template examples

Create these templates with practical placeholders.

Example `_task-packet.md`:

    # Task packet

    - Requested outcome:
    - Why this matters:
    - In scope:
    - Out of scope:
    - Relevant paths:
    - Constraints:
    - Done when:
    - Verification:
    - Risks:
    - Open questions:
    - Expected summary format:

Example `_investigation-template.md`:

    # Investigation

    - Question:
    - Suspected areas:
    - Files inspected:
    - Commands and evidence:
    - Findings:
    - Risks or contradictions:
    - Recommendation:
    - Confidence:

Example `_plan-template.md`:

    # Plan

    - Goal:
    - Affected areas:
    - Minimal diff strategy:
    - Alternatives considered:
    - Verification plan:
    - Rollback or failure plan:
    - Dependencies or sequencing:
    - Approval needed:

Example `_review-template.md`:

    # Review

    - What changed:
    - What was verified:
    - Boundary check:
    - Contract compatibility:
    - Remaining risks:
    - Follow-up tasks:
    - Decision log updates:

#### Adapters

Create these thin adapters:

- `.adapters/claude/CLAUDE.md`
- `.adapters/amazonq/rules/00-agent-os.md`
- `.adapters/codex/AGENTS.md`
- `.adapters/cursor/AGENTS.md`

Adapter rules:

- Keep each adapter short.
- Adapters must point into `.agent-os/` and must not duplicate the full repo map.
- Adapters may mention vendor-specific entry behaviour, but the process itself must stay vendor-neutral.
- Adapters must instruct agents to:
  - read `.agent-os/README.md` first
  - use investigate → plan → implement → verify → review for non-trivial tasks
  - use `.agent-os/verification/commands.md` and `.agent-os/verification/quality-gates.md` before declaring work complete
  - record stable decisions in `memory/decisions.md`
  - record transient findings in `memory/repo-notes.md`

#### Native shims

Create or minimally amend these native entry files when useful:

- `CLAUDE.md`
- `AGENTS.md`
- `.amazonq/rules/00-agent-os.md`

Rules for native shims:

- Keep them minimal.
- Preserve existing content.
- Add pointers to `.agent-os/README.md` and the vendor adapter.
- For `CLAUDE.md`, use `@` imports if that keeps the file shorter and clearer.
- For `AGENTS.md`, create one root file that works as the shared entrypoint for tools that read `AGENTS.md`.
- Only create additional Cursor-native rule files if the repo already uses Cursor-specific files and a root `AGENTS.md` would clearly be insufficient.

#### Existing file merge policy

If you find existing `CLAUDE.md`, `AGENTS.md`, `.amazonq/rules/**`, `.cursor/**`, or `.claude/**` files:

- preserve the original intent
- add or merge a short “Agentify” section
- avoid rewriting working project conventions
- note any tension or overlap in the reports
- if there is a conflict between existing repo guidance and newly inferred guidance, prefer explicit existing repo guidance and record the conflict

#### Implement report

Write `.agent-os/bootstrap/reports/03-implement.md` with:

- files created
- files amended
- why each file exists
- merge decisions
- adapter strategy
- native shim strategy
- anything intentionally skipped

### Verify

Create `.agent-os/bootstrap/reports/04-verify.md`.

Verification here is for the bootstrap itself, not for product behaviour changes.

Perform these checks:

#### Scope check

Confirm that only allowed paths changed.

Use a diff-based scope review. If anything outside the allowed write scope changed, revert or undo it immediately and record the incident.

#### Existence and linking check

Confirm that all mandated files exist and that key links or imported paths point to real files.

At minimum verify:

- `.agent-os/README.md`
- all required core docs
- adapter files
- native shims if created
- report files

#### Evidence check

Confirm that every important command in `commands.md` has an evidence source.

Flag commands without evidence as provisional or remove them.

#### Thin-adapter check

Confirm that:

- `.agent-os/` contains the substance
- adapter files are short
- native shims are thin pointers rather than duplicated documentation

#### Safe command check

If the repo exposes high-confidence, non-destructive verification commands and permission settings allow them, run a small representative subset.

Examples of acceptable categories:

- lint
- typecheck
- targeted unit tests
- read-only build metadata commands
- documentation checks

Do not run commands that install packages, fetch network dependencies, mutate data, or talk to external systems.

If safe execution is not possible, record one of:

- blocked by permissions
- blocked by risk
- no high-confidence safe commands found
- repo has no runnable verification commands

#### Verify report

Write:

- what you checked
- which commands you ran or skipped
- command outcomes
- unresolved uncertainties
- whether the bootstrap meets acceptance

### Review

Create `.agent-os/bootstrap/reports/05-review.md`.

Use this self-review checklist:

- Did I preserve product logic and runtime behaviour?
- Did I avoid dependency and lockfile edits?
- Did I restrict writes to allowed paths?
- Are the generated files concise and repo-specific?
- Are commands evidence-backed and confidence-labelled?
- Are major risk areas, boundaries, and entrypoints captured?
- Is `.agent-os/` clearly the source of truth?
- Are adapters thin and portable?
- Did I preserve or minimally merge existing guidance?
- Are unknowns explicit rather than hidden?

Then assemble `.agent-os/bootstrap/reports/bootstrap-report.md` using the template below.

Also initialise memory sensibly:

- put only stable, durable, evidence-backed items into `memory/decisions.md`
- put ambiguities, observations, and follow-ups into `memory/repo-notes.md`
- put service names, domain terms, acronyms, and repo jargon into `memory/glossary.md`

## Bootstrap report template

Create `.agent-os/bootstrap/reports/bootstrap-report.md` using this shape:

    # Bootstrap report

    - Date:
    - Repository root:
    - Scope notes:
    - Repo type:
    - Primary languages:
    - Package managers and toolchains:
    - CI systems found:
    - High-confidence commands:
    - Medium-confidence commands:
    - Key entrypoints:
    - Key risk areas:
    - Existing guidance merged:
    - Files created:
    - Files amended:
    - Files intentionally not created:
    - Verification performed:
    - Verification skipped and why:
    - Ambiguities:
    - Recommended next tasks:
    - Acceptance summary:

For “Recommended next tasks”, suggest only the highest-value next steps, such as:

- tighten verification commands
- add repo-specific hooks
- add subdirectory-level instructions for packages or services
- refine boundaries after a human review
- add missing workflow rules where ambiguity remains

## Final checklist

Before you finish, confirm all of the following:

- [ ] Only the allowed write scope changed
- [ ] No product logic changed
- [ ] No production dependencies or lockfiles changed
- [ ] The five lifecycle phases were followed
- [ ] `.agent-os/` exists and is the substantive source of truth
- [ ] All required core docs were created or refreshed
- [ ] Thin adapters were created under `.adapters/`
- [ ] Native shims were created or minimally amended where useful
- [ ] Commands are evidence-backed and confidence-labelled
- [ ] Quality gates and task templates exist
- [ ] Memory files were initialised
- [ ] A final bootstrap report exists
- [ ] Ambiguities are explicit
- [ ] Suggested next tasks are included

## Final handoff

End with a concise human-facing summary that contains:

- what you detected
- what you created
- what you amended
- what you verified
- what remains ambiguous
- the top three next recommended tasks

If no approval gate was triggered, present the bootstrap as ready for human acceptance. If a gate was triggered, present the safest next action and wait.
