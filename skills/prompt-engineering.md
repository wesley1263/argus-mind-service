# Skill: Prompt Engineering

How prompts to generative AI models are written and governed inside Generation Engine. This is the
one place in the codebase where "prompt" is a first-class artifact — and it is still just an
implementation detail of Generation Engine, per Article I of `.ai/constitution.md`: nothing about
the domain model may depend on a specific prompt or vendor.

## Prompts Live in Generation Engine's Adapters, Never in Domain

A prompt template is I/O configuration for a specific generative model call — it belongs in
`generation_engine/adapters/`, not in `domain/` or `application/`. `application/` decides *what*
needs to be generated (a Generation Task: target Learning Node, difficulty, content type, per
`specs/generation-engine.md`); the adapter decides *how* to phrase that request to a specific model.
Swapping the underlying model or vendor should only ever touch this adapter layer.

## Every Prompt Is Versioned

Exactly like the confidence model in Learning State Engine (ADR-004), a prompt template has an
explicit version, and every `Generation Task` records which prompt version produced its output.
This is not optional: without it, "why did Generation Engine produce this?" has no answer, and
Validation failures (`specs/generation-engine.md`) can't be traced back to a specific prompt change
to know whether a regression came from the prompt or from the model itself.

## Prompts Are Tested Like Code

A prompt change is evaluated against the relevant `testing/golden-dataset.md` fixtures and scored
using the rubric in `testing/prompt-evaluation.md` **before** it's considered ready to ship — not
spot-checked informally. A prompt change that regresses the golden dataset's pass rate is treated
the same as any other regression: it blocks merge (`skills/github-actions.md`).

## Structure Every Prompt Around the Domain Contract

A generation prompt should be built from the same typed inputs `application/` already computed
(target Learning Node's label/description, Confidence, difficulty, content type) — never by
string-interpolating raw database rows or, worse, another Engine's internal representation. This
keeps the prompt itself reviewable as a function of well-defined inputs, and keeps Chunk
(Ingestion Engine's internal-only concept, ADR-002) from ever leaking into a prompt that a
downstream log or trace might expose.

## Validation Is Not Part of the Prompt

Do not rely on "asking the model nicely" (e.g., "please only output correct, safe content") as the
mechanism that satisfies Article V of the Constitution. Validation (`specs/generation-engine.md`,
ADR-005) is a separate, explicit step downstream of generation — a well-written prompt reduces how
often Validation fails, but it is never the thing Validation is trusted to be.

## Handling Non-Determinism

Generative model output is non-deterministic by nature. Because of that:

- Generation Engine's *decision* of what to generate (target Learning Node, difficulty — R1 in
  `specs/generation-engine.md`) must remain deterministic and testable without calling a model at
  all.
- Only the actual content-production step calls the model, and its output always passes through
  Validation before being trusted, regardless of how confident the model sounded.
- Tests that exercise prompt output directly belong in `testing/prompt-evaluation.md`'s domain
  (statistical, rubric-scored, dataset-driven) — not asserted with exact-string unit tests, which
  would be flaky by construction.

## Secrets and Vendor Credentials

Model API keys are configuration injected at the adapter boundary (see `skills/github-actions.md`
§Secrets and `skills/docker.md`), never hardcoded, never logged, and never part of a prompt template
committed to the repository.
