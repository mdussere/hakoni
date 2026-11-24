<p align="center"><img src="site/assets/hakoni-logo.svg" alt="Hakoni logo: a miniature garden in a closable box" width="240"></p>

# Hakoni

*A garden in a box for AI agents.*

Introduction page: [mdussere.github.io/hakoni](https://mdussere.github.io/hakoni/).

Hakoni explores a simple idea: hand someone **one link**, let their general-purpose coding agent (Claude Code, Codex…) read it, and end up with a small, sandboxed work environment that already knows its job: its tools, its rules, its knowledge and its place in a shared project.

The name is a contraction of *hakoniwa* (箱庭), the Japanese miniature garden in a box: a small, living world with clear edges.

## The idea in four lines

- **The seed** is an onboarding document behind a Git link. It explains the mission, the prerequisites and how to join the project.
- **The box** is a sandboxed container: the delivery unit, with its tools, skills, conventions and access policies.
- **Git** carries the recipe, the shared knowledge, the contributions and the work log.
- **The agent** is interchangeable: it installs the environment, with the user's permission, then works inside it.

The long-term question is provocative on purpose: could small agentic environments like these replace part of the informal, spreadsheet-based tooling that businesses build for themselves?

## Status

Early exploration. Nothing is built yet: the [vision](worklines/first_step/docs/0000_01_vision.md) lists hypotheses and open questions, and the [first plan](worklines/first_step/plan_first_step.md) describes the path to a first seed. Hakoni is meant to be developed with Hakoni.

## Working in this repository

Agents start with [AGENTS.md](AGENTS.md). Work is organised in worklines (`worklines/<name>/plan_<name>.md`) and recorded in [docs/LOGBOOK.md](docs/LOGBOOK.md).

## License

Apache 2.0, see [LICENSE](LICENSE).
