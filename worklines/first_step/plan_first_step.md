---
**Status**: 📋 Draft
**Date**: 2026-11-24
**Priority**: High
**Maintained**: Yes
---

# From the vision to a first seed (workline `first_step`)

## Overview

Hakoni starts here. This workline turns the [vision](docs/0000_01_vision.md) into a first working **seed**: an onboarding Markdown file behind a Git link which, given to Claude Code or Codex, lets the agent build and start a sandboxed container, connect to it, join the Hakoni repository and contribute. Hakoni is its own first use case.

**Done means:** first, from the seed reached from the site, the local Claude Code on Michaël's workstation sets up Hakoni in an OpenShell box and makes a contribution from inside it ([scope](docs/0101_01_demonstrator-scope.md), tests T1–T9); then Codex runs the same seed on a second install; the first security checks pass; the differences between clients are documented. Distributing a seed to other people is the next workline, not this one.

## Workline directory layout

- Plan (this file): `worklines/first_step/plan_first_step.md`
- Produced docs: `worklines/first_step/docs/`
- Scripts (if needed): `worklines/first_step/scripts/`
- Tests: `test/first_step/`

## Goals ✅

- Fix the scope and success criteria of the first demonstrator.
- Know the runtime facts before building: the sandbox runtime, the container engine, how each agent client connects to a container and discovers instructions and skills.
- Write the seed v0 and the container recipe v0 for Hakoni itself.
- Run the dogfooding test with two agent clients and record what happens.
- Run the first security checks.

## Non-goals ❌

- No distribution to other people, and no business use case, before the security checks pass.
- No architecture, file name or runtime fixed in advance: each is decided in its step, with evidence.
- No publication of private experience: the vision stays generic (see `AGENTS.md`, "Public repository").
- No other logo variant in the repository: only the chosen logo (`site/assets/hakoni-logo.svg`).
- No talk or meetup preparation.

## Key decisions / assumptions

- 2026-11-24 (Michaël): the project is called **Hakoni**; repository `mdussere/hakoni`, public, Apache 2.0.
- 2026-11-24 (Michaël): English for the repository; worklines and skills follow the usual conventions (`workline`, green light `🟩 go Step N`, `docs/LOGBOOK.md`).
- 2026-11-24 (Michaël): the logo is the closable-box logo in the **contemporary Japan** palette (indigo, vermilion, off-white). It goes into the repository (`site/assets/hakoni-logo.svg`) and the README. Its inspiration photo is not attributed yet (possibly Kurumi Garden, unconfirmed): see Risks.
- 2026-11-24 (Michaël): a GitHub Pages site lives in `site/`, deployed by a GitHub Actions workflow; it starts with one introduction page to the concept.
- 2026-11-24 (Michaël): the first workline is `first_step`, with the vision as `docs/0000_01_vision.md`.
- 2026-11-24 (Michaël): the first coding agent is **Claude Code alone, locally** in VS Code, working **directly on `main`**, talking to Michaël **in the console only**; its first mission is **Step 1**. Onboarding: `docs/0000_02_onboarding.md`. Codex comes later (Step 5 at the latest).
- 2026-11-24 (Michaël): the vision is reviewed (1.1 done), after his own edits and dictations (§ 2, § 4 scenario, § 6 structure).
- 2026-11-24 (Michaël): the first demonstrator starts from the seed, reached from the site, and makes a contribution to Hakoni from inside the box; it runs on this workstation, from the local Claude Code in VS Code, with OpenShell. Acceptance: a contribution that comes from the box. Details: [docs/0101_01_demonstrator-scope.md](docs/0101_01_demonstrator-scope.md).
- 2026-11-24 (Michaël): vocabulary: a Hakoni is a germinating project, the seed is its starting point, the box is the container and the sandbox; otherwise the generic agentic terms (agent, harness…).
- 2026-11-25 (Michaël): instead of a paper survey, Step 2 tests the runtime with a trial seed, **seed 0**, given to the local Claude Code. It goes as far as the SSH access and a clone of Hakoni inside the box. For this trial only, the box is fully open to the Internet, holds no keys, and Claude Code inside it logs in interactively. These are temporary exceptions to the default sandbox rules in `AGENTS.md`; Steps 4 and 6 restore them.
- Assumption: OpenShell is the first sandbox runtime to evaluate, from the author's experiments; Docker or an equivalent as the container engine; SSH as the first connection path. All three are checked in Step 2, not assumed.
- Assumption: skills live once in `skills/`, linked from `.claude/skills/` (Claude Code) and `.agents/skills/` (Codex). Whether Codex discovers them there is checked in Step 2.

## Risks / constraints

- **The host bootstrap is a trust boundary.** Installing prerequisites and building a container act outside the sandbox. Mitigation: the seed asks before every installation or privileged command, and points to reviewed, versioned recipes only.
- **Client differences.** Claude Code and Codex may differ on skill discovery, SSH, permissions and approvals. Mitigation: measure them in Step 2 and Step 5; do not promise equivalence before the test.
- **Untrusted input.** The seed itself is fetched content. Mitigation: the seed tells the agent what it will do and asks before acting; no "curl | sh".
- **Public repository.** Mitigation: the public check in `commit-prep`; fictional examples only.
- **Logo provenance.** The logo was drawn with an AI assistant from a photo of a miniature garden whose author is not confirmed. Mitigation: no copy of the photo in the repository; confirm the inspiration and that the logo is not a derivative before any wider use (talk, branding).
- **Seed 0 is open by design.** An open network and an interactive login in the box are only for a trial on Michaël's own machine. Mitigation: no secrets go in the box, the trial is not distributed, and it is not the seed v0. OpenShell's documentation (v0.1.10) shows no "allow all" policy and documents only an API key for Anthropic, so both points may fail; the experience report records it.
- **The runtime may not fit.** If OpenShell does not suit the use, Step 2 records why and proposes an alternative before Step 4.

## 🚦 Green light protocol

- Default: planning and docs only until Michaël explicitly says **"🟩 go Step N"**.
- If the scope changes materially, update this plan before proceeding, with a logbook entry.

## Plan steps (no execution until green light)

### Step 0 — Repository bootstrap
**Status**: ✅ Done (2026-11-24)
- [x] 0.1 `AGENTS.md` (instructions for every client), `CLAUDE.md` pointing to it, `README.md`.
- [x] 0.2 Skills in `skills/` (`workline`, `commit-prep`, `shared-chat-discussion`, `clean-verbatim`), linked from `.claude/skills/` and `.agents/skills/`.
- [x] 0.3 `pyproject.toml` with uv (Playwright and pytest in dev), `.gitignore`, `docs/LOGBOOK.md`.
- [x] 0.4 This workline and the [vision](docs/0000_01_vision.md), anonymised for a public repository.
- [x] 0.5 The founding discussion captured and cleaned: [docs/0000_03_hakoni-founding-discussion.md](docs/0000_03_hakoni-founding-discussion.md), translated and anonymised. The raw export and the logo SVGs stay in `worklines/first_step/private/`, ignored by Git.
- [x] 0.6 The logo (contemporary Japan palette) in the README, and a GitHub Pages site: `site/index.html` (introduction to the concept) and `.github/workflows/pages.yml`.

### Step 1 — Frame the first demonstrator
**Status**: ✅ Done (2026-11-24)
- [x] 1.1 Michaël reviews the vision: corrections, what to keep, what to drop.
- [x] 1.2 Write the demonstrator's scope: what the seed must achieve, for whom, on which machines (Michaël's workstations), with which agent clients and versions.
- [x] 1.3 Turn the success criteria (vision § 7) into a checklist of observable tests, each with its expected evidence.
- [x] 1.4 Decide the vocabulary to use in the repository (seed, plant, grow, or other).
- **Acceptance:** `docs/0101_01_demonstrator-scope.md` approved by Michaël.

### Step 2 — Seed 0: a full-scale trial of the runtime
**Status**: 🧪 In progress
- [x] 2.1 Write seed 0, [docs/0201_01_seed-0.md](docs/0201_01_seed-0.md). From the host, the local Claude Code installs the prerequisites with Michaël's approval, builds a minimal image and creates an OpenShell box open to the Internet. It then prepares the SSH access and clones Hakoni over HTTPS inside the box. No keys or tokens go into the box: Claude Code inside it logs in interactively.
- [x] 2.2 Michaël commits and pushes seed 0, then gives its link to a fresh local Claude Code session.
- [x] 2.3 Experience report, [docs/0202_01_seed-0-experience-report.md](docs/0202_01_seed-0-experience-report.md), written afterwards with Michaël from the agent's end-of-run summary: commands, approvals, failures, and the OpenShell facts observed against its documentation (install, container engine, policy, SSH, login inside the box). The host agent commits nothing.
- [ ] 2.4 From the trial: how Claude Code finds instructions and skills inside the box, and what is left to check for Codex.
- [ ] 2.5 Recommendation for the v0 runtime and connection path, with the alternatives considered.
- **Acceptance:** from seed 0 alone, `ssh <box alias>` from the host opens a shell in the box, Hakoni is cloned there, and Claude Code runs logged in inside it; the experience report is written; Michaël chooses the v0 runtime.

### Step 3 — Seed v0
**Status**: ⏳ Pending
- [ ] 3.1 Write the seed: mission, prerequisites check, every action announced before it runs, approvals requested, no blind remote execution.
- [ ] 3.2 Join flow: clone, branch per contributor or agent, working folder, where the plan is, how to log work.
- [ ] 3.3 Continuity: what the agent does at each restart (fetch, review changes, resume).
- [ ] 3.4 A review of the seed as untrusted input: what could a malicious or modified seed make an agent do?
- **Acceptance:** the seed file in the repository; a dry read by each client, which summarises what it would do, matches the intent.

### Step 4 — Container recipe v0
**Status**: ⏳ Pending
- [ ] 4.1 Recipe (for example a Dockerfile) with the minimal tools for working on Hakoni: Git, Python, uv.
- [ ] 4.2 Sandbox policy v0: allowed files and mounts, allowed network destinations, no Docker socket, resource limits.
- [ ] 4.3 Identity and secrets: the user's own Git identity and credentials, provided at run time, never in the image or the repository.
- [ ] 4.4 Workspace layout: what is rebuildable, what persists, what is shared through Git.
- **Acceptance:** the container builds and starts on one machine; an agent connects and runs `uv run pytest` inside it; tests in `test/first_step/`.

### Step 5 — Dogfooding test: Hakoni builds Hakoni
**Status**: ⏳ Pending
- [ ] 5.1 Claude Code receives the seed link on install A (Michaël's workstation): tests T1–T9 of the [scope](docs/0101_01_demonstrator-scope.md).
- [ ] 5.2 Codex receives the same link on install B: same checklist, as the next run after 5.1.
- [ ] 5.3 Record each manual step, approval and failure, and the differences between clients.
- [ ] 5.4 Fix the seed or the recipe, and rerun until both clients pass, or record why one cannot.
- **Acceptance:** `docs/0501_01_dogfooding-report.md` with the evidence for each criterion of Step 1.

### Step 6 — First security checks
**Status**: ⏳ Pending
- [ ] 6.1 Out-of-policy file access, network call and command are refused (one test each).
- [ ] 6.2 No secret in the image, the repository or the logs.
- [ ] 6.3 An instruction added to the seed or a skill cannot raise permissions or leave the sandbox.
- **Acceptance:** repeatable tests in `test/first_step/` and `docs/0601_01_security-checks.md`; open risks listed for the next workline.

---

## Progress tracking ✅

- ✅ Step 0 — Repository bootstrap
- ✅ Step 1 — Frame the first demonstrator
- 🧪 Step 2 — Seed 0: a full-scale trial of the runtime
- ⏳ Step 3 — Seed v0
- ⏳ Step 4 — Container recipe v0
- ⏳ Step 5 — Dogfooding test: Hakoni builds Hakoni
- ⏳ Step 6 — First security checks

---

## Time log

| Timestamp (`date`) | Update |
|---|---|
| Wed 25 Nov 2026 | Seed 0 run by the local Claude Code (2.2): OpenShell 0.1.10 with rootless Podman, box reachable over SSH, Hakoni cloned inside. Experience report written (2.3). |
| Wed 25 Nov 2026 | Step 2 reshaped into a full-scale trial: seed 0 written (`docs/0201_01_seed-0.md`), from OpenShell's documentation v0.1.10. Waiting for Michaël's commit and run. |
| Tue 24 Nov 2026 | Step 1 done: Michaël approved the demonstrator scope, with its three proposals (Codex as the next run, a `Hakoni-Instance` commit trailer as proof of origin, essence/instance/persistence kept as vision terms). "Done means" updated. |
| Tue 24 Nov 2026 | Step 1: 1.1 done (vision reviewed by Michaël). Demonstrator scope drafted in `docs/0101_01_demonstrator-scope.md` (1.2 to 1.4). |
| Tue 24 Nov 2026 | Step 0.6: logo chosen by Michaël (contemporary Japan) and added to the README; `site/` with an introduction page and a Pages deployment workflow. |
| Tue 24 Nov 2026 | Step 0.5: founding ChatGPT discussion captured with the scraper (329 messages), cut to the Hakoni part, cleaned into `docs/0000_03_hakoni-founding-discussion.md`; raw export and logo SVGs kept private (`private/` in `.gitignore`). |
| Tue 24 Nov 2026 | Workline created. Step 0 done: repository conventions, skills, uv, logbook; vision written from Michaël's notes, anonymised. Steps 1–6 drafted, waiting for review. |

---

## Workline artifacts

- [docs/0000_01_vision.md](docs/0000_01_vision.md): what Hakoni tries to show, where the idea comes from, success criteria and open questions.
- [docs/0000_03_hakoni-founding-discussion.md](docs/0000_03_hakoni-founding-discussion.md): how the idea and the name came about (Agent X → Seed → Hakoni), Michaël's decisions versus the assistant's proposals.
- [site/index.html](../../site/index.html): public introduction page to the concept, published with GitHub Pages.
- [docs/0101_01_demonstrator-scope.md](docs/0101_01_demonstrator-scope.md): what the first demonstrator proves, where, with which client; tests T1–T9 with their evidence; vocabulary. Approved.
- [docs/0202_01_seed-0-experience-report.md](docs/0202_01_seed-0-experience-report.md): what happened when seed 0 ran, OpenShell facts against its docs, what the seed v0 should change.
- [docs/0201_01_seed-0.md](docs/0201_01_seed-0.md): trial seed for the local Claude Code: install OpenShell, create an open box, prepare SSH, clone Hakoni inside.
- [docs/0000_02_onboarding.md](docs/0000_02_onboarding.md): onboarding for the coding agent (Claude Code, local, on `main`, console only); first mission: Step 1. Also the first draft of a seed.
