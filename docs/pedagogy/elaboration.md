# Elaboration

## What It Is

Elaboration is the practice of connecting new information to existing knowledge by explaining,
justifying, or asking "how" and "why" it's true, rather than encoding it as an isolated fact. It
produces richer, more interconnected memory traces than simple repetition of the fact alone.

## Scientific Evidence

Craik & Lockhart's (1972) *levels of processing* framework proposed that "deeper," more meaningful
processing (relating new information to existing knowledge, extracting meaning) produces more
durable memory than "shallow" processing (surface features like sound or appearance). Building on
this, **elaborative interrogation** research (notably Pressley and colleagues through the 1980s–90s)
found that prompting learners with "why is this true?" or "why would this make sense?" questions
during study measurably improves retention of factual material compared to simple restudying,
particularly when the learner already has some related background knowledge to draw the connection
from.

## Why Smart App Uses It

Elaboration is what turns an isolated Learning Node into part of a Student's actual mental model,
consistent with the Core Product Thesis's emphasis on "mental model" over "fact recall"
(`docs/domain/product-philosophy.md`). It's also the mechanism that makes the Knowledge Graph's
prerequisite structure pedagogically useful, not just organizationally useful: asking "why does this
depend on that" is elaboration applied directly to a Knowledge Edge.

## Where It Appears in the Product

"Why" and "how does this connect to X" prompts attached to Quiz and Summary content, deliberately
referencing a Learning Node's prerequisite or related nodes from the Knowledge Graph, not just the
node in isolation.

## Engineering Implications

Generation Engine's content-generation logic should have access to a target Learning Node's
immediate neighborhood (`specs/knowledge-engine.md`'s neighborhood query) specifically so it can
construct elaborative connections to real, related content — an elaboration prompt referencing a
fabricated or irrelevant connection would actively mislead the Student about the Knowledge Graph's
actual structure.

## Product Implications

Feedback (`docs/domain-model/generation-entities.md`) after an incorrect or incomplete response
should, where possible, explain *why* the correct answer is correct in relation to something the
Student already has some Confidence in — elaborative feedback, not just corrective feedback.

## Prompt Implications

Prompts generating elaborative content should be given the target Learning Node's real prerequisite
and related nodes from `specs/knowledge-engine.md`'s contract as explicit context, and instructed to
build the "why" connection from that real structure rather than inventing a plausible-sounding but
unverified connection — a case where `docs/prompt-engineering/prompt-review-checklist.md`'s
factual-grounding check applies directly.

## Related Documents

`docs/pedagogy/feynman-technique.md`, `docs/domain/knowledge-engine.md`,
`docs/domain-model/generation-entities.md` (Feedback), `docs/prompt-engineering/prompt-review-checklist.md`.
