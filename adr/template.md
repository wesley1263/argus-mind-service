# ADR-XXX: <Short, decision-oriented title>

> Copy this file to `ADR-<next-number>-<kebab-case-title>.md`, fill in every section, and add a row
> to `/adr/README.md`. See `/.ai/development-principles.md` §2 for when an ADR is required.

- **Status:** Proposed | Accepted | Superseded by ADR-XXX | Deprecated
- **Date:** YYYY-MM-DD
- **Deciders:** who made this call

## Context

What situation makes a decision necessary? What forces are in tension (e.g., flexibility vs.
simplicity, correctness vs. latency)? State this in ubiquitous-language terms where possible, and
be honest about constraints — this section is what lets a future reader judge whether the reasoning
still holds.

## Decision

State the decision itself, in one or two sentences, followed by whatever elaboration is needed to
make it actionable. Be specific enough that someone could implement it, or review an implementation
against it, without asking a follow-up question.

## Consequences

What becomes easier? What becomes harder? What new obligations does this decision create (things
that must now be kept true elsewhere in the system)? Include the honest negatives, not just the
justification — an ADR that only lists upside wasn't reasoned through.

## Alternatives Considered

What else was on the table, and specifically why it was rejected. This is what prevents the same
alternative from being re-proposed and re-rejected from scratch a year later without anyone
remembering why.
