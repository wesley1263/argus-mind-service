# Vision

## The Problem

Most learning material is authored once and delivered the same way to everyone: the same
explanation, the same sequence, the same pace, regardless of what a given Student already knows or
is struggling with. The result is a mismatch that compounds — a Student re-reads material they've
already mastered, or is pushed forward past a gap that will resurface later as confusion that looks
unrelated to its actual cause.

Adaptive learning promises to fix this, but most implementations treat "adaptive" as a thin
personalization layer bolted onto otherwise static content, or as "point an AI model at the
Student and let it improvise." Neither approach has a durable, inspectable model of what the
Student actually knows. Without that model, "adaptive" is a guess, not a method.

## The Product Is the Methodology, Not the AI

Smart App exists to make the methodology itself the durable asset:

1. **Ingest** real source material into structured content.
2. **Model** that material as a Knowledge Graph — an explicit map of Learning Nodes and how they
   depend on each other.
3. **Observe** each Student honestly through Evidence — what they actually did, not what we assume.
4. **Derive** a Learning State per Student that says, with a Confidence per Learning Node, what
   they currently know.
5. **Generate**, and rigorously Validate, the next right piece of content for that specific Student
   given that specific state.

Any of the five steps above can change its internal technique — a better parsing approach in
Ingestion, a better extraction model in Knowledge, a better generative model in Generation — without
changing what the platform *means*. That durability is the point. See `/.ai/constitution.md`
Article I.

This repository, `argus-mind-service`, is the backend service that implements this methodology: the
"mind" behind Smart App. The name is intentional — the service is not a feature of the product, it
*is* the adaptive core the rest of the product experience is built around.

## Who This Serves

The direct beneficiary of every design decision in this repository is the **Student** — the person
whose Learning State the platform is trying to represent honestly and act on responsibly. Every
Engine exists to serve that goal:

- Ingestion and Knowledge exist so there is something true and well-structured to know.
- Evidence and Learning State exist so the platform's belief about a Student is grounded in what
  actually happened, not assumption.
- Generation and Validation exist so what reaches the Student is both relevant and trustworthy.

## What Success Looks Like

- A Student's Confidence per Learning Node is explainable: it can always be traced back to specific
  Evidence, not to an opaque score nobody can account for.
- Ingesting a new Document extends the Knowledge Graph without anyone hand-authoring
  relationships from scratch.
- Swapping the technique behind any single Engine — a new extraction approach, a new generation
  model — never requires touching another Engine, because the contracts between them didn't move.
- Nothing reaches a Student that hasn't passed Validation, and every Validation failure is visible,
  not silently dropped.

## Non-Goals (for now)

- **Not a general-purpose content authoring tool.** Ingestion works from real source Documents; it
  is not a WYSIWYG course builder.
- **Not a chatbot.** Generation Engine produces specific, Validated learning content driven by
  Learning State — not open-ended conversation.
- **Not committed to any specific AI vendor or model.** See Article I of the Constitution — that
  would contradict "the methodology is the product."
- **Not microservices on day one.** See ADR-001 — the five Engines are modeled as sovereign
  boundaries inside one deployable service until there is a real operational reason to split them.

## Where This Fits

This document explains *why* the platform exists. For *how* it's structured, see
`architecture-overview.md`. For the words used to talk about it precisely, see
`/glossary/README.md`. For the rules that keep this vision from eroding one convenient shortcut at
a time, see `/.ai/constitution.md`.
