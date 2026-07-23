---
name: Architecture Proposal
about: Propose a change to the system's structure — a new Engine, a contract change, a new cross-cutting concern
title: "[Architecture] "
labels: architecture
---

<!-- If accepted, the decision gets recorded as an ADR under adr/ — this issue tracks the proposal
and discussion, the ADR becomes the binding record. Run this past the `architect` subagent. -->

## Proposed Change

<!-- What's changing structurally — a new Engine, splitting/merging an existing one, a new
published contract, a new cross-cutting concern. -->

## Current State

<!-- What CLAUDE.md's Architecture section says today, and what's inadequate about it. -->

## Boundary Justification

<!-- If this proposes a new/changed Engine boundary: what distinct business concept does it own,
and what distinct contract does it publish? Same reasoning as ADR-001. -->

## Impact on Existing Contracts

<!-- Does this change what any existing Engine owns or publishes? Additive or breaking? Which
existing consumers are affected? -->

## Non-Negotiable Check

- [ ] Preserves Engine sovereignty — no new direct cross-Engine dependency introduced.
- [ ] Preserves Evidence-is-truth / Confidence-is-derived (ADR-003, ADR-004).
- [ ] Preserves the Validation gate (ADR-005).
- [ ] Is the simplest structure that satisfies the actual requirement — not generality for a
      hypothetical case.

## Alternatives Considered

## Recommendation

- [ ] Proceed as proposed
- [ ] Proceed, pending an ADR
- [ ] Reject — state which principle above it violates
