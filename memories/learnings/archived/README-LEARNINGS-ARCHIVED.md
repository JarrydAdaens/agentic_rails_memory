---
name: learnings-archived
description: Full retained history of session learnings that have been processed by a curation run.
metadata:
  version: "1.0"
  agentic_rails_source_version: "2.1"
  owner: "Jarryd Adaens"
  repo: "agentic-rails-memory"
---
# Learnings - Archived

Once a curation run processes a learning from [`../new/README-LEARNINGS-NEW.md`](../new/README-LEARNINGS-NEW.md), the file moves here.
Over time this may hold hundreds of files.

## Why keep everything

- **Auditability.** The full record of what was captured and when is preserved.
- **Re-processing.** A future, smarter curation pass can re-read the archive and
  re-evaluate what should be promoted to golden.

Files here are considered processed but not authoritative. Resume rebuilds do **not**
sample the whole archive — they sample [`../golden/README-LEARNINGS-GOLDEN.md`](../golden/README-LEARNINGS-GOLDEN.md) plus [`../new/README-LEARNINGS-NEW.md`](../new/README-LEARNINGS-NEW.md)
— precisely so that rebuilds stay tractable as this folder grows.
