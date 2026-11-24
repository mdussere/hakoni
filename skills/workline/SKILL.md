---
name: workline
description: Creates and maintains workline plans under `worklines/<workline_name>/plan_<workline_name>.md` plus supporting docs under `worklines/<workline_name>/docs/`. Use when the user asks for a plan, a workline, a roadmap, a "workplan", or any multi-step effort that should be tracked in this repository.
---

# Workline (worklines planning)

## Goal

Produce a **clear, reviewable plan** that follows this repository's planning conventions:

- The canonical plan lives in `worklines/<workline_name>/plan_<workline_name>.md`.
- Supporting docs live in `worklines/<workline_name>/docs/`.
- Workline-local scripts, if any, live in `worklines/<workline_name>/scripts/`.

## When to create a new workline

Create a new workline when the work is any of:

- a multi-step or multi-file effort with sequencing, risks or acceptance criteria;
- an operational workflow (packaging, onboarding, sandboxing, distribution);
- a discovery or research track that should be preserved as decisions and evidence.

## Workline scaffold

Create:

- `worklines/<workline_name>/plan_<workline_name>.md`
- `worklines/<workline_name>/docs/` (only if the plan references produced docs)
- `worklines/<workline_name>/scripts/` (only if needed)

Add the workline to the table of active worklines in `AGENTS.md`.

## Plan template

Use this structure; adapt the headings, keep the intent.

```markdown
---
**Status**: 📋 Draft
**Date**: YYYY-MM-DD
**Priority**: Low | Medium | High
**Maintained**: Yes
---

# <Title> (workline `<workline_name>`)

## Overview

<1–3 paragraphs: what we are doing, why, and what "done" means.>

## Workline directory layout

- Plan (this file): `worklines/<workline_name>/plan_<workline_name>.md`
- Produced docs: `worklines/<workline_name>/docs/`
- Scripts (if needed): `worklines/<workline_name>/scripts/`

## Goals ✅

- <goal>

## Non-goals ❌

- <non-goal>

## Key decisions / assumptions

- <decision or assumption, with its date and who made it>

## Risks / constraints

- <risk, and its mitigation or explicit limitation>

## 🚦 Green light protocol

- Default: planning and docs only until the user explicitly says **"🟩 go Step N"**.
- If the scope changes materially, update this plan before proceeding.

## Plan steps (no execution until green light)

### Step 1 — <step name>
**Status**: ⏳ Pending | 🧪 In progress | ✅ Done
- [ ] 1.1 <checklist item: actionable and testable>

---

## Progress tracking ✅

- ⏳ Step 1 — <name>

---

## Time log

| Timestamp (`date`) | Update |
|---|---|
| <...> | <...> |

---

## Workline artifacts

- <doc or script path, and the question it answers>
```

### Status conventions

- **📋 Draft** for plans; per step: **⏳ Pending**, **🧪 In progress**, **✅ Done**, **❌ Dropped**.
- Checkboxes are the operational truth; keep the progress tracking in step with them.

## Supporting docs conventions (`worklines/<workline_name>/docs/`)

Filename pattern: `<step>_<num>_<name>.<ext>`

- `<step>`: 4 digits, `<step_idx><substep_idx>` zero-padded (`0101` = substep 1.1, `0204` = 2.4, `1201` = 12.1).
  - `0000`: written **before** plan execution (handoff received, vision, pre-plan decisions).
  - `9999`: written **after** plan execution (close-out, handoff to a follow-up workline).
- `<num>`: 2-digit index within the substep (`01`, `02`, …).
- `<name>`: short kebab-case.

Examples: `0000_01_vision.md`, `0101_01_runtime-survey.md`, `9999_01_handoff.md`.

Each doc states the question it answers and is linked from the plan's "Workline artifacts" section.

## Logbook update (required)

When you create or materially change a workline plan, add an entry to `docs/LOGBOOK.md` under today's date:

- what changed (plan created or updated, docs added, decisions made);
- why it changed (new constraint, new evidence, green light…);
- test results, if code was run (command and outcome).

## Quality bar

- The plan includes **acceptance criteria** in its step checklists, so "done" is unambiguous.
- Explicit non-goals prevent scope creep.
- Short sections, concrete checklists, links to produced docs.
- This repository is **public**: nothing confidential goes into a plan or a doc (see `AGENTS.md`, "Public repository").
