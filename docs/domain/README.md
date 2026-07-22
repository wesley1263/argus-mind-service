# Domain

The pedagogical and product rationale behind the platform — *why* each Engine exists and *why* it
works the way it does, complementing `specs/` (engineering behavior) and `.ai/architecture.md`
(structure). Read `docs/domain-principles.md` first — everything here traces back to it.

| Document | Covers |
|---|---|
| [`product-philosophy.md`](product-philosophy.md) | The Core Product Thesis, unpacked, and what it rules out. |
| [`learning-science.md`](learning-science.md) | How to read `docs/pedagogy/`; the two families of principles and how they're used. |
| [`adaptive-learning.md`](adaptive-learning.md) | What "adaptive" means precisely, and why simpler alternatives don't satisfy the thesis. |
| [`knowledge-engine.md`](knowledge-engine.md) | Why a prerequisite-structured Knowledge Graph, at Topic-level granularity. |
| [`evidence-engine.md`](evidence-engine.md) | Formative assessment theory; why immutability is pedagogical, not just an engineering property. |
| [`learning-state-engine.md`](learning-state-engine.md) | Mastery learning; why Confidence decays and is never binary; the false-negative asymmetry. |
| [`generation-engine.md`](generation-engine.md) | The Generation Effect as the Engine's namesake; why content type and difficulty vary deliberately. |
| [`study-session.md`](study-session.md) | Why Sessions are the right unit of pedagogical design, and what a well-shaped one looks like. |
| [`student-journey.md`](student-journey.md) | The whole loop, narrated as one Student's actual experience. |

## Reading Order

For a new contributor (human or AI agent): `docs/domain-principles.md` →
`product-philosophy.md` → `student-journey.md` (the big picture) → the five per-Engine documents (the
mechanism) → `docs/pedagogy/` (the underlying science each Engine draws on).
