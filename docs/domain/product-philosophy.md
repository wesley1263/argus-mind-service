# Product Philosophy

## Smart App Is Not an Educational App

An educational app answers questions, delivers lessons, or drills facts. Smart App does something
narrower and harder: it tries to change *when and how* a Student first builds a mental model of a
subject, on the hypothesis that this timing matters more than almost any other variable in how
efficiently they learn.

## The Core Product Thesis, Unpacked

> A student who builds a mental model of the subject **before** classroom exposure learns more
> efficiently than a student encountering the content for the first time during class.

Three claims are packed into that sentence, and each has a different implication for the product:

1. **"Builds a mental model"** — not "reads a summary" or "answers a quiz correctly." A mental model
   is a structure of related concepts a Student can reason with, not a fact they can recite. This is
   why Knowledge Engine models Learning Nodes and their relationships explicitly
   (`docs/domain/knowledge-engine.md`) rather than treating a Chapter as an undifferentiated block of
   content to summarize.
2. **"Before classroom exposure"** — timing is the independent variable the whole product turns on.
   Priming research (`docs/pedagogy/priming.md`) and the Generation Effect
   (`docs/pedagogy/generation-effect.md`) both suggest that *retrieving or constructing* an idea
   before formal instruction changes how that instruction is encoded later, compared to encountering
   it cold.
3. **"Learns more efficiently"** — the product optimizes for durable understanding per unit of
   Student effort and time, not for engagement metrics or session length as ends in themselves. See
   `docs/evaluation/README.md` for how this is actually measured.

## What This Rules Out

- **A generic AI tutor answering arbitrary questions.** That optimizes for "answer the question
  asked," not for "build the mental model this Student needs before their next class." Generation
  Engine is deliberately driven by Learning State and the Knowledge Graph
  (`specs/generation-engine.md` R1), not by open-ended Student prompts.
- **Content that could be understood passively.** If a Student can absorb something by reading
  alone, generating it as a lecture-style explanation wastes the Generation Effect. The platform
  favors formats that require the Student to do something — retrieve, construct, explain
  (`docs/pedagogy/feynman-technique.md`) — over formats that only require them to read.
- **Optimizing for AI capability.** A more capable underlying model is only valuable insofar as it
  serves the thesis above. `.ai/constitution.md` Article I exists so that engineering never quietly
  substitutes "make the AI better" for "make the learning better" — these are not the same goal, and
  they can diverge.

## Why "The AI Is Only an Implementation Detail" Is a Product Claim, Not Just an Engineering One

If Smart App's value depended on a specific model's cleverness, competitors with access to the same
or a better model would erase that value overnight. The actual, durable asset is the *methodology*:
which Learning Nodes matter, what counts as Evidence, how Confidence should be estimated, when
content should be regenerated, and which pedagogical techniques to apply and when. That methodology
is expressed across `docs/domain/`, `docs/pedagogy/`, and `docs/domain-rules/` — it would survive a
complete swap of the underlying generative model, which is exactly the point.

## Relationship to Other Documents

- `docs/domain-principles.md` — the permanent, enumerated principles this philosophy implies.
- `docs/domain/adaptive-learning.md` — how the thesis is operationalized as a closed feedback loop.
- `docs/domain/student-journey.md` — the thesis, narrated as one Student's actual experience.
- `.ai/constitution.md` Article I — the engineering enforcement of "the AI is a tool."
