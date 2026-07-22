# Glossary — The Ubiquitous Language

This is the single, canonical definition of every business term used in Smart App /
`argus-mind-service`. Per Article VI of `/.ai/constitution.md`, this language is **law**: specs,
code, tests, comments, logs, and conversation about this system must all use these terms with
exactly these meanings. If a concept doesn't have an entry here yet, it doesn't get used elsewhere
until it does — the entry is added in the same change that first needs it.

Terms are grouped by which Engine owns them (see `/.ai/architecture.md` §2 for the ownership
table), because ownership and meaning are tied together: only the owning Engine may change how a
term is represented internally. An alphabetical index follows for quick lookup.

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
precisely because nothing outside Ingestion Engine depends on it. See ADR-002.

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
understands it's possible to know, for a given body of Documents.

---

## Evidence Engine

Captures what actually happened during a Student's Session, as immutable fact.

### Session
A bounded period of interaction between a Student and the platform — it has a start, an end, and
exactly one Student. Evidence is generated during a Session, and Validated Generation Task output
is delivered within one.

### Evidence
An immutable, timestamped observation of a Student's interaction or performance with respect to one
or more Learning Nodes (an answer submitted, a self-assessment, time spent, a hint requested).
Evidence is the only input the Learning State Engine trusts, and once written it is never edited or
deleted — a correction is new Evidence, never a change to old Evidence. See ADR-003 and Article IV
of the Constitution.

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

---

## Alphabetical Index

| Term | Owning Engine |
|---|---|
| [Adaptive Loop](#adaptive-loop) | — (cross-cutting) |
| [Chapter](#chapter) | Ingestion Engine |
| [Chunk](#chunk-internal-only) *(internal only)* | Ingestion Engine |
| [Confidence](#confidence) | Learning State Engine |
| [Document](#document) | Ingestion Engine |
| [Engine](#engine) | — (cross-cutting) |
| [Evidence](#evidence) | Evidence Engine |
| [Generation Task](#generation-task) | Generation Engine |
| [Knowledge Edge](#knowledge-edge) | Knowledge Engine |
| [Knowledge Graph](#knowledge-graph) | Knowledge Engine |
| [Learning Node](#learning-node) | Knowledge Engine |
| [Learning State](#learning-state) | Learning State Engine |
| [Mastery](#mastery-informal) *(informal)* | Learning State Engine |
| [Session](#session) | Evidence Engine |
| [Student](#student) | — (cross-cutting) |
| [Topic](#topic) | Ingestion Engine |
| [Validation](#validation) | Generation Engine |

## Maintaining This Glossary

- A new business concept is added here in the same change that first uses it — never as a
  follow-up (`/.ai/development-principles.md` §5).
- Renaming or redefining a term is a change to the shared mental model of the entire codebase and
  requires an ADR (`/.ai/constitution.md` Article VI).
- A term marked *(internal only)* must never appear in a published Engine contract, an API, a log
  line visible to a Student, or any document outside its owning Engine's own material.
