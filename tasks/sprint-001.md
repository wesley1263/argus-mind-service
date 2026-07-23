# Sprint 1: Document Ingestion

**Spec:** `specs/001-document-ingestion.md`. **Status:** Planned, approved, not started.

## Context

First sprint that writes actual code — also the sprint that creates `docker-compose.yml`, the
`src/`/`tests/` trees, and CI workflows every later sprint reuses. Scoped to Ingestion Engine alone;
does not reach into Knowledge Engine (Sprint 2) or Authentication (Sprint 8).

Two decisions made while planning:
- **No auth on the ingestion endpoint for this sprint.** The spec's "Operator" actor has no defined
  role yet (`specs/008-authentication.md` only covers Student login). Adding one is a one-line
  `Depends(...)` later, not a redesign.
- **Published contracts live in each Engine's own `ports/` module** (already reflected in
  `CLAUDE.md`'s Architecture section) — this is the one thing another Engine may import from this
  package.

Not blocking, worth knowing: `tasks/backlog.md` previously listed "Embedding Engine"/"Retrieval
Engine" as done sprints with no supporting spec/ADR — dropped from the backlog.

## Scope

**In:** `POST /v1/documents` (multipart + optional outline, sync size check, `202`),
`GET /v1/documents/{id}` (status + chapters/topics or failure reason), heading-based PDF structuring
and outline-based plain-text structuring on top of a private `Chunk` (never exposed), async
processing that never blocks the caller, a `DocumentIngested`/`DocumentIngestionFailed` contract
that structurally cannot carry Chunk data, plus the Docker/CI/`CHANGELOG.md` scaffolding.

**Deferred:** OCR (`OCRResult`) and real `Embedding` computation — neither AC requires them (AC1 is
a text-layer PDF, AC2 is plain text + outline; both are heading/outline-based, not semantic). The
`embedding` column still gets created (nullable) so no breaking migration later. Also deferred: a
real message broker (nothing justifies one yet) and the consumer half of the contract test
(Knowledge Engine doesn't exist until Sprint 2 — tracked as a named Sprint 2 task).

## Technical Decisions

None of these need an ADR (reversible, single-Engine, swappable adapter choices):

- **PDF parsing: `pdfplumber`** (MIT, exposes per-character font-size — needed for heading
  hierarchy). Rejected `PyMuPDF` (AGPL/commercial license), `pypdf` (no font metadata).
- **ORM: Tortoise ORM, migrations via Aerich** (not SQLAlchemy/Alembic) — async-native, matches
  `memory/coding-standards.md`'s async-only-in-adapters rule. Aerich autogenerates migrations by
  diffing Tortoise `Model` classes in `adapters/postgres/models.py` — no hand-written SQL. Models
  never cross into `domain/`/`application/`; the repository adapter converts at the boundary.
- **DI: `dependency-injector`.** `platform/container.py` wires repository/parsers/publisher into
  `IngestDocumentUseCase`; routes use `@inject` + `Depends(Provide[...])`. Unit tests bypass the
  container and construct the use case directly against in-memory fakes.
- **Lifespan, not `@app.on_event`**: Tortoise init/teardown + container wiring in one
  `@asynccontextmanager` passed to `FastAPI(lifespan=lifespan)`.
- **Async: FastAPI `BackgroundTasks`** for "don't block the caller," plus a tiny synchronous
  in-process event bus (`platform/event_bus.py`, per-handler `try/except` isolation) for
  `DocumentIngested` delivery. Zero subscribers registered this sprint (Knowledge Engine doesn't
  exist yet) — intentional. **Accepted limitation:** not durable; a process crash mid-processing
  leaves a Document stuck, no auto-retry. Revisit via ADR if real operational pressure justifies a
  durable queue.
- **Tests: pytest exclusively** (`pytest-asyncio`, `pytest-cov`, `httpx.AsyncClient`).

## Docker / Compose

Multi-stage Dockerfile: `builder` (Poetry install, `src/`, Aerich config) → `runtime` (venv + `src/`
only, runs `uvicorn argus.platform.http.main:app`). `lint`/`test` reuse `builder`.

| Service | Purpose |
|---|---|
| `db` | Postgres, `pg_isready` healthcheck, named volume |
| `migrate` | One-shot `aerich upgrade`, depends on `db` healthy |
| `app` | `uvicorn`, depends on `db` healthy + `migrate` completed |
| `lint` | `ruff check . && ruff format --check . && mypy .` |
| `test` | Full `pytest` (unit + integration + architecture), against the real `db` service |

Config via `.env` (gitignored) + `.env.example`: `DATABASE_URL`, `MAX_DOCUMENT_SIZE_BYTES`.

## Tasks

**Scaffolding**
1. `pyproject.toml` deps: `fastapi`, `uvicorn[standard]`, `python-multipart`, `pydantic-settings`,
   `tortoise-orm`, `aerich`, `dependency-injector`, `pdfplumber`; dev: `ruff`, `mypy`, `pytest`,
   `pytest-asyncio`, `pytest-cov`, `httpx`, `import-linter`.
2. `Dockerfile`, `docker-compose.yml`, `.env.example`.
3. `[tool.ruff]`, `[tool.mypy]` (strict), `[tool.pytest.ini_options]`, `[tool.coverage.run]`,
   `[tool.aerich]` in `pyproject.toml`.
4. `import-linter` contracts: no Engine imports another except via its own `ports/`; `shared_kernel`
   imports nothing; `ingestion_engine` layered `adapters → ports → application → domain` inward-only;
   `tortoise`/`dependency_injector` imports confined to `adapters/`+`platform/`.
5. `CHANGELOG.md` (Keep a Changelog, `Unreleased`).
6. `.github/workflows/{lint,tests,docker-build}.yml`.
7. `tests/{unit,integration,architecture}/` tree, mirroring `src/argus/<engine>/`.

**Shared Kernel & Platform**

8. `shared_kernel/identifiers.py` — `DocumentId`/`ChapterId`/`TopicId` (`NewType` over `uuid.UUID`).
9. `platform/config.py` — `pydantic-settings` `Settings`.
10. `platform/event_bus.py` — sync pub/sub, per-handler `try/except`.
11. `platform/orm.py` — `TORTOISE_ORM` config dict.
12. `platform/container.py` — DI container.
13. `platform/http/app.py` — app factory, `lifespan`, `/health`.
14. `platform/http/main.py` — ASGI entrypoint.

**Database (`ingestion` schema)**

15. `aerich init` + `init-db`.
16. `adapters/postgres/models.py` — `DocumentModel` (mutable `status` — this is a state machine, not
    append-only like Evidence; don't apply ADR-003's grant-revocation pattern here), `ChapterModel`,
    `TopicModel`, `ChunkModel` (`embedding` nullable JSON field, not `pgvector` — avoid an extension
    dependency this sprint).
17. `aerich migrate` + `aerich upgrade`.

**Domain (pure, no I/O)**

18. `domain/{document,chapter,topic,chunk}.py` — frozen dataclasses, `DocumentStatus` enum.
19. `domain/errors.py` — `IngestionError` base + `UnsupportedFormatError`, `CorruptFileError`,
    `NoStructureDetectedError`, `DocumentTooLargeError`.
20. `domain/outline.py` — `Outline`/`OutlineChapter`.
21. `domain/size_validation.py` — pure `validate_size(...)` (AC5).
22. `domain/status_transitions.py` — legal state-diagram transitions only.
23. `domain/heading_parser.py` — `list[TextLine]` → structure via font-size hierarchy; raises
    `NoStructureDetectedError` (AC1, R2).
24. `domain/outline_structuring.py` — `paragraphs + Outline` → structure (title/order from outline;
    content-to-topic split is a deterministic even split — AC2 only asserts title/hierarchy match).

**Ports**

25. `ports/contracts.py` — `DocumentIngested`, `DocumentIngestionFailed`.
26. `ports/repositories.py` — `DocumentRepository` Protocol.
27. `ports/parsers.py` — `DocumentParserPort` Protocol.
28. `ports/event_publisher.py` — `DocumentEventPublisher` Protocol.

**Application**

29. `application/ingest_document.py` — `IngestDocumentUseCase`: parse → structure → persist →
    publish `DocumentIngested`; any `IngestionError` → persist `Failed`+reason, publish
    `DocumentIngestionFailed`, never `DocumentIngested`.

**Adapters**

30. `adapters/postgres/document_repository.py` — Tortoise-based, converts Model ↔ domain dataclass.
31. `adapters/parsing/pdf_parser.py`, `text_parser.py`.
32. `adapters/events/in_process_publisher.py`.
33. `adapters/http/{schemas,mappers,exception_handlers,router}.py` — router uses
    `@inject`/`Depends(Provide[...])`; size validated synchronously before scheduling anything
    (AC5); auth intentionally absent.
34. Wire router + container into `platform/http/app.py`.

**Tests**

35. Unit: heading parser (AC1 + failure), outline structuring (AC2), status transitions, size
    validation (AC5), use case against in-memory fakes (AC3, R6) — constructed directly, never via
    the container.
36. Integration: Tortoise repository round-trip; full HTTP+DB round trip (AC1/2/3/5); producer-side
    `DocumentIngested` contract test asserting its field set is exactly `{document_id, chapters,
    topics}` (AC4) — consumer half deferred to Sprint 2, tracked explicitly.
37. Architecture: no cross-Engine imports except via `ports/`; `Chunk` never appears outside
    Ingestion Engine; zero framework/ORM imports in `domain/`/`application/`.
38. `pytest-cov` gates (≥85% overall, ≥95% domain/application).

## Verification

```bash
docker compose build
docker compose up -d db
docker compose run --rm migrate
docker compose run --rm lint
docker compose run --rm test
docker compose up app db   # manual smoke: real PDF/text fixture through the full flow
```

| AC | Verified by |
|---|---|
| AC1 (PDF headings → structure) | unit heading-parser + integration HTTP |
| AC2 (text + outline → structure) | unit outline-structuring + integration HTTP |
| AC3 (corrupt file → reason, no partial publish) | unit use-case + integration HTTP |
| AC4 (no Chunk in the contract) | architecture test + producer-side contract test + response schema |
| AC5 (oversized rejected synchronously) | unit size-validation + integration HTTP |
