# Workflow: Prompt Regression

- **Category:** AI Automation
- **Trigger:** `schedule` (nightly, against the currently-active production prompt/model — not just
  at PR time)
- **Blocking:** Advisory (files an incident-tracking issue; does not block anything retroactively
  since the prompt is already live)

## Purpose

`prompt-evaluation-workflow.md` catches a regression *at the moment a prompt changes*. This workflow
catches the other failure mode: the **underlying model provider** changes behavior for a prompt that
never changed on our side (a silent model version update, a provider-side system prompt change) —
exactly the risk Article I of `.ai/constitution.md` accepts in exchange for not hard-coupling the
domain to one vendor, so it needs its own explicit watch.

## Steps

1. Nightly, re-run the currently-active production prompt against the full `golden-dataset.md` for
   Generation Engine, exactly as `prompt-evaluation-workflow.md` would for a new prompt.
2. Compare against the score recorded when this prompt version was last accepted
   (`skills/prompt-engineering.md`'s versioning requirement makes "last accepted score" a concrete,
   stored value, not something inferred).
3. If any dimension has drifted beyond threshold with **no corresponding prompt change on our side**,
   this is model-provider drift, not a prompt regression in the usual sense — flag it distinctly so
   the response is "investigate the vendor," not "review our last prompt PR."

## Gate / Threshold

Same per-dimension threshold as `prompt-evaluation-workflow.md`, applied nightly instead of only at
change time. A regression here doesn't block a merge (nothing merged) — it opens/updates an incident
issue tagged `prompt-drift`.

## Inputs / Secrets

Same as `prompt-evaluation-workflow.md`.

## Outputs / Artifacts

Nightly score trend (feeds `workflows/repository-health.md`); incident issue on regression,
distinguishing "our prompt changed" (shouldn't happen on a schedule run) from "the model's behavior
changed under us."

## Failure Behavior

A detected regression triggers the same severity response as a production quality incident — per
Article V of `.ai/constitution.md`, content quality is a first-class concern, and Generation
Engine's Validation gate (`specs/generation-engine.md`) is the last line of defense, not the only
one; this workflow is what catches degradation before Validation's pass-rate itself starts visibly
dropping in production.

## Related Docs

`automation/ai-automation/prompt-evaluation-workflow.md`, `testing/prompt-evaluation.md`,
`skills/prompt-engineering.md`, ADR-005.
