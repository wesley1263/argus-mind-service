# Glossary — Ubiquitous Language

Canonical, one-paragraph definition of every business term. This language is **law**: specs, code,
tests, comments, and logs use these terms with exactly these meanings. A new business concept gets
an entry here in the same change that introduces it — don't use a term that isn't defined here.

Grouped by owning Engine (only the owner may change how a term is represented internally).

## Cross-Cutting

- **Student** — the person the platform serves. Subject of a Learning State, participant in a
  Session, recipient of Validated Generation Task output.
- **Engine** — one of the five bounded subsystems (Ingestion, Knowledge, Evidence, Learning State,
  Generation), each owning its own model and exposing a narrow contract.
- **Adaptive Loop** — the closed cycle: Session produces Evidence → Evidence updates Learning
  State → Learning State (+ Knowledge Graph) shapes the next Generation Task → Validated output is
  delivered in a Session → repeat.

## Ingestion Engine

- **Document** — a raw source of material submitted for ingestion. Root of `Document → Chapter →
  Topic`.
- **Chapter** — a top-level structural division of a Document.
- **Topic** — a coherent subject unit within a Chapter; the granularity Learning Nodes are
  extracted at.
- **Chunk** *(internal only)* — the smallest text unit Ingestion Engine produces for its own
  processing. Never crosses the Engine boundary (ADR-002). Not the same as *Chunking*, the
  cognitive-science principle in `memory/pedagogy.md` — related in spirit, never in implementation.
- **OCRResult** *(internal only)* — text extracted via OCR before Chunking. Same internal-only
  rationale as Chunk.
- **Embedding** *(internal only)* — a vector representation of a Chunk/Topic. Internal because its
  shape depends on whichever model produced it.

## Knowledge Engine

- **Learning Node** — the atomic unit of knowledge in the Knowledge Graph; the smallest thing a
  Student can be said to know or not know.
- **Knowledge Edge** — a typed, directed relationship between two Learning Nodes (e.g.
  prerequisite-of).
- **Knowledge Graph** — the full graph of Learning Nodes + Knowledge Edges. Not personalized — see
  Learning Path.
- **Learning Path** — a Student-specific, dynamically computed sequence over the Knowledge Graph.
  Always derived, never stored as a source of truth (same discipline as Learning State).

## Evidence Engine

- **Session** *(a.k.a. Study Session)* — a bounded period of Student interaction. One entity, two
  names — "Session" in engineering/data contexts, "Study Session" in product/pedagogy contexts.
- **Evidence** — an immutable, timestamped observation of what a Student did. Never edited or
  deleted; a correction is new Evidence (ADR-003).
- **EvidenceType** — the classification of an Evidence observation (answer submitted,
  self-explanation, self-reported confidence, hint requested, time on task). Different types may be
  weighted differently by the confidence model.

## Learning State Engine

- **Learning State** — a Student's derived understanding, per Learning Node. Always computed from
  Evidence, never written directly (ADR-004).
- **Confidence** — a normalized `[0,1]` score, continuous (never binary), decaying over time
  without fresh Evidence. Never set directly by anyone.
- **Mastery** *(informal)* — everyday way to say "high Confidence." Never a field name or stored
  value — the system of record is always Confidence.

## Generation Engine

- **Generation Task** — a unit of work: produce content for a target Learning Node/Difficulty/
  content type, using Learning State + Knowledge Graph. Not deliverable until Validated.
- **Validation** — the mandatory quality/safety/pedagogical gate before any content reaches a
  Student. A failure produces a fallback/retry, never a silent bypass (ADR-005).
- **Difficulty** — how demanding content is, calibrated relative to the Student's current
  Confidence — not an absolute scale.
- **PromptContext** — the structured input assembled for a model call (target Learning Node, real
  Knowledge Edges, Confidence, requested content type/Difficulty). Never raw Chunk data.
- **Feedback** — the response shown after a Student engages with content, in reply to specific
  Evidence. Distinct from Evidence (the observation) and Validation (the pre-delivery gate).
- **Reinforcement** — a Generation Task targeted at a low-Confidence Learning Node, triggered by
  struggle signals.
- **AdaptiveContent** — umbrella Student-facing term for anything Generation Engine produces
  (MindMap, Summary, Game, Quiz, Reinforcement). Code uses the specific type once known.
- **MindMap** — content type representing Knowledge Graph structure spatially; must correspond to
  real Knowledge Edges.
- **Summary** — prose content, most valuable when the Student generates it rather than just reads it.
- **Game** — lightweight, engagement-optimized content type.
- **Quiz** — the primary vehicle for retrieval-based practice.
- **SessionPlan** — the ordered sequence of Generation Tasks planned for one Session; advisory,
  adapts per response.
- **ChapterPlan** — a longer-horizon plan sequencing a Chapter's Learning Nodes across future
  Sessions, personalized via Learning Path.

## Maintaining This Glossary

Renaming or redefining a term needs an ADR — it's a change to the shared mental model of the whole
codebase. A term marked *(internal only)* must never appear in a contract, an API, or anywhere a
Student could see it.
