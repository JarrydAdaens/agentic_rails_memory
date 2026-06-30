---
name: design
description: Design specification for the Agentic Rails memory system (the third rail) - a durable, git-revisioned, markdown-only store of executed-work knowledge that feeds data-driven orchestration.
metadata:
  version: "1.0"
  agentic_rails_source_version: "2.1"
  owner: "Jarryd Adaens"
  repo: "agentic-rails-memory"
---
# Agentic Rails Memory - Design Specification

This is the design document for **`agentic-rails-memory`**, the **third rail** of the
Agentic Rails framework. It defines *what* the memory system is, *why* it exists, and *how*
it is used. It does **not** define the skills, agents, or context that produce or consume
the artifacts — those live in the other two rails.

## The Third Rail in One Paragraph

Agentic Rails has a left rail (**context**, from `agentic_rails_context_starter`) and a
right rail (**tooling** — skills, rules, agent personas — from `agentic_rails_tooling`).
Neither captures *what was learned by doing the work*, in a place that survives across
projects. This repository is that place: a durable, git-revisioned drop zone where skills
and agents "memorize" things by writing files. In a real railway the third rail is the
electrified rail that supplies power; here, memory is what energizes the system to make
**data-driven** decisions instead of relying on gut feel.

> Depositing an artifact does not mean it is clean or ready for another agent to consume.
> It only means something of value was stored outside the project where the work happened.
> This repository will later grow its own tooling to analyze those deposits and push
> findings back into the context starter and tooling rails, making future framework
> decisions data-driven.

---

## Purpose

`agentic-rails-memory` is a **git-backed, markdown-only, intentionally-curated store of
executed-work knowledge**.

- **What it is:** the central, long-lived memory of the Agentic Rails framework. A place
  agents and skills write artifacts they want to keep across projects.
- **Who it is for:** the human driving Agentic Rails, and the agents working under it,
  across every project that uses the framework.
- **What problem it solves:** model and agent selection today is gut feel guided by vendor
  benchmarks and model cards. There is no durable, self-owned record of how models and
  personas *actually* performed on *real* work. Memory closes that gap.
- **Why this architecture:** plain markdown in git is vendor-agnostic, auditable, and
  zero-infrastructure. Agents read, write, and commit natively. No vector DB, no
  relational DB, no runtime service.

The system powers a **feedback loop**: work happens -> learnings are captured -> curation
distills them into Resumes -> the orchestrator reads Resumes to route the next task ->
better work happens.

## Core Principles

- **Markdown over infrastructure.** Everything is plain markdown in a git repo. No vector
  DB, no relational DB, no runtime service. Vendor-agnostic, auditable, low-friction.
- **Intentional, not exhaustive.** The system does not record everything an agent has ever
  done. Context windows are finite; hoovering everything in is wasteful and corrupts
  signal. Memory is curated.
- **Project isolation by default.** One project's learnings do not contaminate another.
  Cross-project knowledge is promoted deliberately, not by accident.
- **Data over gut.** Resumes are built purely from executed work — not benchmarks, not
  model cards. Real performance on real tasks is the only input.
- **Capture is cheap, promotion is deliberate.** Session-end capture is frequent and
  low-cost. Rewriting a Resume is rare, batched, and intentional, to avoid signal decay
  and corruption from over-frequent regeneration.

---

## System Architecture

### How the Pieces Fit Together

The memory system sits between the two existing rails and the orchestration layer:

- **Producers** (left + right rails): skills and agents working inside any project deposit
  artifacts here at natural checkpoints.
- **Store** (this repository): durable, git-revisioned markdown under `memories/`.
- **Curation** (right-rail tooling): periodically ranks raw deposits, archives them, and
  promotes the best, then rebuilds Resumes.
- **Consumers** (orchestration layer): read Resumes at decision time to route work.

This repository holds **artifacts only**. The logic that captures, curates, ranks, and
rebuilds them is right-rail tooling in `agentic_rails_tooling`. Keeping the data here and
the logic there keeps memory vendor-agnostic, auditable, and easy to reason about.

**External dependencies:**

- `git` - the entire persistence and revision mechanism.
- `agentic_rails_tooling` - hosts the capture, curation, and rebuild logic.
- `agentic_rails_context_starter` - the left-rail template that produces the projects
  whose work generates the artifacts.

### Repository Structure

```text
agentic-rails-memory/
|-- context/                       # design and reference material about THIS project
|   |-- design.md                  # this file
|   `-- wiki/                       # reference notes and cheat sheets
|-- memories/                      # artifacts deposited by agents and skills
|   |-- README-MEMORIES.md
|   |-- learnings/                 # session-end learning artifacts
|   |   |-- README-LEARNINGS.md
|   |   |-- new/README-LEARNINGS-NEW.md
|   |   |-- archived/README-LEARNINGS-ARCHIVED.md
|   |   `-- golden/README-LEARNINGS-GOLDEN.md
|   |-- model-resumes/             # one Resume per model
|   |   |-- README-MODEL-RESUMES.md
|   |   `-- MODEL_RESUME_TEMPLATE.md
|   `-- agent-resumes/             # one Resume per agent persona
|       |-- README-AGENT-RESUMES.md
|       `-- AGENT_RESUME_TEMPLATE.md
|-- AGENTIC_RAILS_README.MD        # durable framework overview (all three rails)
`-- README.md                      # repository landing page
```

The structure is intentionally flexible. New memory types (task reports, model notes,
cross-project references) can be added under `memories/` without redesigning the system.

---

## The Artifacts

### Learnings

The primary artifact store, flowing through a `new -> archived -> golden` lifecycle. A
learning is the raw, self-reflective output of a single work session: what an agent
struggled with, what it did well, the models and personas used, the token cost, and the
task outcome. **One file = one session = one data point.**

A single learning file is never used to directly rewrite a Resume. It is one data point
that *informs* a later rebuild. Regenerating Resumes from individual sessions every time
would corrupt them with noise.

| Stage | Folder | Meaning |
| --- | --- | --- |
| Captured | `learnings/new/` | Freshly deposited, not yet processed. |
| Processed | `learnings/archived/` | Reviewed by curation; full history retained for audit and re-processing. |
| Promoted | `learnings/golden/` | Curated best. Resume rebuilds sample from golden (plus new), never the whole archive. |

### Resumes

A **Resume** is a central, cross-task track record. Picking a model or persona for work is
treated as *hiring for a team*: the Resume is its empirical, your-workflow-specific record,
not a vendor benchmark or model card. Resumes are **rebuilt from scratch, never appended**,
to avoid recency bias.

- **Model Resumes** — one per model (Sonnet, Haiku, Opus, GPT, Qwen, ...). Complexity and
  effort ranges, token efficiency, speed, error patterns, routing guidance.
- **Agent Resumes** — one per agent persona. Task types it handles well or badly, failure
  modes, which underlying models it performs best with, routing guidance.

---

## The Feedback Loop

```text
   +-------------+
   |  Work done  |  (agents execute planning/implementation skills in any project)
   +------+------+
          | session-end capture skill
          v
   +-------------+
   | learnings/  |  raw, timestamped, one file per session
   |    new/     |
   +------+------+
          | curation run (scheduled, e.g. weekly)
          v
   +------------------------------+
   | archived/ + golden/          |  ranked, sorted, best promoted
   +------+-----------------------+
          | Resume rebuild (samples golden + new)
          v
   +----------------------+
   | model + agent Resumes|  kept fresh, never appended
   +------+---------------+
          | read at orchestration time
          v
   +----------------------+
   | Rails orchestrator   |  picks model / agent / reasoning level
   +----------------------+
```

### 1. Capture (write)

At the end of a large work session — which may have cost 100k-300k+ tokens — there is a
natural checkpoint for reflection. A session-end skill self-reflects and writes a learning
file to `memories/learnings/new/`. It captures performance, struggles, wins, models and
personas used, and outcome. It does **not** touch Resumes.

### 2. Curation (manage)

A scheduled run (provisionally weekly) reviews everything:

1. Reads all files in `learnings/new/`.
2. Ranks them (ranking system and evaluators are open questions, out of scope here).
3. Moves processed files to `learnings/archived/`.
4. Promotes the strongest learnings to `learnings/golden/`.
5. Rebuilds model and agent Resumes from golden + new.

This is the **manage** phase that most systems skip and get burned by. It keeps signal
high and prevents decay.

### 3. Consumption (read)

Resumes are read by the orchestration layer at decision time. The memory system's *output*
is fresh, data-backed Resumes the orchestrator can trust.

---

## How the Orchestrator Uses Memory

*(The orchestrator is defined in the right rail; this section only describes the memory
touchpoint.)*

The orchestration layer reads model Resumes (and agent Resumes) to decide, for a given
task: which model (local / Anthropic / OpenAI / ...), which agent persona, which reasoning
level. An optional **prepare step** distills the Resumes into a quick lookup table (for
example, "model X best for implementing code, model Y best for UI") so routing is fast.

The key property: because the data and logic are self-owned, the routing decision is
**trustable**. Unlike vendor auto-modes, there is full transparency into what is being
optimized for — actual measured performance on real work, not hidden server-cost
trade-offs.

---

## Security and Privacy

- **No secrets in artifacts.** Learnings and Resumes must never contain credentials,
  tokens, API keys, customer data, or proprietary source. Capture skills should sanitize.
- **Project isolation.** Learnings are project-scoped by default; cross-project promotion
  is a deliberate curation act, not an automatic side effect.
- **Auditability.** Git history is the audit trail. Nothing is silently deleted; processed
  learnings move to `archived/`, retaining full history.

---

## Open Questions

*(Out of scope for this design; noted for later.)*

- Ranking system and evaluators for the curation run.
- Contradiction / supersession handling: when a golden learning is invalidated by newer
  understanding, how is it retired or overridden?
- Exact Resume schema beyond the current templates.
- Promotion criteria for cross-project (global) vs. project-scoped learnings.
- Cadence and cost controls for Resume rebuilds (cost vs. freshness trade-off).

---

## Navigation

- [../README.md](../README.md) - repository landing page
- [../AGENTIC_RAILS_README.MD](../AGENTIC_RAILS_README.MD) - the three-rail framework overview
- [../memories/README-MEMORIES.md](../memories/README-MEMORIES.md) - overview of the memory store
- [../memories/learnings/README-LEARNINGS.md](../memories/learnings/README-LEARNINGS.md) - the new -> archived -> golden lifecycle
- [../memories/model-resumes/README-MODEL-RESUMES.md](../memories/model-resumes/README-MODEL-RESUMES.md) - Model Resumes
- [../memories/agent-resumes/README-AGENT-RESUMES.md](../memories/agent-resumes/README-AGENT-RESUMES.md) - Agent Resumes
- [wiki/home.md](wiki/home.md) - wiki navigation hub
