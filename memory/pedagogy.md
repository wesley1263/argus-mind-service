# Pedagogy — Condensed

The learning-science basis behind `PROJECT.md`'s principles and `memory/domain-rules.md`. One line
each, not an essay — this is a reference for *why* a design choice exists, not a textbook.

| Principle | Core idea | Where it shows up |
|---|---|---|
| **Priming** | Advance, structural exposure before formal instruction builds a scaffold for later detail | Pre-class Study Sessions; MindMap as an advance organizer for a brand-new Learning Node |
| **Active Recall** | Retrieving information strengthens memory more than re-reading it | Default to recall/production over multiple-choice recognition |
| **Retrieval Practice** | The *strategy* of structuring study around repeated retrieval (vs. Active Recall, the cognitive act itself) | Drives Generation Engine's overall content-selection policy, not just individual questions |
| **Spaced Repetition** | Revisiting material just before it would be forgotten beats revisiting too early or not at all | Confidence's decay-over-time behavior; no separate "review mode," it's built into ordinary selection |
| **Desirable Difficulties** | Conditions that feel harder in the moment often produce more durable learning | Generation Engine deliberately targets just past the Student's current Confidence, not comfortably within it |
| **Cognitive Load Theory** | Working memory is limited — manage intrinsic, extraneous, and germane load deliberately | Bounded new-element count per piece of content; SessionPlan pacing ramps gradually |
| **Dual Coding** | Verbal + visual encoding creates two independent retrieval paths (only when genuinely complementary, not redundant) | MindMap must add real relational structure, not just restate a Summary in boxes |
| **Generation Effect** | Self-generated information is remembered better than information merely read | Default posture: ask the Student to produce before revealing an answer/explanation |
| **Elaboration** | Connecting new info to existing knowledge via "why" produces deeper processing | Feedback and Quiz content reference real Knowledge Edges, not just the isolated node |
| **Feynman Technique** | Explaining a concept simply reveals exactly what isn't understood | Self-explanation prompts; Feedback localizes the specific gap, not a holistic verdict |
| **Metacognition** | Learners are poorly calibrated about their own understanding | Self-reported confidence is captured as a comparison point, never trusted as a Learning State input |
| **Chunking** | Grouping information into meaningful units expands effective working-memory capacity (Miller, chess-expertise research) | Learning Node granularity — one recognizable idea, not a sentence and not a whole Chapter. **Not** the same as Ingestion Engine's internal `Chunk` |
| **Bloom's Taxonomy** | A classification of cognitive demand (Remember → Create), not itself a memory effect | Keeps Generation Engine's content variety honest — not everything defaults to Remember-level recall |
| **Pareto Principle** | A borrowed heuristic (not a learning-science finding): a small set of foundational concepts disproportionately unlocks the rest | Prioritize Learning Nodes with high prerequisite out-degree in the Knowledge Graph |
| **Motor Encoding** | Physical manipulation adds a modest encoding benefit — the weakest-evidenced principle here | MindMap drag interactions kept as real gestures, not collapsed into tap-to-select purely for convenience |

## Two Disambiguations Worth Remembering

- **Chunking** (this table) ≠ **Chunk** (`memory/glossary.md`, Ingestion Engine's internal text
  unit) — same word, unrelated implementation.
- **Active Recall** ≠ **Retrieval Practice** — the cognitive act vs. the scheduling strategy built
  on it.
