# Prompt Design Guide: Learning State

**Job**: interpret **qualitative** Evidence (a `self_explanation` EvidenceType, in particular) into
a structured signal the deterministic confidence computation can use as one input — this prompt does
**not** compute or set Confidence itself. Confidence remains a pure, deterministic function of
structured signals (`.ai/coding-philosophy.md` §3); this prompt's job ends at producing one such
signal, honestly.

## Required Context (PromptContext)

- The self-explanation Evidence's structured claims (already segmented by
  `evidence-engine-prompt.md`).
- The target Learning Node's actual definition and its immediate Knowledge Edges, so the judgment is
  grounded in real content, not the model's own general knowledge of the subject.

## Instructions the Prompt Must Encode

1. For each claim in the self-explanation, judge whether it's consistent with the Learning Node's
   actual definition and relationships — output a structured per-claim judgment
   (`consistent` / `inconsistent` / `partial` / `unrelated`), not a single holistic score.
2. Explicitly flag **hedged or absent** claims about a prerequisite relationship as `partial`, not
   `inconsistent` — the distinction between "got it wrong" and "didn't address it" matters for
   Elaboration-based Feedback later (`docs/pedagogy/elaboration.md`).
3. Never output a numeric Confidence value directly — output only the structured per-claim judgment;
   the confidence model (owned entirely by Learning State Engine's deterministic code) converts that
   judgment into a Confidence delta, using a documented, versioned rule, not this prompt's own
   arithmetic.

## What Validation Must Check

That every judgment references an actual claim from the input and an actual fact about the Learning
Node — a judgment referencing neither is a hallucinated grounding and must fail Validation.

## Why This Split Matters (ADR-004 Compliance)

If this prompt directly output a Confidence number, a prompt/model change could silently and
arbitrarily shift every Student's Confidence trajectory with no auditable, versioned rule governing
the conversion — exactly what ADR-004 exists to prevent. Splitting "interpret the claim" (this
prompt, non-deterministic) from "convert judgment to Confidence" (deterministic code, versioned) is
what keeps the platform's central integrity guarantee intact even while using a generative model for
part of the pipeline.

## Related Documents

`specs/learning-state-engine.md`, `docs/domain/learning-state-engine.md`, ADR-004,
`docs/prompt-engineering/evidence-engine-prompt.md`, `docs/pedagogy/metacognition.md`.
