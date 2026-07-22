# Workflow: Documentation

- **Category:** GitHub Actions
- **Trigger:** `pull_request` (all branches)
- **Blocking:** Required check

## Purpose

Enforces `.ai/development-principles.md` §9 ("Documentation Is Part of the Change") and
`.ai/definition-of-done.md`'s documentation item mechanically: a PR that changes behavior a document
describes must update that document in the same change.

## Steps

1. Diff the PR against its base to identify which `src/argus/<engine>/` paths changed.
2. Map changed paths to the docs that describe them: a change under an Engine's `adapters/http/`
   should correlate with an API-section update in the relevant `specs/*.md`
   (`automation/github-actions/specification-validation.md` does the deeper contract-level check;
   this workflow does a lighter-weight "did *any* doc move" heuristic check); a change to
   `.ai/architecture.md` §2's ownership table without a corresponding ADR is flagged.
3. If code changed under a path with a mapped doc and no file under `specs/`, `docs/`, `.ai/`, or
   `glossary/` changed in the same PR, flag it for human confirmation rather than hard-failing —
   this check has false positives (some code changes genuinely need no doc update) and is
   deliberately advisory-leaning on ambiguous cases while still blocking the clear ones (see Gate).
4. Verify every internal markdown link changed or added in the PR resolves to a real file (the same
   check performed manually during Phase 1–3 authoring, now automated).

## Gate / Threshold

Hard block: any broken internal markdown link introduced by the PR. Soft block (requires an
explicit "no doc update needed" acknowledgment in the PR description, not a silent override): code
changed under a mapped path with no corresponding doc change.

## Inputs / Secrets

None.

## Outputs / Artifacts

PR comment listing which docs likely need updating and why; broken-link report.

## Failure Behavior

Broken links always block. The "likely needs a doc update" case blocks until the author either
updates the doc or adds the acknowledgment — this keeps the check honest without becoming
unbypassable friction for the genuine exceptions.

## Related Docs

`.ai/development-principles.md` §9, `.ai/definition-of-done.md`,
`automation/github-actions/specification-validation.md`.
