---
name: model-resumes-overview
description: Model Resumes - cross-task artifacts recording empirical, workflow-specific performance of each model, rebuilt (never appended) from completion reviews.
metadata:
  version: "1.0"
  agentic_rails_source_version: "2.1"
  owner: "Jarryd Adaens"
  repo: "agentic-rails-memory"
---
# Model Resumes

A **Model Resume** is a central, cross-task artifact that lives outside any single
project's tier hierarchy. There is one Resume per model (Sonnet, Haiku, Opus, GPT,
Qwen, ...).

The framing is deliberate: **picking a model for work is hiring for a team.** A resume
tells you what a worker is good at and their track record. Each Resume holds empirical,
*your-workflow-specific* data — not vendor benchmarks. It is the inverse of a standard
ML "model card" (top-down, generic); Model Resumes are bottom-up ground truth from your
own completion logs.

## What a resume records

- Complexity ranges the model is good at / bad at.
- Effort thresholds where it starts to struggle.
- Token efficiency, speed, and observed error patterns.
- Routing guidance for the orchestrator.

## Rebuild, never append

Resumes are **rebuilt from scratch, never appended.** Appending would over-weight the
most recent job (recency bias).

- **Batched updates:** collect a set of completion reviews and golden learnings over a
  few days, then run the rebuild over the corpus and regenerate all Resumes.
- **Compaction (cost control):** a full rebuild over a large corpus is expensive. Two
  strategies:
  - *Golden-set / stratified sampling:* a cheap first pass scores all logs to pick the
    juiciest ones (failures, surprising successes, edge cases), then deep-evaluates only
    those.
  - *Incremental merge:* evaluate in batches of ~20, then merge the partial Resumes into
    the final Resumes.
- The governing trade-off is **cost vs. freshness** — how stale a Resume may get before a
  rebuild is due (an open parameter for each project).

## Source data

Resumes are rebuilt from the `completion-review.md` files produced inside each project's
implementation-plan folders (specifically their model-signal and "why unexpected"
sections), together with promoted entries in [`../learnings/golden/README-LEARNINGS-GOLDEN.md`](../learnings/golden/README-LEARNINGS-GOLDEN.md).
The rebuild logic is **right-rail tooling** in `agentic_rails_tooling`; this repository
holds only the resulting memory artifacts.

## Files

- [MODEL_RESUME_TEMPLATE.md](MODEL_RESUME_TEMPLATE.md) - copy this to create a Resume for a model.
