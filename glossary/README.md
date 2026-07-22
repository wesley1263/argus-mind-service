# Glossary — The Ubiquitous Language

This is the single, canonical definition of every business term used in Smart App /
`argus-mind-service`. Per Article VI of `/.ai/constitution.md`, this language is **law**: specs,
code, tests, comments, logs, and conversation about this system must all use these terms with
exactly these meanings. If a concept doesn't have an entry here yet, it doesn't get used elsewhere
until it does — the entry is added in the same change that first needs it.

Terms are grouped by which Engine owns them (see `/.ai/architecture.md` §2 for the ownership
table), because ownership and meaning are tied together: only the owning Engine may change how a
term is represented internally. An alphabetical index follows for quick lookup.

Every term below has a one-paragraph, canonical definition — that's what makes this document safe
to use as law (Article VI). For attributes, relationships, lifecycle notes, and worked examples,
see the companion `/docs/domain-model/` folder, organized by the same Engine groupings; if this
glossary and `/docs/domain-model/` ever disagree, this glossary wins and the other document is
stale and must be fixed.

---

## Cross-Cutting Terms

Used across every Engine; owned by no single one.

### Student
The person the entire platform exists to serve. The subject of a Learning State, the participant
in a Session, the intended (Validated) recipient of Generation Task output. "Student" is used even
in contexts other systems might call "user" or "learner" — it is the one term for this role,
chosen deliberately to keep the platform's purpose in view.

### Engine
One of the five bounded, single-responsibility subsystems that compose the platform: Ingestion
Engine, Knowledge Engine, Evidence Engine, Learning State Engine, Generation Engine. Each Engine
owns its own internal model and exposes only an explicit, versioned contract to the rest of the
system. See `/.ai/architecture.md`.

### Adaptive Loop
The closed cycle that makes the platform adaptive in fact, not just in name: a Session produces
Evidence; Evidence updates Learning State; Learning State (with the Knowledge Graph) shapes the
next Generation Task; a Validated Generation Task is delivered in a Session; repeat. See
`docs/architecture-overview.md`.

---

## Ingestion Engine

Turns a raw Document into structured material the Knowledge Engine can build from.

### Document
A raw source of educational material submitted to the platform (e.g., a textbook, an article, a
syllabus, a set of lecture notes). The unit of input to the Ingestion Engine, and the root of the
structural hierarchy Document → Chapter → Topic.

### Chapter
A top-level structural division of a Document, typically mirroring the source material's own
organization (a book chapter, a course module, a major section). A Document has one or more
Chapters.

### Topic
A coherent unit of subject matter within a Chapter — smaller than a Chapter, and the level of
granularity at which the Knowledge Engine typically extracts Learning Nodes. A Chapter has one or
more Topics.

### Chunk *(internal only)*
The smallest unit of text the Ingestion Engine produces for its own internal processing (e.g., for
embedding or extraction). Chunk is **owned exclusively by the Ingestion Engine** and must never
appear in another Engine's model, in a published contract, in an API response, or anywhere a
Student could see it. Its size and shape are free to change as ingestion technique evolves,
precisely because nothing outside Ingestion Engine depends on it. See ADR-002. Not to be confused
with **Chunking**, the cognitive-science principle (`/docs/pedagogy/chunking.md`) — related in
spirit, never in implementation.

### OCRResult *(internal only)*
The text extracted from a scanned or image-based Document page by optical character recognition,
before it is split into Chunks. Owned exclusively by the Ingestion Engine, for the same reason as
Chunk: its shape is a function of the OCR technique used, not a business concept. See
`/docs/domain-model/ingestion-entities.md`.

### Embedding *(internal only)*
A vector representation of a Chunk or Topic, used internally by the Ingestion Engine for
similarity-based processing. Owned exclusively by the Ingestion Engine — publishing it would
hard-couple every consumer to whichever model produced it, contradicting Article I of the
Constitution. See `/docs/domain-model/ingestion-entities.md`.

---

## Knowledge Engine

Builds and maintains the Knowledge Graph from what Ingestion Engine publishes.

### Learning Node
The atomic, addressable unit of knowledge in the Knowledge Graph — the smallest thing a Student can
be said to know or not know (a concept, a fact, a skill). Every Confidence value and every
Generation Task ultimately refers to one or more Learning Nodes.

### Knowledge Edge
A typed, directed relationship between two Learning Nodes in the Knowledge Graph (for example,
"is a prerequisite of," "is related to," "is part of"). Edges are what make the Knowledge Graph a
graph rather than a flat list.

### Knowledge Graph
The full directed graph of Learning Nodes connected by Knowledge Edges, maintained by the Knowledge
Engine from ingested material. Represents the structure of everything the platform currently
understands it's possible to know, for a given body of Documents. Never personalized per Student —
see Learning Path below for the Student-specific view.

### Learning Path
A Student-specific, dynamically computed sequence or subset of Learning Nodes representing the
platform's current recommendation for what that Student should engage with next. Always a derived
projection over the Knowledge Graph and Learning State — like Learning State itself, never stored
or treated as an independent source of truth. See `/docs/domain-model/knowledge-entities.md`.

---

## Evidence Engine

Captures what actually happened during a Student's Session, as immutable fact.

### Session *(a.k.a. Study Session)*
A bounded period of interaction between a Student and the platform — it has a start, an end, and
exactly one Student. Evidence is generated during a Session, and Validated Generation Task output
is delivered within one. "Study Session" is the same entity, referred to by that name in product
and pedagogy contexts (`/docs/domain/study-session.md`) — there is one entity, not two.

### Evidence
An immutable, timestamped observation of a Student's interaction or performance with respect to one
or more Learning Nodes (an answer submitted, a self-assessment, time spent, a hint requested).
Evidence is the only input the Learning State Engine trusts, and once written it is never edited or
deleted — a correction is new Evidence, never a change to old Evidence. See ADR-003 and Article IV
of the Constitution.

### EvidenceType
The classification of what kind of observation an Evidence record represents (e.g., an answer
submitted, a self-explanation, a self-reported confidence, a hint request, time on task). Different
EvidenceTypes may be weighted differently by the Learning State Engine's confidence model. See
`/docs/domain-model/evidence-entities.md` for the full taxonomy.

---

## Learning State Engine

Derives what a Student currently knows, from their Evidence.

### Learning State
A Student's current, derived understanding of their own mastery, expressed per Learning Node.
Learning State is always computed from Evidence by the Learning State Engine — it is a projection,
never a value anyone writes directly. See ADR-004 and Article IV of the Constitution.

### Confidence
A normalized, per-(Student, Learning Node) score within a Learning State, representing the
platform's current estimate of how well that Student has mastered that Learning Node. Confidence is
recomputed from Evidence — including decaying over time without fresh Evidence — and is never set
directly by any actor. "Mastery" (below) is the informal way to talk about a high Confidence value;
Confidence is the only value the system actually stores and computes.

### Mastery *(informal)*
The everyday way of describing "a Student has high Confidence in a Learning Node." Used in prose,
UX copy, and conversation — never as a field name, a stored value, or a synonym for Confidence in
code. The system of record is always Confidence.

---

## Generation Engine

Decides and produces the next right piece of content for a Student, and gets it Validated before
it can be delivered.

### Generation Task
A unit of work describing what learning content should be produced next — e.g., a question of a
given difficulty for a given Learning Node for a given Student — decided using that Student's
Learning State and the Knowledge Graph. A Generation Task's output is not deliverable until it has
passed Validation.

### Validation
The mandatory quality, safety, and pedagogical-soundness gate that every Generation Task's output
must pass before it may reach a Student. Validation is an explicit, auditable step with its own
pass/fail outcome — not an inline check inside generation — and a failed Validation always produces
a fallback or a retry, never a silent bypass. See ADR-005 and Article V of the Constitution.

### Difficulty
A value describing how demanding a piece of content is, calibrated relative to a Student's current
Confidence for the target Learning Node — not a fixed, learner-independent scale. See
`/docs/pedagogy/desirable-difficulties.md`.

### PromptContext
The structured, typed input assembled for a Generation Task's underlying model call: the target
Learning Node, its real Knowledge Edges, the Student's Confidence, and the requested content
type/Difficulty/Bloom's level. Never raw Chunk data. See
`/docs/domain-model/generation-entities.md`.

### Feedback
The response shown to a Student after they engage with content, in reply to a specific piece of
Evidence. Distinct from Evidence itself (the observation) and from Validation (the pre-delivery
gate). See `/docs/domain-rules/README.md`'s "the child is never blamed."

### Reinforcement
A Generation Task specifically targeted at a low-Confidence Learning Node, produced in response to
struggle signals rather than ordinary forward progression. See `/docs/domain-rules/README.md`'s
"only problematic Learning Nodes are regenerated."

### AdaptiveContent
The Student-facing umbrella term for anything Generation Engine produces — MindMap, Summary, Game,
Quiz, and Reinforcement are all kinds of AdaptiveContent. Used in product copy and cross-content
reporting; engineering code uses the specific content type once it's known.

### MindMap
A content type representing Knowledge Graph structure spatially. Its structure must correspond to
real Knowledge Edges. See `/docs/pedagogy/dual-coding.md`.

### Summary
A content type covering a Learning Node or Topic in prose, most valuable when it requires Student
generation rather than passive reading. See `/docs/pedagogy/generation-effect.md`.

### Game
A lightweight, engagement-optimized content type. See `/docs/domain-principles.md` §10 ("fun
creates habit").

### Quiz
A content type that is the primary vehicle for Retrieval Practice and Active Recall. See
`/docs/pedagogy/retrieval-practice.md`.

### SessionPlan
The ordered sequence of Generation Tasks intended for one Session, encoding pacing and format
variety. Advisory — actual delivery adapts per Student response. See
`/docs/domain/study-session.md`.

### ChapterPlan
A longer-horizon planning artifact sequencing a Chapter's Learning Nodes across multiple future
Sessions, personalized per Student via Learning Path. See
`/docs/domain-model/generation-entities.md`.

---

## Alphabetical Index

| Term | Owning Engine |
|---|---|
| [AdaptiveContent](#adaptivecontent) | Generation Engine |
| [Adaptive Loop](#adaptive-loop) | — (cross-cutting) |
| [Chapter](#chapter) | Ingestion Engine |
| [ChapterPlan](#chapterplan) | Generation Engine |
| [Chunk](#chunk-internal-only) *(internal only)* | Ingestion Engine |
| [Confidence](#confidence) | Learning State Engine |
| [Difficulty](#difficulty) | Generation Engine |
| [Document](#document) | Ingestion Engine |
| [Embedding](#embedding-internal-only) *(internal only)* | Ingestion Engine |
| [Engine](#engine) | — (cross-cutting) |
| [Evidence](#evidence) | Evidence Engine |
| [EvidenceType](#evidencetype) | Evidence Engine |
| [Feedback](#feedback) | Generation Engine |
| [Game](#game) | Generation Engine |
| [Generation Task](#generation-task) | Generation Engine |
| [Knowledge Edge](#knowledge-edge) | Knowledge Engine |
| [Knowledge Graph](#knowledge-graph) | Knowledge Engine |
| [Learning Node](#learning-node) | Knowledge Engine |
| [Learning Path](#learning-path) | Knowledge Engine |
| [Learning State](#learning-state) | Learning State Engine |
| [Mastery](#mastery-informal) *(informal)* | Learning State Engine |
| [MindMap](#mindmap) | Generation Engine |
| [OCRResult](#ocrresult-internal-only) *(internal only)* | Ingestion Engine |
| [PromptContext](#promptcontext) | Generation Engine |
| [Quiz](#quiz) | Generation Engine |
| [Reinforcement](#reinforcement) | Generation Engine |
| [Session](#session-aka-study-session) *(a.k.a. Study Session)* | Evidence Engine |
| [SessionPlan](#sessionplan) | Generation Engine |
| [Student](#student) | — (cross-cutting) |
| [Summary](#summary) | Generation Engine |
| [Topic](#topic) | Ingestion Engine |
| [Validation](#validation) | Generation Engine |

## Maintaining This Glossary

- A new business concept is added here in the same change that first uses it — never as a
  follow-up (`/.ai/development-principles.md` §5).
- Renaming or redefining a term is a change to the shared mental model of the entire codebase and
  requires an ADR (`/.ai/constitution.md` Article VI).
- A term marked *(internal only)* must never appear in a published Engine contract, an API, a log
  line visible to a Student, or any document outside its owning Engine's own material.
