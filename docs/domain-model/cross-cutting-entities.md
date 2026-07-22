# Domain Model: Cross-Cutting Entities

> Companion to `glossary/README.md`. Entities owned by no single Engine.

## Student

**Definition**: The person the entire platform exists to serve — the subject of a Learning State,
the participant in a Session, the intended Validated recipient of GenerationTask output
(`glossary/README.md`).

| Attribute | Type | Notes |
|---|---|---|
| `id` | identifier | Shared Kernel type — the one identifier every Engine may reference |
| Account/credential data | — | Owned by Platform (`specs/authentication.md`), not by any Engine |

**Relationships**: has a Learning State per Learning Node encountered; participates in Sessions;
owns Evidence records (indirectly, via Sessions).

**Non-relationship, stated explicitly**: a Student is never directly linked to a "judgment" or
"grade" entity — no such entity exists in this domain model, per "The System Never Judges Learning"
(`docs/domain-principles.md` §13). The closest concept, Confidence, is deliberately modeled as an
estimate about a Learning Node, not a rating of the Student.

## Related Documents

`glossary/README.md`, `specs/authentication.md`, `docs/domain-principles.md`.
