# Prompt Design Guide: Game Generation

**Job**: generate a Game (`docs/domain-model/generation-entities.md`) — the content type whose
primary purpose is engagement (`docs/domain-principles.md` §10, "fun creates habit"), while still
targeting a real Learning Node.

## Required Context

Standard `PromptContext`; a selection from a fixed set of supported game mechanics (matching,
sorting, sequencing — chosen for their compatibility with Motor Encoding interaction,
`docs/pedagogy/motor-encoding.md`) rather than an open-ended "invent a game" instruction, which
would be far harder to Validate reliably.

## Instructions the Prompt Must Encode

1. Every game element must map to real content — a matching game's pairs must be genuine
   Learning-Node-grounded facts/terms, not filler.
2. Keep element count within Cognitive Load Theory bounds for the mechanic chosen — a sorting game
   with too many categories defeats its own purpose.
3. Difficulty is expressed through the *mechanic's* parameters (number of distractors, time
   pressure if applicable) calibrated to the supplied Difficulty value, not through obscuring
   correct content.
4. No "trick" content — a Game's difficulty should come from genuine retrieval demand
   (`docs/pedagogy/retrieval-practice.md`), not from ambiguous or misleading framing, which would
   undermine trust rather than build durable learning.

## What Validation Must Check

That every game element traces to real Learning Node/Knowledge Edge content (same grounding
requirement as every other content type); mechanic-specific structural well-formedness (e.g., a
sorting game's categories are mutually exclusive).

## Related Documents

`docs/domain-principles.md` §10–11, `docs/pedagogy/motor-encoding.md`,
`docs/prompt-engineering/adaptive-content-prompt.md`.
