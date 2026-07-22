# Workflow: Architecture Validation

- **Category:** GitHub Actions
- **Trigger:** `pull_request` (all branches)
- **Blocking:** Required check

## Purpose

Runs the `testing/architecture-testing.md` suite as its own distinct, clearly-labeled CI job — even
though it's technically part of `docker compose run --rm test` — so a Constitutional boundary
violation (`.ai/constitution.md` Articles III–V) is immediately visible as its own failed check,
not buried inside a generic "tests failed" result.

## Steps

1. Run the architecture-test suite specifically (`tests/architecture/`, per
   `testing/architecture-testing.md`) — this can be the same underlying test run as `tests.md`, but
   reported as a separately-named check.
2. For each rule in `testing/architecture-testing.md`'s enforcement table (no cross-Engine imports,
   `Shared Kernel` dependency direction, Chunk containment, no direct Confidence writes, Evidence
   immutability grants, the Validation gate, framework-import layering), report pass/fail
   individually, not as one aggregate boolean.
3. If any rule fails, identify the specific offending file/import/grant so the fix is obvious
   without re-deriving which of the ~7 rules broke.

## Gate / Threshold

100% of the rules in `testing/architecture-testing.md`'s table pass. This check has no tiered
severity — every rule here exists because it encodes a Constitutional Article, and
`.ai/review-checklist.md`'s stance is that these are never approved "with a follow-up."

## Inputs / Secrets

None.

## Outputs / Artifacts

Per-rule pass/fail report as a PR status check and job summary table.

## Failure Behavior

Blocks merge, unconditionally. Unlike `dependency-scan.md`, there is no accepted-exception path —
a genuine need to cross a boundary means the boundary itself needs an ADR
(`.ai/development-principles.md` §2), not a suppressed check.

## Related Docs

`testing/architecture-testing.md`, `.ai/constitution.md`, `.ai/architecture.md` §4.
