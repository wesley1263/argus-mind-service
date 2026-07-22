# Templates

Lightweight templates for project artifacts other than specs and ADRs, which have their own
dedicated templates (`specs/template.md`, `adr/template.md`) because they carry binding process
requirements. These are simpler fill-in-the-blank shapes for planning and tracking.

| Template | Use for |
|---|---|
| [`feature.md`](feature.md) | Capturing a feature idea before it's promoted to a full spec. |
| [`epic.md`](epic.md) | Grouping several related specs under one larger initiative. |
| [`rfc.md`](rfc.md) | An open-ended proposal where the right approach is genuinely undecided — distinct from an ADR, which records a decision already made. |
| [`spike.md`](spike.md) | Time-boxed research needed before a spec can be written responsibly. |
| [`bug.md`](bug.md) | Reporting and tracking a defect, with an explicit check for Evidence/Confidence/Validation integrity issues. |
| [`task.md`](task.md) | The smallest unit of tracked implementation work — what `prompts/generate-tasks.md` produces. |
| [`decision.md`](decision.md) | A smaller, reversible decision not requiring full ADR rigor. |
| [`release.md`](release.md) | The shape of each `CHANGELOG.md` entry (Keep a Changelog format). |
| [`meeting-notes.md`](meeting-notes.md) | Standard meeting notes, with explicit routing of decisions to `decision.md` or an ADR. |

## Choosing Between `decision.md`, `rfc.md`, and an ADR

These three are easy to conflate — use this to pick the right one:

- **`decision.md`** — the decision is already made and is cheap to reverse. Write it down for
  context, move on.
- **`rfc.md`** — the decision is *not yet made* and benefits from broader discussion before it is.
  An accepted RFC that turns out to be irreversible or cross-Engine graduates into an ADR.
- **`adr/template.md`** — the decision is irreversible or cross-Engine
  (`.ai/development-principles.md` §2), regardless of how it was arrived at. This is the only one
  of the three that's binding on future implementation per `.ai/constitution.md` Article VII.
