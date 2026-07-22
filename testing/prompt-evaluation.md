# Prompt Evaluation

## Purpose

`skills/prompt-engineering.md` establishes that a prompt change is evaluated before it ships, not
spot-checked informally. This document is the evaluation method itself: how a Generation Engine
prompt (or the model/technique behind it) is scored against `golden-dataset.md` before being
considered safe to activate.

## Relationship to Validation

Prompt Evaluation and Validation (`specs/generation-engine.md`, ADR-005) are not the same thing,
and it matters that they stay distinct:

- **Validation** runs on every single piece of content, in production, before it can reach a
  Student — it's a binary, real-time gate.
- **Prompt Evaluation** runs on a curated dataset, offline, before a prompt change is shipped at
  all — it's a statistical, pre-release quality measure over many examples at once.

A prompt with a high Prompt Evaluation score still has every individual output pass through
Validation in production; a high evaluation score is evidence a prompt is *likely* to produce
Validation-passing content often, not a substitute for checking each one.

## Evaluation Dimensions

Each golden dataset scenario's output is scored along dimensions that mirror what Validation itself
checks (`specs/generation-engine.md`), so a regression here predicts a Validation regression before
it happens in production:

| Dimension | Question |
|---|---|
| Correctness | Is the content factually accurate relative to the Knowledge Graph it targets? |
| Difficulty match | Does it match the requested difficulty for the target Learning Node? |
| Pedagogical soundness | Does it actually help build the targeted Learning Node's understanding, not just superficially mention it? |
| Safety | Is it free of unsafe, inappropriate, or off-topic content? |
| Language/tone consistency | Does it use the platform's ubiquitous language correctly where relevant (e.g., not exposing internal jargon to a Student)? |

## Scoring

Each dimension is scored on a fixed rubric scale (e.g., 1–5) by a reviewer (human, or a
model-based judge whose own outputs are periodically spot-checked by a human — a model judge is
itself just another Generation-Engine-adjacent output subject to the same "don't blindly trust
generative output" discipline as everything else in this project). Scores are aggregated per
dimension across the whole dataset, not just averaged into one meaningless composite number — a
prompt that trades correctness for tone consistency should be visible as exactly that trade, not
hidden inside a single score.

## Gating a Prompt Change

A prompt (or underlying model) change:

1. Runs against the full relevant golden dataset.
2. Is compared dimension-by-dimension against the currently-active prompt's last recorded scores.
3. Blocks merge (`skills/github-actions.md`'s `golden-dataset-eval` job) if any dimension regresses
   past a configured threshold, with the specific regressed scenarios surfaced for review — not
   just a pass/fail verdict.
4. On acceptance, the new prompt's version and its scores are recorded together
   (`skills/prompt-engineering.md`'s versioning requirement), so the score history itself becomes
   the audit trail for why a prompt was considered good enough to ship.

## What This Does Not Replace

Prompt Evaluation does not replace `golden-dataset.md`'s curation discipline (the dataset itself
must stay representative and correctly labeled) or Validation's real-time gate. It's the missing
middle layer: proving a change is *probably* good, at scale, before trusting Validation to catch the
individual cases that slip through.
