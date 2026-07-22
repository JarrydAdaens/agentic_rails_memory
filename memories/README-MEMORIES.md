---
name: memories-overview
description: Overview of the memories store - the durable, git-revisioned drop zone where Agentic Rails skills and agents deposit cross-project artifacts.
metadata:
  version: "1.1"
  agentic_rails_source_version: "2.1"
  owner: "Jarryd Adaens"
  repo: "agentic-rails-memory"
---
# Memories

This is the artifact store of the **records office**: the durable, git-revisioned place
where skills and agents running under Agentic Rails deposit things they want to keep
across projects. Dropping a file here is how an agent "memorizes" something.

> Depositing an artifact does **not** mean it is clean, curated, or ready for another
> agent in another project to consume. It only means something of value was stored
> outside the project where the work happened. Curation comes later.

## What lives here

| Document | Role |
| --- | --- |
| [`README-LEARNINGS.md`](learnings/README-LEARNINGS.md) | Session-end learning artifacts, flowing `new -> archived -> golden`. |
| [`README-MODEL-RESUMES.md`](model-resumes/README-MODEL-RESUMES.md) | One Resume per model, rebuilt from completion reviews. |
| [`README-AGENT-RESUMES.md`](agent-resumes/README-AGENT-RESUMES.md) | One Resume per agent persona, rebuilt from completion reviews. |

The structure is intentionally flexible. New memory types (task reports, model notes,
cross-project references, and so on) can be added here without redesigning the system.

## Deposit vs. consume

- **Deposit (cheap, frequent):** any skill or agent may write an artifact here at a
  natural checkpoint. Capture is meant to be low-friction.
- **Consume (deliberate, rare):** reading memory back into a decision (for example,
  rebuilding a Resume or routing a task) is a curated, intentional act. Raw deposits
  are not authoritative until they have been processed.

See [`../context/design.md`](../context/design.md) for the full design of the memory
system and the capture -> curate -> consume feedback loop.
