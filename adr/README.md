# Architecture Decision Records

An ADR captures an irreversible or cross-Engine decision, the context that forced it, and the
alternatives that were rejected and why. The record is what lets a future change safely revisit a
decision instead of either blindly re-implementing it or blindly re-litigating it from zero context.

## When to Write One

Per `/.ai/development-principles.md` §2, a decision needs an ADR if reversing it later would
require any of:

- Changing a published Engine contract in a way that breaks an existing consumer.
- Migrating stored data with no safe rollback.
- Renaming or redefining a term in `/glossary/README.md`.
- Changing which Engine owns a concept.

When genuinely unsure, write the ADR — see Article VII of `/.ai/constitution.md`.

## Process

1. Copy `template.md` to `ADR-<next-number>-<kebab-case-title>.md`, using the next sequential
   number (zero-padded to 3 digits).
2. Fill in every section — Context, Decision, Consequences, Alternatives Considered.
3. Set `Status: Proposed` while under discussion; move to `Accepted` once it's settled and the
   corresponding Spec/Plan can proceed.
4. Add a row to the index below in the same change.
5. If a later ADR reverses or replaces an earlier one, the earlier one's status changes to
   `Superseded by ADR-XXX` — it is never deleted. The record of a decision, including decisions we
   later moved away from, is itself valuable history.

## Index

| ADR | Title | Status |
|---|---|---|
| [ADR-001](ADR-001-five-engine-modular-architecture.md) | Adopt a Five-Engine Modular Architecture Within One Deployable Service | Accepted |
| [ADR-002](ADR-002-chunk-is-an-internal-only-concept.md) | Chunk Is an Internal-Only Concept of the Ingestion Engine | Accepted |
| [ADR-003](ADR-003-evidence-is-an-immutable-append-only-log.md) | Evidence Is an Immutable, Append-Only Log | Accepted |
| [ADR-004](ADR-004-confidence-is-a-derived-recomputable-projection.md) | Confidence Is a Derived, Recomputable Projection — Never a Source of Truth | Accepted |
| [ADR-005](ADR-005-generation-output-must-pass-validation-gate.md) | All Generation Engine Output Must Pass Through a Validation Gate Before Reaching a Student | Accepted |
