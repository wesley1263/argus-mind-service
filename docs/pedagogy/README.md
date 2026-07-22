# Pedagogy

Fifteen learning-science principles the platform is built on, each documented with what it is, its
scientific evidence, why Smart App uses it, where it appears in the product, and its engineering,
product, and prompt implications. Read `docs/domain/learning-science.md` first for how these
principles relate to each other and how to read their evidence sections honestly.

## Encoding Principles (how information gets in)

| Principle | Core idea |
|---|---|
| [`priming.md`](priming.md) | Advance exposure before formal instruction builds a scaffold for later detail. |
| [`dual-coding.md`](dual-coding.md) | Verbal + visual encoding creates two independent retrieval paths. |
| [`elaboration.md`](elaboration.md) | Connecting new information to existing knowledge via "why" deepens processing. |
| [`chunking.md`](chunking.md) | Grouping information into meaningful units expands effective working-memory capacity. **Not** the same as Ingestion Engine's `Chunk` — see the disambiguation note in that document. |
| [`generation-effect.md`](generation-effect.md) | Self-generated information is remembered better than information merely read. |
| [`motor-encoding.md`](motor-encoding.md) | Physical manipulation adds a modest additional encoding benefit — the weakest-evidenced principle here, treated honestly as such. |

## Retrieval & Consolidation Principles (how memory is strengthened)

| Principle | Core idea |
|---|---|
| [`active-recall.md`](active-recall.md) | Retrieving information strengthens memory more than re-reading it. |
| [`retrieval-practice.md`](retrieval-practice.md) | The scheduling/structuring strategy built on Active Recall — distinct from the cognitive act itself. |
| [`spaced-repetition.md`](spaced-repetition.md) | Revisiting material just before it would be forgotten produces the strongest retention. |
| [`desirable-difficulties.md`](desirable-difficulties.md) | Conditions that feel harder often produce more durable learning than conditions that feel easier. |

## Cross-Cutting Principles

| Principle | Core idea |
|---|---|
| [`cognitive-load-theory.md`](cognitive-load-theory.md) | Working memory is limited; manage intrinsic, extraneous, and germane load deliberately. |
| [`feynman-technique.md`](feynman-technique.md) | Explaining a concept simply reveals exactly what isn't yet understood. |
| [`metacognition.md`](metacognition.md) | Learners are poorly calibrated about their own understanding — trust Evidence, not self-report. |

## Classification & Prioritization Tools

| Tool | Nature |
|---|---|
| [`bloom-taxonomy.md`](bloom-taxonomy.md) | A classification scheme for cognitive demand — used to keep content variety honest. |
| [`pareto-principle.md`](pareto-principle.md) | A borrowed heuristic, not a cognitive-science finding — used for prioritizing foundational Learning Nodes. Its evidence section says so explicitly. |

## Two Deliberate Name Collisions, Resolved

- **`chunking.md`** (cognitive principle) vs. **Chunk** (Ingestion Engine's internal artifact,
  ADR-002) — related in spirit, never in implementation.
- **`active-recall.md`** vs. **`retrieval-practice.md`** — the cognitive act vs. the pedagogical
  strategy built on it. Each document states the distinction explicitly rather than treating the
  terms as interchangeable.
