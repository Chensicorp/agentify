# Workflow: investigate

## Purpose

Build an evidence-backed picture of a question, change area, or repository state before planning or implementing anything. Scan only — no product files are modified.

## Inputs

- A question or task description
- Relevant path hints (if known)
- `project/boundaries.md` — read first

## Steps

1. Read `project/boundaries.md` to understand no-go zones before touching anything.
2. Identify the relevant files, directories, and commands related to the question.
3. Read source files, manifests, CI, and docs — do not run commands unless they are read-only and in `verification/commands.md`.
4. Label findings as `evidenced`, `inferred`, or `provisional`.
5. Identify risks, contradictions, and unknowns explicitly.
6. Fill in `tasks/_investigation-template.md` (copy first).

## Output artefacts

- Filled investigation template (saved to `tasks/` or referenced inline)
- Updated `memory/repo-notes.md` if new ambiguities surface

## Stop conditions

- If investigation reveals the task would require modifying a boundary zone, stop and flag for human review.
- If evidence is so sparse that planning would be based entirely on guesses, note this explicitly before handing off.

## Handoff

Pass findings to the **plan** workflow. Include confidence labels and open questions.
