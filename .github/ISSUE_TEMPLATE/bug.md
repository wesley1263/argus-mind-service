---
name: Bug
about: Report a defect in existing behavior
title: "[Bug] "
labels: bug
---

<!-- Full field guidance: templates/bug.md -->

## Summary

<!-- One or two sentences: what's wrong, in terms of observable behavior. -->

## Severity

- [ ] Critical
- [ ] High
- [ ] Medium
- [ ] Low

## Affected Engine(s)

<!-- See .ai/architecture.md §2: Ingestion, Knowledge, Evidence, Learning State, Generation,
Platform, or Cross-Engine. -->

## Steps to Reproduce

1.
2.

## Expected vs. Actual

- **Expected:**
- **Actual:**

## Environment

<!-- Version/commit; whether this reproduces via `docker compose run --rm test` locally or only in
a deployed environment. -->

## Evidence

<!-- Logs, error output, relevant Evidence/Generation Task ids if this is Adaptive-Loop-related. -->

## Constitutional Check

- [ ] This bug involves Evidence integrity or Confidence correctness (ADR-003, ADR-004)
- [ ] This bug involves content reaching a Student without passing Validation (ADR-005)

<!-- Either box checked means this is a Constitutional issue, not an ordinary bug — flag for
priority triage per .ai/constitution.md Articles IV/V. -->
