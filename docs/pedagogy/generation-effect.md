# Generation Effect

## What It Is

The generation effect is the finding that information is remembered better when a learner generates
it themselves — completing a word from a fragment, producing an answer, filling a gap — compared to
being given the same information to simply read. The effort of production itself strengthens the
memory trace, beyond whatever benefit comes from attention or time-on-task alone.

## Scientific Evidence

Slamecka & Graf (1978) established the effect experimentally: participants who generated target
words from cues (e.g., completing "rapid : f___" with "fast") recalled them significantly better
later than participants who simply read the complete pairs. The effect has been replicated across
many materials and generation tasks since, and is closely related to — but experimentally distinct
from — the testing effect (`docs/pedagogy/active-recall.md`): generation effect studies typically
involve producing information *during* initial learning, not just retrieving already-learned
information later.

## Why Smart App Uses It

This is the effect Generation Engine is literally named for. The platform's default posture —
having the Student produce an answer, a completed structure, or an explanation rather than consuming
a finished one — is a direct application: every content type that asks the Student to fill something
in, complete a partial structure, or answer before seeing an explanation is exploiting this effect
deliberately.

## Where It Appears in the Product

Quiz questions (produce an answer); MindMap completion tasks (produce missing relationships/nodes);
"predict before you're told" framing in Summary-adjacent content, where a Student is asked to guess
an outcome or definition before the system confirms or corrects it.

## Engineering Implications

Content generation logic should default to a "produce, then confirm" structure over a "present,
then optionally quiz" structure wherever a content type supports it — this is a concrete ordering
decision Generation Engine's content templates should encode explicitly
(`specs/generation-engine.md`), not leave to chance based on how a prompt happens to be phrased.

## Product Implications

A content type that only ever presents finished information (a pure explanatory Summary with no
generation step) should be treated as the exception requiring justification, not the default — see
`docs/domain/generation-engine.md`'s note that explanatory content is "a deliberate exception made
for a specific pedagogical reason."

## Prompt Implications

Prompts for any content type capable of a generation step should be instructed to withhold the
answer/explanation until after requesting the Student's attempt, and never front-load the answer
"for clarity" — a well-intentioned but effect-defeating mistake a generative model can easily make
if not explicitly constrained (`docs/prompt-engineering/prompt-review-checklist.md`).

## Related Documents

`docs/pedagogy/active-recall.md`, `docs/pedagogy/elaboration.md`, `docs/domain/generation-engine.md`,
`docs/prompt-engineering/generation-engine-prompt.md`.
