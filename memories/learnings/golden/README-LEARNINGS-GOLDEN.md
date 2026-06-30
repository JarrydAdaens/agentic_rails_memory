---
name: learnings-golden
description: Curated best learnings, sampled during Resume rebuilds to keep signal high.
metadata:
  version: "1.0"
  agentic_rails_source_version: "2.1"
  owner: "Jarryd Adaens"
  repo: "agentic-rails-memory"
---
# Learnings - Golden

The curated best learnings: failures worth remembering, surprising successes, sharp edge
cases, and high-signal sessions. Curation promotes files here from [`../new/README-LEARNINGS-NEW.md`](../new/README-LEARNINGS-NEW.md)
during a run.

## Role in rebuilds

Resume rebuilds (model and agent) sample from **golden plus new**, never from the entire
archive. Golden is the durable, high-signal corpus that keeps regeneration:

- **Tractable** — a small set instead of the whole history.
- **High signal** — noise and routine sessions are left in the archive.
- **Stable** — promotion is rare and deliberate, so golden does not churn.

## Maintenance

- Promotion is intentional and batched, not automatic on every capture.
- When a golden learning is superseded by newer understanding, it should be retired or
  overridden during a curation run (the supersession policy is an open question tracked
  in [`../../../context/design.md`](../../../context/design.md)).
