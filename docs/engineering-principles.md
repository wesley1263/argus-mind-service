# Engineering Principles

> This is the short, human-readable distillation of "how we build." Every principle here is
> enforced in more detail elsewhere — this page exists so a new reader (or a new agent session)
> can get the shape of it in five minutes, then go deeper via the links.

## 1. Specification Before Code

We don't write code to discover what we want. We write down what we want, in the platform's own
business language, and only then write code that satisfies it. This isn't bureaucracy for its own
sake — it's what makes it possible for many independent AI agent sessions, with no shared memory
between them, to build one coherent system instead of five incompatible ones.
→ `/.ai/development-principles.md`, `development-workflow.md`

## 2. The Methodology Is the Product, the AI Is a Tool

Nothing about how a Learning Node, Evidence, or Confidence is defined may depend on which AI model
or vendor happens to power Generation Engine today. If a design decision would make that swap
harder, the decision is wrong, not the constraint.
→ `/.ai/constitution.md` Article I, `vision.md`

## 3. Five Engines, Real Boundaries

Ingestion, Knowledge, Evidence, Learning State, and Generation each own their own model and expose
only a small, deliberate contract to everyone else. A shortcut that reaches across that boundary
"just this once" is the single most common way large systems rot, and it's rejected in review
every time, regardless of how small it looks.
→ `/.ai/architecture.md`, `architecture-overview.md`

## 4. Evidence Is Truth; Everything Else Is Derived

The only thing this platform writes and simply trusts is Evidence — an honest record of what a
Student actually did. Learning State and Confidence are always computed from it, never edited
directly, so any belief the system holds about a Student can always be traced back to something
that really happened.
→ `/.ai/constitution.md` Article IV, ADR-003, ADR-004

## 5. Nothing Reaches a Student Unvalidated

Generated content is powerful and also fallible. It passes through an explicit Validation gate
every time, with no bypass, regardless of how the content was produced or how confident the
generation step was.
→ `/.ai/constitution.md` Article V, ADR-005

## 6. One Language, Used Precisely

Every business term means exactly one thing, defined once, in `/glossary/README.md`, and used
identically in specs, code, tests, and conversation. A term used loosely in a spec becomes a bug in
code three steps later.
→ `/.ai/constitution.md` Article VI, `/glossary/README.md`

## 7. Simplicity Is Enforced, Not Assumed

We build what the current spec requires and stop there. Abstractions, configuration flags, and
"just in case" flexibility are added when a real second case shows up, not before. This applies
exactly as much to an AI agent generating code as to a human writing it by hand.
→ `/.ai/constitution.md` Article VIII, `/.ai/coding-philosophy.md` §6

## 8. Irreversible Decisions Get Written Down

Anything that would be expensive or risky to reverse later — a contract shape, a glossary term, an
Engine boundary — gets an ADR explaining the reasoning, not just the outcome. Future decisions get
to build on that reasoning instead of re-litigating it from scratch.
→ `/.ai/constitution.md` Article VII, `/adr/README.md`

## 9. Two Different Gates, on Purpose

An author checks their own work against a Definition of Done before asking for review. A reviewer
independently checks it again against a Review Checklist. The two documents look similar but exist
to catch different blind spots — the author's and the reviewer's are rarely the same.
→ `/.ai/definition-of-done.md`, `/.ai/review-checklist.md`

## 10. Documentation Drift Is a Bug

If a change makes a document inaccurate, the document is fixed in the same change, not filed as a
follow-up. A repository that many independent AI sessions will read has zero tolerance for docs
that quietly stopped being true.
→ `/.ai/development-principles.md` §9
