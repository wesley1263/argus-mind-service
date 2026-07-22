# Prompt Design Guide: AdaptiveContent (Shared Base Contract)

**Job**: the shared instruction set every specific content-type prompt (MindMap, Summary, Game,
Quiz, Reinforcement) inherits, so universal constraints are defined once and specific guides only
add what's actually specific to their format.

## The Shared Contract

Every AdaptiveContent-producing prompt, regardless of type, must:

1. Accept the standard `PromptContext` (`generation-engine-prompt.md`) and no other implicit inputs.
2. Use only `/glossary/README.md` terminology in any Student-visible copy the prompt produces —
   never expose internal terms (`Chunk`, `Embedding`, `PromptContext` itself) to the Student.
3. Respect the Difficulty value supplied — never soften it "to be helpful"
   (`docs/pedagogy/desirable-difficulties.md`).
4. Stay within a stated maximum content length/element count appropriate to Cognitive Load Theory
   (`docs/pedagogy/cognitive-load-theory.md`) — set per content type in that type's specific guide.
5. Never fabricate a fact not present in the supplied Learning Node/Knowledge Edge data.
6. Produce output in the exact structured shape Validation expects for that content type — free-form
   prose where structured data is expected is a Validation failure, not a stylistic variance.

## Why a Shared Base Exists

Without it, each content-type prompt would re-derive (and likely subtly re-word) the same
constraints, and a fix to one (e.g., a tone adjustment for "the child is never blamed") would need
to be manually propagated to five different prompts — exactly the kind of duplication
`.ai/coding-philosophy.md` §6 argues against, applied to prompts instead of code.

## How Specific Guides Extend This

Each of `mindmap-validation-prompt.md`, `summary-evaluation-prompt.md`,
`game-generation-prompt.md`, `quiz-generation-prompt.md`, and (for spoken input)
`audio-evaluation-prompt.md` states only what's genuinely specific to that format — its own
structured output shape, its own length/element bounds, and any pedagogy principle particularly
relevant to it (e.g., Dual Coding for MindMap).

## Related Documents

`docs/prompt-engineering/generation-engine-prompt.md`, `docs/domain-model/generation-entities.md`,
`docs/prompt-engineering/prompt-review-checklist.md`.
