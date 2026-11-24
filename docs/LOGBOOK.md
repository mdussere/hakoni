# Logbook

## 2026-11-24

- **Repository bootstrapped** (workline `first_step`, Step 0).
  - Why: Michaël created `mdussere/hakoni` (public, Apache 2.0) to explore packaging, sandboxing and handing over small agentic work environments, and to develop Hakoni with Hakoni.
  - `AGENTS.md` holds the instructions for every agent client; `CLAUDE.md` points to it. English everywhere; public-repository rules (no confidential names, tools or data).
  - Skills in `skills/`, linked from `.claude/skills/` and `.agents/skills/`: `workline` (mandatory), `commit-prep`, `shared-chat-discussion`, `clean-verbatim`, adapted from Michaël's other repositories.
  - `pyproject.toml` with uv (Playwright and pytest as dev dependencies). `uv sync` OK; `uv run python -c "import playwright"` OK.
- **Workline `first_step` created** ([plan](../worklines/first_step/plan_first_step.md)): from the vision to a first seed tested with Claude Code and Codex. Steps 1–6 drafted, nothing executed.
  - Vision written from Michaël's private notes, translated and anonymised for a public repository: [0000_01_vision.md](../worklines/first_step/docs/0000_01_vision.md). The private original stays outside this repository.
- **Onboarding for the coding agent** written: [0000_02_onboarding.md](../worklines/first_step/docs/0000_02_onboarding.md).
  - Michaël's choices: Claude Code alone, locally in VS Code, directly on `main`, console only; first mission: Step 1 (after its green light).
  - No earlier agent-onboarding file existed in Michaël's local repositories to reuse (tanstaafllm has mandate and foundation notes, not onboarding files). This file is also the first draft of a Hakoni seed; the agent is asked to report what was missing in it.
- **Founding discussion captured** (workline `first_step`, Step 0.5): [0000_03_hakoni-founding-discussion.md](../worklines/first_step/docs/0000_03_hakoni-founding-discussion.md).
  - Why: Michaël asked to keep the ChatGPT discussion where Hakoni was born, to complete Step 0.
  - Command: `uv run python skills/shared-chat-discussion/scrape_shared_chat.py --url <share link> --output <scratchpad>` → `Exported 329 messages`, no "Show more" left.
  - The earlier part (unrelated, private) is dropped; the export starts at the agent-packaging message. The rest is translated, cleaned and anonymised: no employer, colleague, internal project or internal tool names. Per Michaël, the experiments described are personal experiments, not part of any employer's work.
  - The raw export and the logo SVGs (copied from Michaël's private ideas repository) are kept in `worklines/first_step/private/`, now ignored by Git (`private/` in `.gitignore`), consistent with the "no logo until its inspiration and rights are clear" non-goal. The share link is not written in the repository.
- **Logo and GitHub Pages site** (workline `first_step`, Step 0.6).
  - Michaël's decision: the closable-box logo in the contemporary Japan palette goes into the repository and the README. This replaces the non-goal "no logo until its inspiration and rights are clear"; the unconfirmed inspiration is now listed as a risk in the plan.
  - `site/` holds the Pages site: `index.html` (introduction to the concept) and `assets/hakoni-logo.svg`. `.github/workflows/pages.yml` deploys `site/` on pushes to `main` that touch it.
