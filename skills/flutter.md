# Skill: Flutter (Client Consuming This Backend)

`argus-mind-service` is a backend service; it contains no Flutter code. This skill exists for
whichever repository implements Smart App's client and needs to consume this backend correctly — and
for anyone in this repository designing an API who needs to think about what's reasonable to ask a
client to do. If a Flutter client repository adopts this harness's conventions, this file is what it
should build from.

## The Client Is a Thin Presentation Layer

Per Article I of `.ai/constitution.md`, the adaptive-learning methodology lives in this backend, not
in the client. A Flutter client must never independently compute, cache-and-trust, or override a
Confidence value, an eligibility/locked decision, or what content to show next — those are Learning
State Engine's and Generation Engine's jobs (`specs/learning-state-engine.md`,
`specs/generation-engine.md`). The client requests, displays, and forwards Student interaction back
as Evidence; it does not decide.

## Generate the API Client, Don't Hand-Write It

The backend's OpenAPI schema (`skills/fastapi.md`) is the source of truth for every request/response
shape. Generate typed Dart models and API bindings from it rather than hand-writing HTTP calls —
hand-written client code drifts from the backend contract silently; generated code fails to compile
the moment the contract changes, which is the feedback you want.

## Authentication

Mirror `specs/authentication.md` exactly: store the Access Token in memory for the life of the app
session, store the Refresh Token in secure platform storage (Keychain/Keystore, never
`SharedPreferences`/plain storage), and implement the same rotation behavior the backend expects —
a used-once Refresh Token, refreshed proactively before the Access Token's short expiry lapses. Do
not implement a "remember me forever" pattern that skips rotation; it defeats the reuse-detection
security model the backend relies on.

## The Study Session Loop Is Synchronous From the Client's View

`specs/study-session.md` composes several backend calls into one request/response step per
interaction. The client's job is simple: show content, collect a response, submit it, show the next
content (or a "nothing ready yet" state per that spec's AC4) — it does not need its own local state
machine mirroring the backend's Generation Task lifecycle. Resist the urge to reimplement
`specs/generation-engine.md`'s State Diagram client-side; treat the backend's response as the single
source of truth for what to render.

## State Management Principles (Stack-Agnostic)

Regardless of which Flutter state-management approach the client repository adopts:

- Server-derived state (Learning State, Knowledge Graph structure, Generation Task content) is
  treated as **read-through, not owned** by the client — it's re-fetched or invalidated on the
  relevant action, never locally mutated to "look right" optimistically for anything that reflects
  Confidence or progress. Optimistic UI is fine for things like "message sent" acknowledgment, never
  for adaptive-loop state.
- Local-only UI state (which screen tab is active, form input before submission) is kept clearly
  separate from server-derived state so it's obvious, on any given screen, which parts are safe to
  guess ahead of a server response and which must wait for one.

## Offline / Latency Handling

The Adaptive Loop genuinely requires a round trip per interaction (Evidence must be recorded before
the next content is meaningfully chosen — `specs/study-session.md`). Design for visible,
honest loading states rather than synthesizing a plausible-looking "next question" locally while
offline; a Student answering content the backend never validated undermines the entire Validation
guarantee in `specs/generation-engine.md`.

## Ubiquitous Language in the UI

Copy can use "Mastery" informally (per `glossary/README.md`'s explicit allowance), but any field
name, analytics event, or state variable in client code uses the exact glossary term
(`confidence`, `learningNode`, `session`) — the same discipline `.ai/constitution.md` Article VI
requires backend-side, so a bug report or log line means the same thing on either side of the wire.
