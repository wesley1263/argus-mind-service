# Constitution

> Status: **Ratified**
> Applies to: every human and every AI agent that writes to this repository
> Precedence: this document outranks every other document in this repository. Where any other
> document conflicts with the Constitution, the Constitution wins and the other document is wrong
> and must be fixed.

## Preamble

This repository, `argus-mind-service`, is the engineering core of **Smart App**, an Adaptive
Learning Platform. Smart App is not an "AI education app." It is a methodology — a disciplined,
evidence-based way of modeling what a Student knows and deciding what they should encounter next —
implemented as software. The AI techniques used inside Generation Engine are a means to an end,
replaceable at any time. The methodology, expressed through the five Engines (Ingestion,
Knowledge, Evidence, Learning State, Generation) and the contracts between them, is the enduring
product.

This Constitution exists because this repository will be built, extended, and refactored largely
by AI coding agents operating over long time horizons, often without a human re-explaining context
on every task. The Constitution is the part of the "operating system" that does not change often.
It states the non-negotiables so that every future decision — human or agent-made — can be checked
against a fixed reference instead of re-derived from vibes.

If you are an AI agent reading this before generating code or specs: everything below is binding.
Where you believe a request from a task description conflicts with this Constitution, stop and
surface the conflict instead of resolving it silently.

---

## Article I — The Methodology Is the Product

The adaptive learning methodology — how Evidence becomes Learning State, and how Learning State
becomes the next right thing to show a Student — is the asset this company is building. The
specific model, prompt, or library used inside the Generation Engine is an implementation detail
and must always remain swappable without changing the meaning of Learning Node, Confidence,
Evidence, or Validation.

**Binding rule:** no document, spec, or piece of code may hard-couple the platform's domain model
(the ubiquitous language in `/glossary/`) to a specific AI vendor, model, or library. Domain
concepts are defined in business terms, never in terms of an API response shape.

## Article II — Specification-Driven Development Is Mandatory

No non-trivial code is written before a specification exists. The lifecycle is:

**Spec → (ADR, if the decision is irreversible or cross-Engine) → Plan → Implement → Verify against
Definition of Done → Review against Review Checklist → Merge.**

A pull request that implements behavior not traceable to a spec, or that silently changes a
contract between Engines without a corresponding ADR, does not meet the Definition of Done,
regardless of code quality. See `development-principles.md` for the full lifecycle and
`definition-of-done.md` / `review-checklist.md` for the gates.

## Article III — The Five Engines Are Sovereign Boundaries

The platform is decomposed into exactly five Engines: **Ingestion, Knowledge, Evidence, Learning
State, Generation.** Each Engine:

1. Owns its own domain model and internal concepts (e.g., Chunk belongs to Ingestion alone).
2. Exposes a small, explicit, versioned contract to the rest of the system and consumes only the
   published contracts of other Engines — never their internals.
3. May be extracted into an independently deployable service later without changing its contract.

No Engine may reach into another Engine's storage, internal types, or private concepts. Crossing a
boundary except through a published contract is a Constitutional violation, not a style
preference, and must be rejected in review regardless of how convenient the shortcut is. See
`architecture.md` for the full contract map.

## Article IV — Evidence Is Truth, Confidence Is Derived

Evidence is the only thing in this system that is written directly and trusted as fact. Learning
State — including Confidence — is always a computed projection over Evidence and is never written
to directly by any actor, including internal tools, support staff, or migrations. Corrections to a
Student's trajectory happen by recording new Evidence, never by editing a derived value. See
ADR-003 and ADR-004.

**Binding rule:** if a future feature seems to require writing directly to Learning State or
Confidence, that is a signal the feature is misdesigned, not a signal to add a write path. Model
the correction as Evidence instead.

## Article V — Nothing Reaches a Student Without Validation

Generation Engine output is generated content and generated content can be wrong, unsafe, or
pedagogically unsound. No content produced for a Student — a question, an explanation, an
exercise, feedback — may be delivered without first passing Validation as an explicit, auditable
step. A Validation failure must produce a fallback or a retry, never a silent bypass. See ADR-005.

## Article VI — Ubiquitous Language Is Law

Every business concept used in specs, ADRs, code, tests, logs, and comments must match its
definition in `/glossary/README.md` exactly. If a concept doesn't exist in the glossary yet, the
glossary is updated in the same change before the concept is used elsewhere. Internal-only
concepts (currently: Chunk) must never leak into a public contract, an API, a log line visible to
a Student, or a cross-Engine boundary.

Renaming a glossary term is a breaking change to the shared mental model of the whole codebase and
requires an ADR, not a find-and-replace.

## Article VII — Every Irreversible Decision Gets an ADR

A decision is irreversible in this repository's sense if reversing it later would require a
cross-Engine contract change, a data migration with no safe rollback, or re-litigating a term in
the glossary. Examples: choosing what a Learning Node contains, choosing how Confidence is
computed, choosing the boundary of an Engine. Reversible decisions (a function's internal
algorithm, a variable name, a library used entirely inside one Engine's private implementation) do
not need an ADR. When in doubt, write the ADR — the cost of an unnecessary ADR is far lower than
the cost of an undocumented irreversible decision. See `/adr/README.md`.

## Article VIII — Simplicity Is a Requirement, Not a Preference

Build the thing that is needed now, correctly and to spec, and no more. Do not add abstraction,
configuration, or generality for imagined future requirements. A future requirement gets a future
spec. This applies equally to humans and AI agents: an agent that "helpfully" generalizes beyond
the spec has violated this Article. See `coding-philosophy.md`.

## Article IX — This Document Only Changes By Amendment

The Constitution is not edited casually. A change to this document requires its own ADR explaining
what changed and why, and the amendment is recorded at the bottom of this file. Every other
document in `.ai/` and `docs/` must remain consistent with the Constitution; if a change here
invalidates something elsewhere, that other document is updated in the same change.

---

## Amendment Log

| Date       | Change                        | ADR |
|------------|--------------------------------|-----|
| 2026-07-21 | Initial ratification (Phase 1: Foundation) | — |
