# Golden Dataset

## Purpose

Some correctness questions can't be answered by a pass/fail unit test, because there's no single
correct output — only a range of reasonable ones, and a clear sense of what's *wrong*. "Did
Knowledge Engine extract sensible Learning Nodes from this Topic?" and "did Generation Engine
produce content appropriate to this Confidence level?" are both this kind of question. A golden
dataset is a curated set of real (or realistic) inputs paired with human-reviewed, agreed-correct
(or agreed-acceptable-range) outputs, used to score a change rather than just pass/fail it.

## What Lives in a Golden Dataset

- **Knowledge Engine**: a set of representative Documents (`specs/document-ingestion.md` output —
  Chapters/Topics) paired with a reviewed set of expected Learning Nodes and Knowledge Edges
  (`specs/knowledge-engine.md`), including deliberately tricky cases (ambiguous prerequisite
  direction, sparse Topics with little extractable content).
- **Generation Engine**: a set of (Student Learning State, Knowledge Graph neighborhood) input
  scenarios paired with example acceptable and unacceptable outputs, used both to evaluate a
  generation/prompt change (`prompt-evaluation.md`) and to sanity-check the target-selection logic
  in `specs/generation-engine.md` R1 against realistic states.
- **Learning State Engine**: sequences of Evidence paired with expected Confidence trajectories
  (including decay behavior, `specs/learning-state-engine.md` AC3), used to validate a confidence
  model version change before it's activated (R7 in that spec).

## Curation and Governance

- A golden dataset entry is added or changed through the same review discipline as any other
  change — a PR, reviewed by someone who can actually judge correctness for that entry (a subject
  matter reviewer for Knowledge Engine content, not just an engineer).
- Each entry records *why* its expected output is correct, briefly — not just the expected value.
  A future reviewer needs to be able to tell whether a scoring drop reflects a real regression or an
  entry that was wrong to begin with.
- Golden dataset changes are versioned alongside the model/prompt/extraction-technique versions
  they're meant to evaluate (`skills/prompt-engineering.md`, `specs/learning-state-engine.md` R4) so
  a score is always comparable to the right baseline.

## How It's Used in CI

`skills/github-actions.md`'s `golden-dataset-eval` job runs the current extraction/generation code
against the dataset and produces a score (see `prompt-evaluation.md` for the specific rubric used on
the Generation Engine side). A score regression below a configured threshold blocks merge, the same
as a failing test — but because the underlying judgment is closer to "how good," not strictly
"correct or not," a score change is reported with enough detail (which entries regressed, by how
much) for a reviewer to judge whether it's acceptable, not just a red/green gate.

## Growing the Dataset

Every real-world extraction or generation failure reported as a bug (`templates/bug.md`) that
reveals a case the dataset didn't cover is itself a candidate new entry — this is one of the primary
ways the dataset grows, alongside deliberate curation. This mirrors `regression-testing.md`'s
principle at the dataset-scoring layer rather than the pass/fail layer.
