---
name: agent-resumes-overview
description: Agent Resumes - cross-task artifacts recording empirical, workflow-specific performance of each agent persona, rebuilt (never appended) from completion reviews.
metadata:
  version: "1.0"
  agentic_rails_source_version: "2.1"
  owner: "Jarryd Adaens"
  repo: "agentic-rails-memory"
---
# Agent Resumes

An **Agent Resume** is the persona-level counterpart to a Model Resume. There is one
Resume per agent persona (the specialist agents and personas defined in the right rail,
`agentic_rails_tooling`).

Where a Model Resume profiles a raw model, an Agent Resume profiles a *configured worker*:
a persona with a particular role, instruction set, and tool access. The same model can
earn very different track records under different personas, so they are tracked separately.

> Picking a model is hiring for raw ability; picking an agent persona is hiring for a
> role. Both deserve a track record built from real work.

## What a resume records

- Task types the persona handles well / badly.
- Complexity and effort ranges where it succeeds or degrades.
- Observed failure modes (scope creep, lost context, over-engineering, ...).
- Which underlying models the persona performs best with.
- Routing guidance for the orchestrator.

## Rebuild, never append

Like Model Resumes, Agent Resumes are **rebuilt from scratch, never appended**, to avoid
recency bias. They are regenerated in batched curation runs from completion reviews and
promoted entries in [`../learnings/golden/README-LEARNINGS-GOLDEN.md`](../learnings/golden/README-LEARNINGS-GOLDEN.md). The rebuild logic is
**right-rail tooling** in `agentic_rails_tooling`; this repository holds only the
resulting artifacts.

## Files

- [AGENT_RESUME_TEMPLATE.md](AGENT_RESUME_TEMPLATE.md) - copy this to create a Resume for an agent persona.
