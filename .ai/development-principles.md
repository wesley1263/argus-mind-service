# Development Principles

> Audience: anyone (human or AI agent) proposing, planning, or landing a change in this repository.
> This document governs the *process*. See `coding-philosophy.md` for how code is written and
> `architecture.md` for where it lives. The human-readable walkthrough of this same lifecycle,
> with a diagram, is `docs/development-workflow.md`.

## 1. The Lifecycle

Every change of substance — a new capability, a change to a contract, a new Engine internal —
moves through the same lifecycle:

```
Spec → [ADR if irreversible/cross-Engine] → Plan → Implement → Definition of Done → Review → Merge
```

1. **Spec.** State the problem and the desired behavior in business terms (ubiquitous language),
   before any implementation detail. A spec that can't be written without inventing a new glossary
   term means the glossary needs updating first.
2. **ADR (conditional).** If the spec requires an irreversible or cross-Engine decision (Article
   VII of the Constitution), write the ADR before planning implementation. The ADR records *why*,
   not just *what* — future agents need the reasoning to know when the decision is safe to revisit.
3. **Plan.** Break the spec into concrete steps: which Engine(s) are touched, what contracts change,
   what tests prove the behavior. A plan that touches more than one Engine's internals in the same
   change is a signal the boundary is wrong — stop and reconsider before implementing.
4. **Implement.** Follow `coding-philosophy.md`. Implementation should not introduce behavior the
   spec didn't ask for.
5. **Definition of Done.** The author checks their own work against `definition-of-done.md` before
   requesting review. This is a self-check, not a formality — it catches most issues before a
   reviewer's time is spent.
6. **Review.** A reviewer (human or agent) checks the change against `review-checklist.md`.
7. **Merge.** Only after Definition of Done and Review both pass.

## 2. When an ADR Is Required

A decision needs an ADR if reversing it later would require any of:

- Changing a published Engine contract in a way that breaks an existing consumer.
- Migrating stored data with no safe rollback.
- Renaming or redefining a term in `/glossary/README.md`.
- Changing which Engine owns a concept.

If none of those apply, proceed straight from Spec to Plan. When genuinely unsure, write the ADR —
see Article VII of the Constitution.

## 3. Specs Live Close to What They Describe

A spec for a change inside a single Engine lives with that Engine's own documentation once `src/`
exists. A spec that affects a cross-Engine contract, the glossary, or the architecture lives
alongside the ADR that will accompany it. This repository does not yet have a `specs/` tree — it
will be introduced in the implementation phase, following the pattern set here.

## 4. Contracts Change Additively by Default

A published Engine contract (see `architecture.md` §2) may gain new optional fields or new
operations without an ADR, as long as every existing consumer keeps working unmodified. Any change
that could break an existing consumer — removing a field, changing a field's meaning, changing a
required parameter — requires an ADR and a explicit migration note for consumers.

## 5. The Glossary Changes in the Same Commit as Its First Use

If a change introduces a new business concept, the glossary entry is added in that same change,
not as a follow-up. A concept used in code before it exists in the glossary is not reviewable —
reject it in review (Article VI of the Constitution).

## 6. Testing Strategy

- **Unit tests** cover the functional core of a single Engine (see `coding-philosophy.md` §3) —
  fast, no I/O, no mocks needed because the core doesn't do I/O.
- **Contract tests** pin down the shape and meaning of what an Engine publishes, and run against
  both the producer and every consumer, so a breaking change fails CI at the boundary where it
  actually breaks something, not three Engines downstream.
- **Integration tests** exercise a single Engine's adapters (its real storage, real I/O) against
  its own ports.
- No test suite substitutes for a spec. A change with 100% coverage but no spec does not meet
  Definition of Done.

## 7. Branching and Commits

- Work happens on short-lived branches off the trunk; branch names describe the change, not the
  author or ticket number.
- Commits are scoped to one logical change. A commit that touches a contract and unrelated
  formatting in the same diff makes review and revert harder than they need to be.
- Commit messages explain *why*, matching the "why over what" principle used throughout this
  repository's documentation (names and diffs already say *what*).

## 8. Proposing a New Engine or Removing One

The five Engines named in the Constitution (Article III) are the platform's current
decomposition, not a permanently fixed number. Proposing a sixth Engine, splitting one, or merging
two is always an ADR-level decision: it must justify the new boundary in terms of ownership of a
distinct business concept and a distinct contract, following the same reasoning as ADR-001.

## 9. Documentation Is Part of the Change

A change that alters behavior a document describes (architecture, glossary, a workflow) is not
done until that document is updated. Stale documentation is treated as a bug.
