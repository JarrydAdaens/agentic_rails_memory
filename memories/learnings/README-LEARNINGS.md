---
name: learnings-overview
description: Overview of the learnings store and its new -> archived -> golden lifecycle for session-end learning artifacts.
metadata:
  version: "1.0"
  agentic_rails_source_version: "2.1"
  owner: "Jarryd Adaens"
  repo: "agentic-rails-memory"
---
# Learnings

The primary artifact store of the memory system. A learning is the raw, self-reflective
output of a work session: what an agent struggled with, what it did well, the models
used, the token cost, and the task outcome. **One file = one session = one data point.**

A single learning file is never used to directly rewrite a Resume. It is one data point
that *informs* a later rebuild. Regenerating Resumes from individual sessions every time
would corrupt them with noise.

## Lifecycle: `new -> archived -> golden`

| Stage | Document | Meaning |
| --- | --- | --- |
| Captured | [`README-LEARNINGS-NEW.md`](new/README-LEARNINGS-NEW.md) | Freshly deposited, not yet processed by curation. |
| Processed | [`README-LEARNINGS-ARCHIVED.md`](archived/README-LEARNINGS-ARCHIVED.md) | Reviewed by a curation run; full history retained for audit and re-processing. |
| Promoted | [`README-LEARNINGS-GOLDEN.md`](golden/README-LEARNINGS-GOLDEN.md) | The curated best learnings. Resume rebuilds sample from golden (plus new), never the whole archive. |

This keeps Resume rebuilds tractable and the signal high: curation samples a small,
high-value set rather than re-reading hundreds of raw sessions.

## Principles

- **Capture is cheap, promotion is deliberate.** Session-end capture is frequent and
  low-cost. Promotion to golden is rare, batched, and intentional.
- **Intentional, not exhaustive.** The system does not record everything an agent ever
  did. Hoovering everything in wastes context budget and corrupts signal.
- **Project isolation by default.** One project's learnings do not contaminate another.
  Cross-project promotion is deliberate, not accidental.

The ranking and evaluator logic that drives curation is **right-rail tooling** in
`agentic_rails_tooling`; this repository holds only the resulting artifacts.
