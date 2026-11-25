# Logbook

## 2026-11-25

- **Step 2 reshaped: seed 0, a full-scale trial** (workline `first_step`). Michaël chose to test the runtime by doing, rather than by a paper survey: [0201_01_seed-0.md](../worklines/first_step/docs/0201_01_seed-0.md) is a trial seed for the local Claude Code.
  - The seed asks for approval before every install. It installs a container engine and OpenShell, writes and builds a minimal image, and creates the box `hakoni-seed0`. It prepares the SSH access, clones Hakoni over HTTPS inside the box, then summarises the run in the conversation. It commits nothing; the experience report is written afterwards, with Michaël.
  - Temporary choices for this trial (Michaël): the box is fully open to the Internet, holds no keys, and Claude Code inside it logs in interactively. They are recorded in the plan as exceptions to the default sandbox rules.
  - OpenShell facts taken from its documentation (v0.1.10, the repository now at `ai-openshell/openshell`):
    - the default policy denies all outbound traffic, and no "allow all" policy is documented;
    - for Anthropic, only an API-key provider is documented;
    - `sandbox create --from` takes an image, not a Dockerfile;
    - `sandbox ssh-config` prints an SSH entry.

    These facts are not checked on the machine yet; the trial checks them.
  - Plan: Step 2 now runs 2.1 to 2.5 (2.1 done), with a new risk ("Seed 0 is open by design").

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
- **Pages site deployed**: commits `76cb459`, `7d972cb`, `8500dcd`, `8995435` pushed to `main`; the "Deploy site to GitHub Pages" run succeeded, and https://mdussere.github.io/hakoni/ serves a page identical to `site/index.html` (checked with `curl` and `diff`).
- **Founding discussion trimmed**: at Michaël's request, the two earlier experiments (onboarding by link, sandbox as a work unit) are removed from [0000_03_hakoni-founding-discussion.md](../worklines/first_step/docs/0000_03_hakoni-founding-discussion.md): summary, detailed summary and verbatim sections; the remaining sections are renumbered.
- **Founding discussion in the first person plural**: "we" replaces Michaël's name in [0000_03_hakoni-founding-discussion.md](../worklines/first_step/docs/0000_03_hakoni-founding-discussion.md), in the summaries and as the speaker label of the verbatim.
- **Vision § 2 extended**: Michaël reworded the first experience; a new sub-section records the same onboarding pattern used between agents in TANSTAAFLLM, where an orchestrator agent prepares a new workline, its environment and the incoming agent's onboarding document (Hakoni's first agent joined that way). No existing skill covers writing such a document (searched the Hakoni and TANSTAAFLLM repositories and the global Claude Code and Codex skills).
- **Vision § 6 structured** (Michaël's dictation): a Hakoni is described as five parts (the container, the knowledge, the interface with the outside through providers, the outside tooling that prepares providers and renews their credentials, the memory); the earlier bullets become its principles. OpenShell terms stay to be checked in Step 2.
- **Vision § 4: projected scenario** (Michaël's dictation): a personal sparring-partner Hakoni handed to friends, who copy the repository privately and let their coding agent plant it (dependencies, providers, OpenShell container, SSH connection, clone inside the sandbox) before switching to a session inside the box, possibly through VS Code Remote-SSH. Open points listed: private copies of a public repository, remote sessions per client.
- **Vision § 4: essence, instance, persistence** (Michaël): the essence is the original repository and guide; an instance is an OpenShell container created from it on someone's computer (several are possible); the persistence is that person's own copy of the repository, which holds their logs and changes and feeds contributions back to the essence (for example by pull request). Input for the vocabulary decision in Step 1.4.
- **Fork visibility checked**: GitHub's documentation confirms that a fork of a public repository stays public and cannot be made private on its own; a private persistence must be a duplicate (bare clone and mirror push) or a repository created from a template, and neither keeps a documented pull-request route to the essence. Recorded in vision § 4 with the three documentation links.
- **Step 1 started** (workline `first_step`): Michaël considers the vision reviewed (1.1 ticked) and gives the demonstrator's scope: from the seed, reached from the site, set up Hakoni and make a contribution from inside an OpenShell box, on this workstation, with the local Claude Code in VS Code. Draft [0101_01_demonstrator-scope.md](../worklines/first_step/docs/0101_01_demonstrator-scope.md) written (1.2 to 1.4), waiting for approval.
  - Observed: Ubuntu 26.04.1 LTS, Claude Code 2.1.119, VS Code 1.138.0; `docker`, `podman` and `openshell` are not installed (`command -v`).
  - Proposed scope change, not applied yet: Codex and a second install move out of the first demonstrator; the plan's "Done means" (two clients, two installs) is unchanged until Michaël decides.
- **Step 1 done**: Michaël approved [0101_01_demonstrator-scope.md](../worklines/first_step/docs/0101_01_demonstrator-scope.md) and its three proposals: Codex and a second install are the next run (Step 5.2), not the first demonstrator; a `Hakoni-Instance: <box name>` commit trailer proves that the contribution comes from the box; essence, instance and persistence stay vision terms. The plan's "Done means" now puts the Claude Code run first, then Codex. Step 2 waits for its green light.
