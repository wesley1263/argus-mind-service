# Pareto Principle

## What It Is

The Pareto Principle (the "80/20 rule") is the general observation, originating in economics
(Vilfredo Pareto's observation about wealth distribution), that in many systems a small proportion
of causes accounts for a large proportion of effects. Applied to curriculum design, the informal
claim is that a small subset of foundational concepts in a subject disproportionately determines a
Student's ability to understand the rest.

## Scientific Evidence

**Honesty check, stated up front**: unlike every other document in `docs/pedagogy/`, this is *not*
a cognitive-science finding about how memory or learning works — it's a heuristic borrowed from
economics and general systems thinking, applied here by analogy. There is no controlled experimental
literature establishing an "80/20" ratio specifically for curriculum prioritization. What *does*
have real support is the underlying structural claim it's standing in for: prerequisite/foundational
concepts in a knowledge domain tend to have disproportionately high "out-degree" in a dependency
graph (many other concepts depend on them) — which is a structural property Knowledge Engine can
actually measure directly from the Knowledge Graph's Knowledge Edges, rather than assuming a fixed
ratio. This document is included specifically so that assumption is stated honestly rather than
smuggled in as if it were settled science.

## Why Smart App Uses It

As a **prioritization heuristic**, not a scientific claim: when Generation Engine or a human
reviewing extraction results has to decide which Learning Nodes deserve the most SessionPlan
attention, prerequisite out-degree in the Knowledge Graph is a principled, measurable proxy for
"foundational" — the Pareto framing is a useful way to communicate the *idea* of prioritization to
stakeholders, even though the actual engineering decision should be driven by measured graph
structure, not an assumed ratio.

## Where It Appears in the Product

Learning Node prioritization within target selection (`specs/generation-engine.md` R1) can weight a
Learning Node's out-degree (how many other nodes list it as a prerequisite) as one input alongside
Confidence, so foundational nodes get earlier and more attention.

## Engineering Implications

If out-degree weighting is implemented, it should be computed directly from
`knowledge.knowledge_edges` (`skills/postgres.md`), not hardcoded as "the most important 20% of
nodes" by manual curation — a measured graph property, refreshed as the Knowledge Graph grows, per
"Knowledge Always Evolves Incrementally" (`docs/domain-rules/README.md`).

## Product Implications

Product messaging should avoid stating an "80/20" claim as if it were a scientific guarantee (e.g.,
"master 20% of this course and understand 80% of it") — the honest claim is "foundational concepts
get prioritized because more of your understanding depends on them," which is defensible; the
specific ratio is not.

## Prompt Implications

If a prompt references prioritization ("focus on the most foundational concepts"), it should be
grounded in the actual Knowledge Edge structure passed into context, not left to the model's own
judgment of what's "important," which has no access to the real graph.

## Related Documents

`docs/domain/knowledge-engine.md`, `specs/knowledge-engine.md`, `docs/pedagogy/bloom-taxonomy.md`.
