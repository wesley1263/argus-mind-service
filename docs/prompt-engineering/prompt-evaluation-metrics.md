# Prompt Evaluation Metrics

The concrete rubric behind `testing/prompt-evaluation.md`'s five dimensions, made specific to this
platform's pedagogy — that document defines the *process* (score, compare, gate); this document
defines *what "good" means* for each dimension, per content type.

## The Five Dimensions, Made Concrete

### Correctness
Does the content's factual content match the Knowledge Graph it's grounded in? Scored by checking
every factual claim against the supplied Learning Node/Knowledge Edge data — not against the
model's own general knowledge, which may be more "correct" in a general sense but wrong for this
platform's specific Knowledge Graph.

### Difficulty Match
Does the content's actual demand match the requested Difficulty, calibrated relative to the target
Student's Confidence (`docs/pedagogy/desirable-difficulties.md`) — not an absolute judgment of
"hard" or "easy."

### Pedagogical Soundness
Content-type-specific:
- **Quiz**: does it match its stated Bloom's level structurally (`docs/pedagogy/bloom-taxonomy.md`)?
  Does it favor recall/production over recognition where appropriate
  (`docs/pedagogy/active-recall.md`)?
- **MindMap**: does its structure correspond to real Knowledge Edges and add non-redundant spatial
  information (`docs/pedagogy/dual-coding.md`)?
- **Summary/Feynman evaluation**: does Feedback localize the specific gap rather than giving a
  holistic verdict (`docs/pedagogy/feynman-technique.md`)?
- **Game**: does difficulty come from genuine retrieval demand, not misleading framing
  (`docs/pedagogy/retrieval-practice.md`)?

### Safety
Free of unsafe, inappropriate, or off-topic content, and — specific to this platform — free of any
framing that violates "the child is never blamed" (`docs/domain-rules/README.md`).

### Language/Tone Consistency
Uses `/glossary/README.md` terminology correctly in any Student-visible copy; never exposes internal
terms (Chunk, PromptContext, Embedding).

## Scoring Scale

1–5 per dimension per scenario, scored by a human reviewer or a periodically-audited model judge
(`testing/prompt-evaluation.md`'s note on judge auditing), never averaged into one composite number
— per `prompt-regression-strategy.md`'s point about hiding meaningful drift inside an average.

## Minimum Bar for a New Prompt Version

No dimension below 4/5 on any golden-dataset scenario at first activation; a first-time prompt with
no prior baseline requires this bar explicitly (rather than being graded on "no worse than nothing"),
per `automation/ai-automation/prompt-evaluation-workflow.md`.

## Related Documents

`testing/prompt-evaluation.md`, `automation/ai-automation/prompt-evaluation-workflow.md`,
`docs/golden-dataset/evaluation-methodology.md`, `docs/pedagogy/`.
