# Feature Template

> Use this to capture and triage a feature idea **before** it becomes a full specification. This is
> intentionally lighter than `specs/template.md` — it's the intake form, not the spec. Once a
> feature is prioritized, use `prompts/generate-specification.md` to turn it into a real spec in
> `/specs/`.

- **Title:**
- **Requested by:**
- **Date:**
- **Owning Engine(s) (best guess):** <one or more of Ingestion, Knowledge, Evidence, Learning
  State, Generation, Platform, or Cross-Engine — see `.ai/architecture.md` §2>

## Problem

*What's true today that shouldn't be, or what's missing? Ground this in the Student or platform
outcome, not the solution.*

## Desired Outcome

*What should be true once this exists? One or two sentences — this becomes the seed of the future
spec's Goals section.*

## Why Now

*What makes this worth prioritizing over other work — a specific pain point, a dependency for
something else, a stated Student/business need.*

## Rough Size

- [ ] Small (single Engine, no contract change)
- [ ] Medium (single Engine, contract change — likely needs an ADR)
- [ ] Large (cross-Engine orchestration or new Engine — needs `prompts/review-architecture.md`
      first)

## Next Step

- [ ] Promote to a full spec via `prompts/generate-specification.md`
- [ ] Needs an RFC first (`templates/rfc.md`) — genuinely open question about the right approach
- [ ] Rejected / deferred (state why below)

**Notes:**
