# Onboarding — the coding agent working on Hakoni

**Question this document answers:** you are a coding agent joining Hakoni. What is the project, how do you set up, how do you work with Michaël, and what is your first mission?

Written on 24 November 2026 for **Claude Code**, working locally in Michaël's VS Code on `~/git/hakoni`, on the `main` branch. Michaël, the project owner, gives you this file as your starting point.

This file is also the first draft of what Hakoni calls a *seed*: an onboarding document an agent reads to join a project. Note what is missing or unclear in it as you go; that feedback is part of the work (see § 7).

## 1. The project in a few lines

Hakoni explores how to package, sandbox and hand over a small, self-contained agentic work environment, so that someone can receive it from **one link** and use it with a general-purpose coding agent. The name comes from *hakoniwa* (箱庭), a miniature garden in a box.

Nothing is built yet. The project is meant to become its own first use case: Hakoni should eventually be developed from inside a Hakoni environment.

## 2. Read, in this order

1. [AGENTS.md](../../../AGENTS.md): the repository rules. `CLAUDE.md` loads it for you.
2. [README.md](../../../README.md): the public summary.
3. [The vision](0000_01_vision.md): the experiences behind the idea, hypotheses, success criteria and open questions. **Hypotheses, not decisions.**
4. [The plan](../plan_first_step.md), completely: goals, non-goals, steps and acceptance criteria.
5. [The workline skill](../../../skills/workline/SKILL.md): how plans, docs and the logbook are kept.
6. [docs/LOGBOOK.md](../../../docs/LOGBOOK.md): what has happened so far.

## 3. Set up

```bash
cd ~/git/hakoni
git pull
uv sync
uv run python -c "import playwright; print('ok')"
```

- Python is managed with **uv** only. Repeatable tests go in `test/<workline>/` and run with `uv run pytest`.
- Do not install anything on the machine (system packages, Docker, a sandbox runtime) without asking Michaël first, with the exact command.

## 4. How we work

- **Branch:** work directly on `main`. Run `git pull` at the start of each session, and read what changed since your last session (`git log`, the plan, the logbook) before resuming.
- **Communication:** in the console only. Michaël often writes or dictates in French: answer him in French. Everything you write in the repository is in English.
- **Notes:** your decisions, evidence and results go into the workline: the plan (checkboxes, time log) and docs in `worklines/first_step/docs/`, named as the `workline` skill says. Every material change gets a dated entry in `docs/LOGBOOK.md`.
- **Green light:** planning and writing docs are allowed. Implementing a plan step needs Michaël's explicit **`🟩 go Step N`**. If the scope changes, update the plan first.
- **Commits:** only when Michaël asks, with the `commit-prep` skill (explicit file list, public check, then push).
- **Facts:** separate what Michaël decided, what you observed (with the command or source), and what you assume. Never present a hypothesis from the vision as a decision.

## 5. Public repository

`mdussere/hakoni` is **public**. Never write the names of Michaël's employers, colleagues, clients or internal projects, nor their internal tools, configurations or data. The vision describes his experience generically on purpose: keep it that way, and use fictional examples. No secrets, tokens or `.env` files. If unsure, leave it out and ask.

## 6. Your first mission: Step 1 of the plan

Step 1, "Frame the first demonstrator", is in the plan. Michaël has chosen it as your first mission. It still needs his `🟩 go Step 1` before you write its deliverable.

1. Read everything in § 2. Then give Michaël a short summary: what you understood of Hakoni, the first demonstrator and the plan, and your questions.
2. Once he gives the go, review the vision with him (1.1): what to correct, keep or drop. Record his answers.
3. Draft `worklines/first_step/docs/0101_01_demonstrator-scope.md` (1.2 to 1.4):
   - what the first seed must achieve, for whom, on which machines, with which agent clients and versions;
   - the success criteria of vision § 7 turned into a checklist of observable tests, each with its expected evidence;
   - the vocabulary to use (seed, plant, grow, or other), as a proposal.
4. Ask Michaël to approve it. Step 1 is done when he does: tick the boxes, update the progress tracking and the time log, add a logbook entry.

Do not start Step 2 (the runtime survey) without its own go.

## 7. Feedback on this onboarding

At the end of your first session, add a short section to the logbook entry: what in this file was missing, wrong or unclear, and what you had to work out yourself. Hakoni is about making onboarding by link work; this is its first test.

## 8. When you are stuck

Stop and ask Michaël in the console, with what you tried and what you need. Do not guess around a permission, an installation or a confidentiality doubt.
