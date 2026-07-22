# Prompt: Create ADR

**Use when:** a decision has been identified as irreversible or cross-Engine
(`.ai/development-principles.md` §2) and needs to be recorded before implementation proceeds.
**Inputs required:** the decision being made, the context that forced it, and the alternatives
considered so far.
**Loads context from:** `adr/template.md`, `adr/README.md`, existing ADRs (especially any that
might already cover related ground), `.ai/constitution.md` Article VII.

```
Create an ADR for the following decision: {{decision_description}}.

First, confirm this actually needs an ADR (.ai/development-principles.md §2) — reversing it later
would have to require a breaking contract change, a data migration with no safe rollback, a
glossary rename, or a change in which Engine owns a concept. If none of those apply, say so and
recommend proceeding without an ADR instead of creating one reflexively.

Check adr/README.md's index for an existing ADR that already covers this ground or that this
decision would supersede. If one exists and this decision reverses it, mark the old one
"Superseded by ADR-XXX" rather than leaving it silently contradicted.

Copy adr/template.md to adr/ADR-{{next_number}}-{{kebab-case-title}}.md and fill in every section:
- Context: the forces in tension, stated honestly — include the constraint that's making this hard,
  not just the conclusion.
- Decision: specific enough that someone could implement it, or review an implementation against
  it, without a follow-up question.
- Consequences: both what gets easier AND what gets harder or becomes a new obligation. An ADR that
  only lists upside hasn't been reasoned through.
- Alternatives Considered: what else was on the table and specifically why it was rejected, so this
  isn't re-litigated from zero context later.

Use only terms already in /glossary/README.md. If this decision requires a new business term,
add it to the glossary in this same change, per Article VI.

Add a row to adr/README.md's index in the same change, with Status: Proposed (or Accepted if this
is being finalized now, not drafted for discussion).
```

**Output:** a filled-out ADR file following the template exactly, indexed in `adr/README.md`, with
honest trade-offs and a clear record of rejected alternatives.
