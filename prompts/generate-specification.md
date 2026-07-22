# Prompt: Generate Specification

**Use when:** starting a new feature, before any implementation planning begins.
**Inputs required:** a description of the problem or feature, in business terms.
**Loads context from:** `specs/template.md`, `glossary/README.md`, `.ai/architecture.md`,
existing specs in `/specs/` (for consistency of style and to avoid duplicating an existing one).

```
Generate a specification for: {{feature_description}}.

Before writing:
1. Check /specs/ for an existing spec that already covers this, or that this would extend — don't
   create a duplicate.
2. Check /glossary/README.md for the terms this feature involves. Use them exactly. If a needed
   business concept doesn't exist yet, add it to the glossary in this same change before using it
   in the spec (.ai/constitution.md Article VI) — do not invent an ad hoc name and hope it gets
   reconciled later.
3. Identify which Engine(s) this spec belongs to, using the ownership table in
   /.ai/architecture.md §2. If it spans more than one Engine's responsibilities without being a
   thin orchestration (see specs/study-session.md for that pattern), that's a sign the feature
   might need to be split into per-Engine specs plus one orchestration spec.

Copy specs/template.md to specs/{{kebab-case-name}}.md and fill in every section:
- Business Context, Goals (numbered, with explicit Non-goals), Requirements (traced to Goals),
  Acceptance Criteria (Given/When/Then, testable).
- Sequence Diagram and State Diagram as real Mermaid diagrams reflecting this feature's actual
  actors and entities — not the template's placeholder names left unedited.
- API, Events, Database sections following the conventions in skills/fastapi.md,
  skills/postgres.md. Mark a section "Not applicable: <why>" rather than deleting it if genuinely
  unused (see specs/student-progress.md or specs/study-session.md for examples of that pattern).
- Risks, stated honestly — an empty Risks table is a sign this wasn't examined critically.
- Future Work — specific, deferred extensions, not vague aspiration.
- Definition of Done — spec-specific criteria in addition to, not instead of,
  /.ai/definition-of-done.md.

Flag explicitly whether this spec requires an ADR before implementation
(.ai/development-principles.md §2), and if so, what decision that ADR needs to cover.
```

**Output:** a complete spec file in `specs/`, following the template exactly, ready for
`prompts/generate-tasks.md` or direct implementation via `prompts/implement-feature.md`.
