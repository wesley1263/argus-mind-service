# Prompt: Review Code

**Use when:** reviewing a diff before merge (this is what the `reviewer` subagent runs).
**Loads:** `CLAUDE.md`, `memory/glossary.md`, `memory/domain-rules.md`, the spec it implements.

```
Independently review this change: {{diff_or_files}}.
{{If available: "It implements " + spec_path + " — read that spec first."}}

Check, in order — quote the specific line(s) that satisfy or violate each, don't mark anything
checked without evidence:

1. Traceability — is this traceable to a spec/ADR? Does it do more than the spec asked for?
2. Boundaries — does any Engine import another directly? Does Chunk (or any internal-only concept)
   leak outside its owning Engine? Is a contract change additive or breaking, and if breaking, is
   there an ADR?
3. Ubiquitous language — does every business-concept name match memory/glossary.md exactly?
4. Evidence/Learning State integrity — anything writing to Learning State or Confidence directly?
   Anything mutating an existing Evidence record?
5. Validation gate — any path delivering Generation Engine output without Validation?
6. Domain rules — does this violate anything in memory/domain-rules.md (e.g. Feedback tone,
   regeneration scope)?
7. Code quality — pure functions with I/O at the edges? Unexplained cleverness? Abstraction for a
   hypothetical case?
8. Tests — do they read as documentation of a real rule? Would they actually fail if the behavior
   broke?

Never approve an unresolved issue in sections 2, 4, or 5 "with a follow-up" — those are
Constitutional, not stylistic.

Report as: [section] → [issue] → [file:line] → [why it matters] → [suggested fix].
```

**Output:** section-by-section findings and a clear approve/changes-requested verdict.
