# Prompt: Security Review

**Use when:** reviewing a change that touches authentication, data handling, external inputs, or
anything that processes untrusted content (including generative AI output/input).
**Inputs required:** the diff or feature area to review.
**Loads context from:** `specs/authentication.md`, `.ai/constitution.md` Articles IV and V,
`skills/prompt-engineering.md`, `skills/postgres.md`.

```
Perform a security review of {{target}}.

Cover, explicitly, each of the following — mark each Not Applicable with a reason if it genuinely
doesn't apply, rather than omitting it:

1. AuthN/AuthZ: does every new endpoint correctly require and verify an Access Token
   (specs/authentication.md)? Is any Student-scoped data reachable by another Student's token
   (e.g. missing a student_id ownership check on a resource lookup)?
2. Input handling: is untrusted input (file uploads in Ingestion Engine, free-text Evidence
   observations, generative-model output before Validation) validated/sanitized at the boundary
   before it reaches domain logic or storage? Could a crafted Document or Evidence payload cause
   unbounded resource use (specs/document-ingestion.md's size limits, specs/evidence-engine.md)?
3. Injection: any raw SQL construction outside the adapters/ repository layer
   (skills/postgres.md)? Any place a prompt is built by naive string interpolation of untrusted
   content in a way that could manipulate a generative model's behavior
   (skills/prompt-engineering.md)?
4. Secrets: any credential, API key, or token hardcoded, logged, or committed, instead of injected
   at the adapter boundary (skills/docker.md, skills/github-actions.md §Secrets)?
5. Data integrity guarantees: does this change respect Evidence immutability (ADR-003) and the
   no-direct-Confidence-write rule (ADR-004)? A bypass of either is a security-relevant integrity
   issue, not just an architecture nit, because it undermines the auditability the platform relies
   on to explain itself.
6. Validation gate: for anything Generation-Engine-adjacent, could this change let unvalidated
   content reach a Student under any code path (ADR-005)?
7. Rate limiting / abuse: does a new public endpoint have appropriate rate limiting, particularly
   anything under specs/authentication.md's threat model (credential stuffing, token replay)?

For each finding, state: the specific file/line, the concrete exploit scenario (not just "this
could be a problem"), the severity, and the fix. Do not report a theoretical concern without a
plausible attack path — but do not skip a real one because it seems unlikely.
```

**Output:** a severity-ranked list of concrete, exploit-scenario-backed findings across the seven
areas above, or explicit confirmation that an area was checked and found clean.
