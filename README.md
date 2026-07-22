---
name: agentic-rails-memory-readme
description: Landing page for agentic-rails-memory, the records office of Agentic Rails - a durable, git-revisioned, markdown-only store of cross-project executed-work knowledge.
metadata:
  version: "1.1"
  agentic_rails_source_version: "2.1"
  owner: "Jarryd Adaens"
  repo: "agentic-rails-memory"
---
# Agentic Rails Memory

This repository is the **records office** of [Agentic Rails](AGENTIC_RAILS_README.MD).

```text
Left rail  = context   (agentic_rails_context_starter)
Right rail = tooling   (agentic_rails_tooling)
Carriage   = the project being built
Driver     = the human
Records office = memory  (this repository, beside the line)
```

A railway keeps records in a building beside the line: the logbook of every run, and a
service file for every crew member. Here, **learnings are the logbook** and **Resumes are
the service files**. The left rail gives a project its context; the right rail gives
agents their behaviour; this repository gives the whole framework a durable place to
remember what actually happened — and, later, to turn that record into better routing
decisions. It is a record system, not part of the track: the train runs the same whether
or not anyone walks in.

## What this repository is

A durable, **git-revisioned, markdown-only** place where skills and agents running under
Agentic Rails deposit artifacts they want to keep **across projects**. An agent
"memorizes" something by writing a file here.

> Depositing an artifact does **not** mean it is clean, curated, or ready for another
> agent in another project to consume. It only means something of value was stored
> outside the project where the work happened. Curation comes later.

This repository will grow its own durable tooling to analyze the deposited artifacts and
push findings back into the context and tooling rails — so future framework decisions
become **data driven** rather than gut feel.

## Notice About Learning

I do not know if this is the correct approach to doing agent memory. This is my own exploration of a memory mechanism that exists across different projects. 

Here's my vibe about learning this stuff: Experiment. Try it out, *be curious*.

There's other, more opinionated and mature memory solutions out there. They're probably awesome. Here's one that I saw a few months ago at AIE in Melbourne:

https://www.agent-memory.dev/

Anway, best of luck to you and me both!

## Layout

```text
agentic-rails-memory/
|-- context/            # design and reference material about THIS project
|   |-- design.md       # design document - start here for the full picture
|   `-- wiki/
|-- memories/           # the artifact store
|   |-- learnings/      # session-end learnings: new -> archived -> golden
|   |-- model-resumes/  # one Resume per model
|   `-- agent-resumes/  # one Resume per agent persona
|-- AGENTIC_RAILS_README.MD   # the framework overview
`-- README.md
```

## The feedback loop

```text
work done -> capture (learnings/new) -> curate (archived + golden) -> rebuild Resumes -> orchestrator routes next task -> better work
```

- **Capture is cheap and frequent.** A session-end skill drops one markdown file per
  session into `memories/learnings/new/`.
- **Curation is deliberate and batched.** A scheduled run ranks learnings, archives them,
  promotes the best to `golden/`, and rebuilds the Resumes.
- **Consumption is trustable.** The orchestrator reads Resumes — built from *your* real
  executed work, not vendor benchmarks — to pick model, agent persona, and reasoning
  level.

## Core principles

- **Markdown over infrastructure** — plain files in git, no DB, no runtime service.
- **Intentional, not exhaustive** — memory is curated, not a firehose.
- **Project isolation by default** — cross-project promotion is deliberate.
- **Data over gut** — Resumes are built from real work, never model cards.
- **Capture cheap, promotion deliberate** — frequent writes, rare rebuilds.

## Where to go next

- [`context/design.md`](context/design.md) - the full design of the memory system.
- [`memories/README-MEMORIES.md`](memories/README-MEMORIES.md) - how the artifact store is organized.
- [`memories/model-resumes/README-MODEL-RESUMES.md`](memories/model-resumes/README-MODEL-RESUMES.md) - Model Resumes.
- [`memories/agent-resumes/README-AGENT-RESUMES.md`](memories/agent-resumes/README-AGENT-RESUMES.md) - Agent Resumes.
- [`AGENTIC_RAILS_README.MD`](AGENTIC_RAILS_README.MD) - the framework that ties the rails and the records office together.

## Scope boundary

This repository holds **artifacts only**. The logic that captures, curates, ranks, and
rebuilds them is **right-rail tooling** in `agentic_rails_tooling`. Keeping the data here
and the logic there keeps memory vendor-agnostic, auditable, and easy to reason about.

## License

This repository is licensed under the [Apache License 2.0](LICENSE).
See [NOTICE](NOTICE) for attribution details.
