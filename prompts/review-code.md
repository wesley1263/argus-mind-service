# Prompt: Review Code

**Use when:** reviewing a diff or a set of changed files before or instead of a full PR review
(see `prompts/review-pull-request.md` for the PR-level version, which also checks process/CI).
**Inputs required:** the diff or file set to review, and (if available) the spec it implements.
**Loads context from:** `.ai/review-checklist.md`, `.ai/constitution.md`, `glossary/README.md`,
`.ai/coding-philosophy.md`.

```
You are independently reviewing the following change: {{diff_or_files}}.
{{If available: "It implements " + spec_path + " — read that spec first."}}

Work through /.ai/review-checklist.md section by section. For each section, quote the specific
line(s) of the diff that satisfy or violate it — do not mark a box checked without pointing at
evidence in the actual code:

1. Traceability — is this change traceable to a spec/ADR? Does it do more than the spec asked for?
2. Boundaries — does any Engine import another Engine's package directly? Does Chunk (or any other
   declared internal-only concept) leak outside its owning Engine? Is any contract change additive
   or breaking, and if breaking, is there an ADR?
3. Ubiquitous Language — does every business-concept name match /glossary/README.md exactly? Does
   any new concept have a glossary entry in this same change?
4. Evidence / Learning State Integrity — does anything write to Learning State or Confidence
   directly? Does anything mutate or delete an existing Evidence record or other fact-of-record?
5. Validation Gate — does any new path deliver Generation Engine output to a Student without
   passing Validation? Is a Validation failure handled explicitly?
6. Code Quality — is business logic expressed as pure functions with I/O at the edges? Is there
   unexplained cleverness? Was any abstraction introduced for a hypothetical case, not the current
   spec?
7. Tests — do the tests read as documentation of a business rule? Would they actually fail if the
   behavior broke? Are contract tests present for any changed contract?
8. Documentation — is every document whose described behavior changed updated in this same change?
9. Definition of Done — spot-check the author's own /.ai/definition-of-done.md claims rather than
   trusting them.

Do not approve a change with an unresolved issue in sections 2-5 (boundaries, language, integrity,
validation) "with a follow-up" — those are Constitutional, not stylistic, per
/.ai/review-checklist.md's closing note.

Report findings as: [section] → [specific issue] → [file:line] → [why it matters] →
[suggested fix]. If nothing is wrong in a section, say so explicitly rather than omitting it.
```

**Output:** a section-by-section review with concrete, evidence-backed findings, and a clear
approve/changes-requested verdict.
