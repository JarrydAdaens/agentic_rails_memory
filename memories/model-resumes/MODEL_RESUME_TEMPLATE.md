---
name: model-resume-template
description: Template for a single model's Resume. Rebuilt (never appended) from completion reviews to capture empirical, workflow-specific performance.
metadata:
  version: "1.0"
  agentic_rails_source_version: "2.1"
  owner: "Jarryd Adaens"
  repo: "agentic-rails-memory"
---
# Model Resume: *Model Name*

> Rebuilt from scratch on `<YYYY-MM-DD>` over `<N>` completion reviews. **Never appended.** Do not edit by hand between rebuilds; instead add completion reviews and rebuild.

## Snapshot

- Model: `<model + provider>`
- Corpus size this rebuild: `<N completion reviews>`
- Date range covered: `<earliest>` – `<latest>`
- Freshness target: `<rebuild due after X reviews or Y days>`

## Strengths

- Complexity ranges handled well: `<range / examples>`
- Work types it excels at: `<planning, refactor sweeps, tight bug fixes, ...>`

## Weaknesses

- Complexity ranges it struggles with: `<range / examples>`
- **Effort threshold where it degrades:** `<value + unit>`
- Failure modes: `<hallucination, scope creep, lost context, ...>`

## Empirical Metrics

| Dimension | Observation |
| --- | --- |
| Token efficiency | `<tokens per unit of work>` |
| Speed | `<observed throughput>` |
| Estimate accuracy | `<how often estimates were right at given effort/complexity>` |
| Error patterns | `<recurring mistakes>` |

## Routing Guidance

- Prefer this model for: `<complexity / effort / risk profile>`
- Avoid this model for: `<profile>`
- Pair with mitigation when: `<condition>`

## Source Reviews

- `<links to the completion-review.md files this rebuild drew from>`
