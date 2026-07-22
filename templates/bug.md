# Bug Template

- **Title:**
- **Reported by / Date:**
- **Severity:** Critical | High | Medium | Low
- **Affected Engine(s):** <see `.ai/architecture.md` §2 — narrowing this early speeds up triage>

## Summary

*One or two sentences: what's wrong, in terms of observable behavior.*

## Steps to Reproduce

1. …
2. …

## Expected vs. Actual

- **Expected:**
- **Actual:**

## Environment

*Version/commit, and whether this reproduces via `docker compose run --rm test` locally or only in
a deployed environment.*

## Evidence

*Logs, error output, relevant Evidence/Generation Task ids if this is Adaptive-Loop-related — enough
for someone else to investigate without reproducing it themselves first.*

## Constitutional Check

*If this bug involves incorrect Confidence, lost/mutated Evidence, or unvalidated content reaching a
Student, flag it explicitly — these are Article IV/V violations (`.ai/constitution.md`), not
ordinary bugs, and should be treated with corresponding urgency.*

- [ ] This bug involves Evidence integrity or Confidence correctness (ADR-003, ADR-004)
- [ ] This bug involves content reaching a Student without passing Validation (ADR-005)

## Root Cause

*Filled in after investigation — the actual mechanism, not just where the symptom appeared.*

## Fix Approach

*Filled in before the fix is implemented, following `prompts/implement-feature.md`'s workflow
(Red test reproducing the bug, then Green fix) — a bug fix without a test that would have caught it
isn't done, per `.ai/definition-of-done.md`.*
