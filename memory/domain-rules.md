# Domain Rules

Operational rules implied by `PROJECT.md`'s product principles. Each is a real constraint that's
been violated before it was written down — check this before touching Confidence, Feedback, or
regeneration logic.

| Rule | Why | Engineering consequence |
|---|---|---|
| A validated Learning Node never becomes invalid | Reversing validation would make the Knowledge Graph untrustworthy; a concept that was correctly identified doesn't stop being real | No state transition goes from `Published` back to invalid — only forward to `Deprecated` |
| Learning State is estimated, never certain | No assessment method measures understanding with certainty | API always returns Confidence + model version, never a boolean |
| Confidence is never binary | Understanding is continuous; a snapshot binary judgment is stale immediately | Stored/transmitted as a float in `[0,1]`, never a flag |
| False negatives are worse than false positives | A false positive self-corrects on the next attempt; a false negative wastes the Student's time on something they've already learned | Tune/evaluate the confidence model with this asymmetry explicit, not weighted equally |
| The child is never blamed | A Student who feels judged disengages; framing struggle as the learner's fault is worse for motivation than framing it as information | Feedback describes the *gap*, never the *Student* — checked in review |
| Only problematic Learning Nodes are regenerated | Wholesale regeneration wastes cost and discards Evidence already earned elsewhere | Reinforcement is scoped to one Learning Node, never a whole SessionPlan |
| Knowledge evolves incrementally | The platform's value compounds only if new Documents extend the graph without disruptive rework | Extraction is additive and idempotent per Document |
| A Document is immutable once ingested | The source material a Student was taught from is a historical fact | Editing a Document isn't supported — a changed source is a new Document |
| Evidence accumulates forever | Deleting/compacting it destroys the ability to explain any Confidence value | Append-only, enforced at the DB grant level, never deleted (ADR-003) |

See `memory/pedagogy.md` for the learning-science basis behind several of these.
