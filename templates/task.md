# Task Template

> The smallest unit of tracked work — one task is normally one Red/Green cycle
> (`.ai/development-principles.md` §1). Generated in bulk by `prompts/generate-tasks.md` from a
> spec; this template shows the shape each task should take.

- **Description:** <one sentence, action-oriented — "Add `KnowledgeGraphRepository.get_neighborhood`
  port and Postgres adapter">
- **Spec:** `specs/<name>.md`
- **Serves Acceptance Criterion:** <e.g. AC5>
- **Layer:** Migration | Domain | Application | Port | Adapter | Contract Test | Integration
  (see `skills/clean-architecture.md` for what belongs in each)
- **Owning Engine:**
- **Depends on:** <another task, or "none">
- **Size:** XS | S | M | L (an L task is usually a sign it should be split further)
- **Status:** Todo | In Progress | Done

## Notes

*Anything a different implementer (or a future session with no memory of this conversation) would
need to pick this up cold: a gotcha, a decision made while scoping it, a link to the relevant
`skills/*.md` convention.*
