# Cognitive Load Theory

## What It Is

Cognitive Load Theory (Sweller, 1988) models working memory as a severely limited resource
(consistent with Miller's and later Baddeley's working-memory capacity research —
`docs/pedagogy/chunking.md`) and distinguishes three kinds of demand any learning task places on it:
**intrinsic load** (the inherent complexity of the material itself), **extraneous load** (demand
created by how the material is presented — poor formatting, irrelevant detail, confusing
navigation), and **germane load** (demand that directly contributes to building durable mental
schemas). Effective instructional design minimizes extraneous load and manages intrinsic load, to
leave working-memory capacity available for germane load.

## Scientific Evidence

Sweller's original worked-example research and the large body of multimedia-learning work that
followed (notably Mayer's Cognitive Theory of Multimedia Learning, which extends CLT with Dual
Coding — `docs/pedagogy/dual-coding.md`) consistently show that reducing extraneous load (e.g.,
removing redundant on-screen text narrated aloud, integrating labels spatially near what they refer
to) improves learning outcomes, holding the actual content constant. This is one of the more
practically actionable, well-replicated frameworks in instructional design specifically.

## Why Smart App Uses It

Every content-generation decision — how much text, how many concepts introduced at once, whether to
combine text and diagram — is, functionally, a cognitive-load management decision. Without an
explicit framework, a generative model left unconstrained tends to over-explain (adding extraneous
load) or under-scaffold a genuinely complex Learning Node (mismanaging intrinsic load).

## Where It Appears in the Product

SessionPlan design (`docs/domain-model/generation-entities.md`) — sequencing content so intrinsic
load ramps gradually rather than presenting the hardest material first; content-type formatting
rules (e.g., a MindMap groups related Learning Nodes spatially rather than as an undifferentiated
list, reducing extraneous load per the spatial-contiguity principle from multimedia learning
research).

## Engineering Implications

Generation Engine's difficulty parameter (`specs/generation-engine.md` R2) should be understood as
primarily managing *intrinsic* load, while `docs/prompt-engineering/prompt-review-checklist.md`'s
formatting rules manage *extraneous* load — these are separable concerns, and conflating them (e.g.,
treating "simplify the explanation" and "simplify the concept" as the same instruction) produces
worse content than addressing each explicitly.

## Product Implications

A single piece of generated content should introduce or exercise a small, bounded number of new
elements at once — this is a concrete constraint product design and content-length limits should
respect, not an abstract ideal. Session pacing (`docs/domain/study-session.md`) exists specifically
to spread germane load across time rather than concentrating it.

## Prompt Implications

Every content-generation prompt (`docs/prompt-engineering/`) should carry an explicit
extraneous-load constraint — no decorative content, no restating information already established
earlier in the same Session, no unnecessary framing text before the substantive content begins.

## Related Documents

`docs/pedagogy/dual-coding.md`, `docs/pedagogy/chunking.md`, `docs/domain/study-session.md`,
`docs/prompt-engineering/prompt-review-checklist.md`.
