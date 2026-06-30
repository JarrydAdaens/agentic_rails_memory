---
name: agent-resume-template
description: Template for a single agent persona's Resume. Rebuilt (never appended) from completion reviews to capture empirical, workflow-specific performance.
metadata:
  version: "1.0"
  agentic_rails_source_version: "2.1"
  owner: "Jarryd Adaens"
  repo: "agentic-rails-memory"
---
# Agent Resume: *Persona Name*

> Rebuilt from scratch on `<YYYY-MM-DD>` over `<N>` completion reviews. **Never appended.** Do not edit by hand between rebuilds; instead add completion reviews and rebuild.

## Snapshot

- Persona: `<persona name + source tooling>`
- Typical underlying models: `<models this persona is usually run on>`
- Corpus size this rebuild: `<N completion reviews>`
- Date range covered: `<earliest>` - `<latest>`
- Freshness target: `<rebuild due after X reviews or Y days>`

## Strengths

- Work types it excels at: `<planning, refactor sweeps, tight bug fixes, reviews, ...>`
- Complexity ranges handled well: `<range / examples>`
- Best-paired models: `<which models make this persona shine>`

## Weaknesses

- Work types it handles poorly: `<...>`
- Complexity ranges it struggles with: `<range / examples>`
- **Effort threshold where it degrades:** `<value + unit>`
- Failure modes: `<scope creep, lost context, over-engineering, ignored constraints, ...>`

## Empirical Metrics

| Dimension | Observation |
| --- | --- |
| Outcome rate | `<succeeded / partial / failed by task type>` |
| Token efficiency | `<tokens per unit of work>` |
| Estimate accuracy | `<how often estimates were right at given effort/complexity>` |
| Error patterns | `<recurring mistakes>` |

## Routing Guidance

- Prefer this persona for: `<task type / complexity / effort / risk profile>`
- Avoid this persona for: `<profile>`
- Best run on model(s): `<model + reasoning level>`
- Pair with mitigation when: `<condition>`

## Source Reviews

- `<links to the completion-review.md files and golden learnings this rebuild drew from>`
