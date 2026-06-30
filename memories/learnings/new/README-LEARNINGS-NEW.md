---
name: learnings-new
description: Landing zone for freshly captured, unprocessed session-end learning artifacts.
metadata:
  version: "1.0"
  agentic_rails_source_version: "2.1"
  owner: "Jarryd Adaens"
  repo: "agentic-rails-memory"
---
# Learnings - New

Freshly captured session learnings land here. A session-end skill self-reflects at a
natural checkpoint (often after a large, 100k-300k+ token session) and drops one
timestamped markdown file per session.

## Conventions

- **One file per session.** Name: `<project>_<YYYY-MM-DD>.md`. The on-disk name stays
  clean and human-scannable; heavy metadata lives in the file's frontmatter, not the
  filename. On a same-day, same-project collision, append a numeric counter
  (`<project>_<YYYY-MM-DD>_2.md`).
- **Raw, not curated.** Capture the honest observations; do not polish for reuse.
- **No Resume edits.** A capture skill writes here and nowhere else. It must not touch
  model or agent Resumes.

## What a learning should record

- Task outcome (succeeded / partial / failed / blocked) and why.
- What the agent struggled with and what it did well.
- Full provenance and execution profile: the harness used, and per model its role,
  reasoning vs non-reasoning, thinking effort, and whether fast mode was on.
- Models and agent personas used.
- Token cost and any notable efficiency or latency observations.
- Surprises, edge cases, and failure modes worth remembering.

The right-rail `rails-memory-learnings` skill produces these artifacts.

A curation run later reads everything here, ranks it, moves processed files to
[`../archived/README-LEARNINGS-ARCHIVED.md`](../archived/README-LEARNINGS-ARCHIVED.md), and promotes the strongest to [`../golden/README-LEARNINGS-GOLDEN.md`](../golden/README-LEARNINGS-GOLDEN.md).
