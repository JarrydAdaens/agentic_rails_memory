# Synthesis Guide

Detailed reference for `rebuild-model-resumes`: how to turn session-learning files into a
Model Resume, and how to rank learnings for the golden set. Keep `SKILL.md` as the
workflow; consult this when actually filling sections or promoting golden.

## Learning file anatomy

Session-learning files (`memories/learnings/new/<project>_<date>.md`) carry two layers of
signal:

**Frontmatter (structured):**

- `models_used:` — a list; each entry has `name`, `provider`, `role`, `reasoning`
  (reasoning / non-reasoning), `thinking_effort`, `fast_mode`, and a free-text `notes`.
  The same model can appear multiple times under different roles (orchestration /
  execution / review).
- `agents_used:` — persona/skill names (feeds Agent Resumes, not model Resumes; ignore for
  this skill except as context).
- `outcome:`, `session_scope:`, `harness:`, `token_cost:`, `date`.

**Body (prose):** `Summary`, `What Went Well`, `What Struggled`,
`Per-Model / Per-Agent Notes`, `Candidate Signals`, `Evidence Reviewed`. The
**Per-Model / Per-Agent Notes** section is the richest per-model signal — mine it first.

## Section-by-section map: learning → Resume

Fill each template section from these sources. If the template changes, follow the
template; this map is guidance for the current template shape.

| Resume section | Draw from |
| --- | --- |
| **Snapshot** — model + provider | `models_used[].name` / `.provider`, normalized to the canonical id. |
| **Snapshot** — corpus size | Count of learning files that mention this model (not total files). |
| **Snapshot** — date range | Min/max `date` across the learnings that mention this model. |
| **Snapshot** — freshness target | State the current policy or "open parameter — no target set yet" if none is defined. Do not invent a number. |
| **Strengths** — complexity ranges | Successful `outcome` sessions + `What Went Well` items attributed to this model; note the work's apparent complexity (milestone-scale vs single story). |
| **Strengths** — work types | Roles the model held well (`role` field) cross-referenced with positive Per-Model notes: planning, refactor sweeps, research spikes, tight bug fixes, inline multi-story runs, etc. |
| **Weaknesses** — complexity it struggles with | `What Struggled` items and negative Per-Model notes tied to this model. |
| **Weaknesses** — effort threshold | Only if the corpus shows a degradation point (e.g. "heavy first run ~149k tokens then efficient resumes"). Otherwise "not observed in corpus". |
| **Weaknesses** — failure modes | Recurring mistakes in Per-Model notes: hallucination/fabrication, scope creep, lost context, first-pass interop errors needing empirical correction, etc. |
| **Empirical Metrics** — token efficiency | `token_cost` + any per-task figures in notes. Most sessions are unmeasured — say "unmeasured" rather than guessing. |
| **Empirical Metrics** — speed | Only if the corpus mentions throughput/latency; else "not recorded". |
| **Empirical Metrics** — estimate accuracy | Any notes on estimate-vs-actual (complexity/effort ratings landing right). Else "not recorded". |
| **Empirical Metrics** — error patterns | The recurring, cross-session mistakes (distinct from one-off failure modes). |
| **Routing Guidance** — prefer for | Synthesize from strengths + explicit model-routing candidate signals (e.g. "Fable for research/decision tasks", "single strong inline agent beats boss+subagent for small coupled stories"). |
| **Routing Guidance** — avoid for | Synthesize from weaknesses + failure modes. |
| **Routing Guidance** — pair with mitigation | Conditions where the model works but needs a guardrail (e.g. "empirical correction loop expected on Windows interop — budget live-run cycles"). |
| **Source Reviews** | Relative-path links to every learning file this Resume drew from. Note these are session-learnings standing in for completion reviews during bootstrap. |

## Attribution rules

- Attribute an observation to a model only when the evidence names that model (or the role
  is unambiguous in a single-model session). Do not spread one model's failure across
  every model in a multi-model session.
- Use the `role` field to separate boss/orchestration signal from execution and review
  signal — they are different track records for the same model and both belong in the
  Resume, labelled.
- When two learnings disagree, keep both observations; a Resume can hold nuance ("strong at
  X in run A, needed correction on Y in run B"). Do not average away real signal.
- Prefer **Verified** claims (many learnings tag items Verified / Inferred). Down-weight
  Inferred claims and say so if a Resume leans on them.

## Golden-ranking rubric (curate mode only)

Score each `new/` learning; promote the top-scoring, high-signal files to `golden/` and
archive the rest. Golden must stay small and stable — promote sparingly.

**Score higher (promote):**

- Failures, blocked-not-failed calls, and near-misses — the expensive lessons.
- Surprising successes or surprising cheapness (routing insights).
- Sharp edge cases and reusable rules (high-priority Candidate Signals).
- Sessions with strong, specific model-routing signal (clear prefer/avoid evidence).
- Sessions covering a model or role thinly represented elsewhere (coverage value).

**Score lower (archive, don't promote):**

- Routine successes that merely confirm known behavior.
- Sessions with vague or already-captured signal.
- Duplicative runs when a stronger learning already covers the same model + lesson.

**Promotion discipline:** a learning is either promoted to `golden/` or archived to
`archived/` — every processed `new/` file moves exactly once. When a new golden learning
supersedes an older one, note it for retirement rather than letting golden grow unbounded
(supersession policy is an open question in the memory repo's `context/design.md`).
