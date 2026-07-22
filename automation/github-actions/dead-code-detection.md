# Workflow: Dead Code Detection

- **Category:** GitHub Actions
- **Trigger:** `pull_request` (all branches), `schedule` (weekly, full-repo sweep)
- **Blocking:** Advisory on PR; advisory (tracked, not blocking) on schedule

## Purpose

Flags code that's no longer reachable — a natural byproduct of an AI-agent-heavy codebase where a
refactor (`prompts/refactor.md`) can leave an old implementation path unreferenced without anyone
noticing, contradicting `.ai/coding-philosophy.md`'s "small, explicit, boring" principle (dead code
is the opposite of small).

## Steps

1. Run static unreachable-code analysis (e.g. `vulture` for Python) scoped per Engine package, so
   findings are attributed to the Engine that owns them.
2. Cross-reference findings against the test suite: code with zero test references and zero
   production call sites is a stronger finding than code only missing a caller (which might be a
   public port implementation used via dependency injection and legitimately not "called" from a
   static analyzer's view — filter these out using `skills/clean-architecture.md`'s ports/adapters
   pattern to reduce false positives).
3. On PR: flag any *newly introduced* dead code (a function added but never called within the same
   PR) as advisory.
4. On schedule: sweep the whole codebase and file/update one tracking issue per Engine
   (`templates/bug.md` shape, tagged `dead-code`) summarizing accumulated findings.

## Gate / Threshold

Advisory only — this workflow never blocks a merge, because static dead-code analysis has enough
false positives (dynamic dispatch, port implementations, deliberately-staged code for a
near-term next PR) that hard-blocking would train contributors to ignore it. It stays valuable by
staying accurate, not by being mandatory.

## Inputs / Secrets

None.

## Outputs / Artifacts

PR comment with newly introduced dead code (if any); weekly per-Engine tracking issues.

## Failure Behavior

Never blocks. Feeds `workflows/code-metrics.md` and `workflows/repository-health.md` as a tracked
trend rather than a gate.

## Related Docs

`.ai/coding-philosophy.md` §9, `skills/clean-architecture.md`, `templates/bug.md`,
`automation/workflows/code-metrics.md`.
