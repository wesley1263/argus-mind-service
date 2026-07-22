---
name: Architecture Proposal
about: Propose a change to the system's structure — a new Engine, a contract change, a new cross-cutting concern
title: "[Architecture] "
labels: architecture
---

<!-- This is the intake form for prompts/review-architecture.md. If accepted, the decision itself
gets recorded via prompts/create-adr.md — this issue tracks the proposal and discussion, the ADR
becomes the binding record. -->

## Proposed Change

<!-- What's changing structurally — a new Engine, splitting/merging an existing one, a new
published contract, a new cross-cutting concern (e.g. a new supporting subdomain like
Authentication). -->

## Current State

<!-- What .ai/architecture.md §2/§4 says today, and specifically what's inadequate about it that
motivates this proposal. -->

## Boundary Justification

<!-- If this proposes a new/changed Engine boundary: what distinct business concept does it own,
and what distinct contract does it publish? (.ai/development-principles.md §8, following the same
reasoning as ADR-001.) -->

## Impact on Existing Contracts

<!-- Does this change what any existing Engine owns or publishes? Is it additive or breaking
(.ai/development-principles.md §4)? Which existing consumers are affected? -->

## Constitutional Check

- [ ] Preserves Engine sovereignty (Article III) — no new direct cross-Engine dependency introduced.
- [ ] Preserves Evidence-is-truth / Confidence-is-derived (Article IV, ADR-003, ADR-004).
- [ ] Preserves the Validation gate (Article V, ADR-005).
- [ ] Is the simplest structure that satisfies the actual requirement (Article VIII) — not
      generality for a hypothetical case.

## Alternatives Considered

## Recommendation

- [ ] Proceed as proposed
- [ ] Proceed, pending an ADR (`prompts/create-adr.md`)
- [ ] Reject — state which principle above it violates
