# Golden Dataset: Structure

The concrete schema for each golden dataset item type referenced by `testing/golden-dataset.md`.
That document defines *how* the dataset is used in CI; this document defines *what each entry
actually contains*, with a worked example per type.

## Example Chapters

The root fixture every other type is derived from — a small, real (or realistic) Chapter with known,
human-reviewed correct structure.

```yaml
chapter:
  title: "Newton's Laws of Motion"
  topics:
    - title: "Inertia and the First Law"
      summary: "An object remains at rest or in uniform motion unless acted on by a net force."
    - title: "Force, Mass, and Acceleration"
      summary: "F = ma; acceleration is proportional to net force and inversely proportional to mass."
```

## Expected Learning Nodes

Per Topic, the human-reviewed correct set of Learning Nodes, each with its justification (required
per `docs/prompt-engineering/knowledge-engine-prompt.md`'s output contract).

```yaml
expected_learning_nodes:
  - label: "Inertia"
    justification: "A distinct, independently testable concept from the First Law's topic."
  - label: "Newton's First Law"
    justification: "The formal statement building on Inertia — a separate, higher-order node."
  - label: "Newton's Second Law (F = ma)"
    prerequisite_of: []
    prerequisites: ["Inertia", "Newton's First Law"]
```

## Expected Session Plans

Given a Student Learning State fixture (a small set of Confidence values), the expected SessionPlan
shape: which Learning Nodes, what content types, in what order — checked against
`docs/domain/study-session.md`'s pacing structure (opens easy, ramps, varies format).

## Expected Mind Maps

A reviewed MindMap structure for a given Learning Node neighborhood, checked for correspondence to
real Knowledge Edges (`docs/prompt-engineering/mindmap-validation-prompt.md`).

## Expected Summaries

A reviewed, high-quality Summary example per Learning Node, at multiple quality tiers (a genuinely
good one, a "jargon without substance" one, an incomplete one) so
`docs/prompt-engineering/summary-evaluation-prompt.md` can be scored against known-bad examples,
not only known-good ones.

## Expected Feedback

Given a specific incorrect/incomplete Student response fixture, the expected Feedback shape:
localized to the specific gap, framed per "the child is never blamed"
(`docs/domain-rules/README.md`).

## Expected Confidence

Given an Evidence sequence fixture, the expected resulting Confidence trajectory — including a
decay-over-time scenario (`docs/pedagogy/spaced-repetition.md`) and a false-negative-risk scenario
specifically designed to check the asymmetric error handling (`docs/domain-rules/README.md`).

## Expected Regeneration

A scenario where a specific Learning Node's content should trigger Reinforcement, with the expected
scope of regeneration (that Learning Node only — `docs/domain-rules/README.md`'s "only problematic
Learning Nodes are regenerated") explicitly checked against a fixture where a naive implementation
might incorrectly regenerate an entire SessionPlan.

## Expected Games

A reviewed Game example per supported mechanic (matching, sorting, sequencing —
`docs/prompt-engineering/game-generation-prompt.md`), with expected element grounding.

## Expected Quizzes

A reviewed Quiz example per Bloom's level (`docs/pedagogy/bloom-taxonomy.md`) per Learning Node, so
`docs/prompt-engineering/quiz-generation-prompt.md`'s level-matching requirement is checkable against
real examples, not just described abstractly.

## Related Documents

`testing/golden-dataset.md`, `docs/golden-dataset/evaluation-methodology.md`,
`docs/prompt-engineering/README.md`.
