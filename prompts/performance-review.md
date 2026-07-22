# Prompt: Performance Review

**Use when:** reviewing a change against the latency/throughput expectations implied by its spec,
or investigating a suspected performance regression.
**Inputs required:** the target code/feature, and its spec if one exists (for the
Non-Functional Requirements it declares).
**Loads context from:** the spec's Requirements/Risks sections, `.ai/architecture.md` §6,
`skills/postgres.md`, `skills/fastapi.md`.

```
Perform a performance review of {{target}}.

1. Find every Non-Functional Requirement in the relevant spec(s) that implies a latency/throughput
   budget (e.g. specs/generation-engine.md R6, specs/learning-state-engine.md R6,
   specs/study-session.md R5). State each one explicitly before reviewing against it — don't invent
   a budget the spec doesn't declare.

2. Trace the actual request path for the feature under review, per its Sequence Diagram, and
   identify every synchronous I/O call in it: database queries, calls to another Engine's contract,
   external model calls. For each, ask whether it's on the critical path a Student is waiting on,
   or safely asynchronous (.ai/architecture.md §6 — no Engine should be a synchronous single point
   of failure for another).

3. Database: check for N+1 query patterns, missing indexes on columns used in WHERE/JOIN
   (particularly the identifier columns used for cross-Engine references, since they have no
   foreign key to lean on per skills/postgres.md), and unbounded result sets.

4. Engine boundary calls: is data fetched in the granularity actually needed (e.g.
   specs/student-progress.md's "batch contract calls" requirement, R1), or does the code make a
   contract call per item in a loop?

5. Generation Engine specifically: is content pre-generated/cached ahead of demand where the spec
   anticipates it (ADR-005's consequences, specs/generation-engine.md Future Work), or does every
   request pay full generation + Validation latency?

6. If a golden-dataset or prompt-evaluation suite exists for the feature (testing/golden-dataset.md,
   testing/prompt-evaluation.md), check whether performance characteristics (latency per
   evaluation run) are tracked alongside correctness, since a regression there is invisible to a
   pass/fail-only test.

For each finding: the specific bottleneck, its position on the critical path, an estimate of
impact if measurable (or "needs profiling" if not), and a concrete fix — not a generic "consider
caching" without saying what and where.
```

**Output:** a critical-path-focused list of concrete bottlenecks and fixes, checked against the
spec's own declared latency/throughput requirements rather than generic performance advice.
