---
name: rebuild-model-resumes
description: >-
  Use this when you need to (re)build the per-model Resumes in
  memories/model-resumes/ from the session-learning corpus in memories/learnings/.
  Runs in two modes: the default curate mode ranks and archives processed learnings,
  promotes the strongest to golden, and rebuilds every Resume from golden + new; a
  bootstrap mode (say "bootstrap" or "no archive") rebuilds Resumes from the new/
  learnings only and touches nothing else — for first runs and workflow testing.
metadata:
  version: "1.0"
  agentic_rails_source_version: "2.1"
  owner: "Jarryd Adaens"
  repo: "agentic-rails-memory"
---

# Rebuild Model Resumes

Build one **Model Resume** per model from the session-learning corpus, following the
[`../../../memories/model-resumes/MODEL_RESUME_TEMPLATE.md`](../../../memories/model-resumes/MODEL_RESUME_TEMPLATE.md)
shape. A Resume is empirical, workflow-specific track record for one model — the inverse
of a vendor model card. Read
[`../../../memories/model-resumes/README-MODEL-RESUMES.md`](../../../memories/model-resumes/README-MODEL-RESUMES.md)
for the framing before your first run.

**Core law: rebuild, never append.** A Resume is regenerated from scratch over its whole
source corpus every run. Never edit a Resume by hand and never bolt the latest session
onto an old Resume — that would over-weight recent work (recency bias).

> Scope note: this skill is a **local bootstrap tool** living in the memory repo so a
> Claude instance booted here can run it directly. The memory repo's charter is
> "artifacts only; the rebuild logic is right-rail tooling." Once this workflow is proven
> and automated on a schedule, the logic should migrate to `agentic_rails_tooling`. Treat
> this copy as the seed, not the permanent home.

## Modes

Pick the mode from the invocation. Default to **curate** unless the user signals bootstrap.

| Mode | Trigger | Reads | Archives? | Golden? | Use when |
| --- | --- | --- | --- | --- | --- |
| **Bootstrap** | "bootstrap", "no archive", "don't archive", "dry", "test" | `learnings/new/` only | No | No | First-ever batch; testing the workflow before automation. Leaves the corpus untouched so you can rerun freely. |
| **Curate** (default) | no qualifier, or "curate", "full", "archive" | `learnings/golden/` + `learnings/new/` | Yes | Yes (promotes best) | Steady-state batched rebuild. Processes `new/`, keeps the golden set, files the rest. |

If the invocation is ambiguous and this is the first run against a corpus, ask which mode;
otherwise default to curate.

## Inputs

- Corpus: the `*.md` session-learning files under `memories/learnings/new/` (and, in curate
  mode, `memories/learnings/golden/`). Ignore each folder's `README-*.md`.
- Template: `memories/model-resumes/MODEL_RESUME_TEMPLATE.md` — **read it at runtime** and
  mirror its exact section order and headings. Do not hardcode the structure from this
  skill; the template is the source of truth and may change.
- Output dir: `memories/model-resumes/` — one Resume file per model.

## Workflow

### 1. Load and read the corpus

- Bootstrap mode: read every `*.md` in `learnings/new/` except `README-LEARNINGS-NEW.md`.
- Curate mode: read those plus every `*.md` in `learnings/golden/` except its README.
- Read the resume **template** now so every Resume you emit matches its current shape.

### 2. Extract per-model signal

Each learning file carries structured frontmatter (`models_used:` with role, reasoning,
thinking effort, fast mode, notes; `token_cost:`) and a body with **Per-Model / Per-Agent
Notes**, **What Went Well**, **What Struggled**, and **Candidate Signals**. Pull the
model-specific evidence from both. See
[`references/synthesis-guide.md`](references/synthesis-guide.md) for the field-by-field map
from learning sections to Resume sections and the golden-ranking rubric.

### 3. Normalize model identity

Group evidence by **canonical model id**, not by the raw string. The corpus is
inconsistent (`gpt-5`, `GPT-5` are the same model). Rules:

- Lowercase; collapse to a stable kebab-case id (`GPT-5` → `gpt-5`,
  `claude-fable-5` → `claude-fable-5`).
- Keep vendor-distinct versions separate (`claude-haiku-4-5-20251001` and
  `claude-opus-4-8` are different models; do not merge across version or family).
- One memory can feed **several** Resumes — a boss+worker session naming Fable and Opus
  contributes to both. Attribute each observation to the model it describes, using the
  `role` on each `models_used` entry to keep boss/worker/review signal distinct.

### 4. Rebuild one Resume per model

For every canonical model with at least one observation, write
`memories/model-resumes/<canonical-id>.md` from scratch using the template's sections.
Filename is the canonical id in kebab-case (e.g. `gpt-5.md`, `claude-opus-4-8.md`).

- Fill the frontmatter and every section the template defines; keep its heading order.
- The rebuild banner records **today's date**, the corpus size (number of learnings that
  mentioned this model), and the date range those learnings cover.
- Ground every claim in the corpus. Where the corpus is silent (e.g. thinking effort is
  routinely `unknown`, token cost unmeasured), say so plainly rather than inventing a
  number — honesty of provenance is the point of a Resume.
- **Source Reviews**: link the exact learning files this Resume drew from, by relative
  path. (The template calls these completion reviews; for this bootstrap the learnings are
  the source — link them and note that.)
- Overwrite any existing Resume for the model completely. Never merge with the old file.

### 5. Curate mode only — golden promotion and archiving

Do this **after** all Resumes are written and only in curate mode:

1. **Rank** the `new/` learnings with the rubric in `references/synthesis-guide.md`
   (failures, surprising successes, sharp edge cases, high model-routing signal score
   highest; routine successes score low).
2. **Promote** the strongest — the golden set — by moving those files from `new/` into
   `learnings/golden/`. Golden stays small and high-signal; do not promote routine runs.
3. **Archive** every remaining processed `new/` file by moving it to `learnings/archived/`.
4. After this step `new/` should contain only its README (all files were either promoted
   or archived). Report the split: which files went golden, which went archived.

Bootstrap mode skips this entire step — `new/` is left exactly as found.

### 6. Report

Summarize: mode used, corpus size, list of Resumes written (model → source-learning count),
and — in curate mode — the golden/archived split. Flag any model with thin evidence
(single observation) as low-confidence.

## Output contract

- One `<canonical-id>.md` per model, in `memories/model-resumes/`, matching the template's
  sections and frontmatter, rebuilt from scratch.
- No hand-authored Resume content that isn't traceable to the corpus.
- Bootstrap mode: `learnings/new/` unchanged; `golden/` and `archived/` unchanged.
- Curate mode: `new/` emptied of learnings (README kept), best files in `golden/`, rest in
  `archived/`, with the split reported.

## Validation

Before declaring done, confirm:

- Every Resume matches the **current** template's section order and headings (you re-read
  the template this run).
- Model identities were normalized — no duplicate Resume for the same model under two
  casings, no merge across distinct versions/families.
- Multi-model sessions were attributed to each model they mention, by role.
- Every Resume was fully overwritten (rebuilt), not appended to.
- Each Resume's Source Reviews list resolves to real learning files.
- Bootstrap mode moved/deleted nothing; curate mode's `new/` holds only its README and the
  golden/archived split was reported.
- Provenance gaps (unknown effort, unmeasured tokens) are stated, not fabricated.

## Examples

- `rebuild model resumes` → curate mode: rebuild all Resumes from golden + new, then
  promote golden and archive the rest.
- `rebuild model resumes, bootstrap — don't archive anything` → bootstrap mode: rebuild
  Resumes from `new/` only, leave the corpus untouched.
