# Spike Template

> Use this for time-boxed research or investigation needed before a spec can be written responsibly
> — e.g., "can our extraction approach handle scanned PDFs at all?" A spike produces a recommendation
> and, usually, a spec or RFC as its output — it does not itself implement anything durable.

- **Question to answer:**
- **Timebox:** <e.g. 2 days — a spike without a timebox tends to quietly become unscoped
  implementation work>
- **Owner:**
- **Date:**

## Why This Needs a Spike

*What's unknown that blocks writing a spec responsibly right now — a technical unknown, a
feasibility question, an unfamiliar library/technique.*

## Approach

*What you actually tried — briefly. A spike's code is throwaway by default; note explicitly if any
part of it is intended to be kept.*

## Findings

*What you learned, stated as facts, separate from your opinion about what to do with them.*

## Recommendation

*What should happen next, given the findings. This is the actual point of the spike — a spike that
ends without a recommendation didn't finish its job.*

## Follow-up

- [ ] Promote to a spec (`prompts/generate-specification.md`)
- [ ] Needs an ADR (`prompts/create-adr.md`) — findings revealed an irreversible/cross-Engine
      decision
- [ ] Dead end — documented here so it isn't re-attempted without this context
