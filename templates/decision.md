# Decision Log Template

> Use this for a smaller, generally **reversible** decision that's still worth writing down — a team
> convention, a library choice internal to one Engine, a naming call. If the decision would be
> expensive to reverse later (breaks a contract, needs a data migration with no rollback, renames a
> glossary term, changes Engine ownership), it meets the bar in
> `.ai/development-principles.md` §2 and belongs in `/adr/` instead — use `prompts/create-adr.md`.
> When genuinely unsure which one applies, prefer the ADR; the cost of over-documenting is much
> lower than an undocumented irreversible call (`.ai/constitution.md` Article VII).

- **Decision:**
- **Date:**
- **Owner:**
- **Scope:** <which Engine or area this applies to>

## Context

*What prompted this — brief, a paragraph at most. This is not an ADR's full Context section; if you
need that much rigor, this decision probably belongs in `/adr/`.*

## Reasoning

*Why this option over the obvious alternative(s), briefly.*

## Reversibility

*Confirm explicitly why this is safely reversible — what it would take to change this decision
later, and why that's cheap. If it turns out not to be cheap, stop and use `prompts/create-adr.md`
instead.*
