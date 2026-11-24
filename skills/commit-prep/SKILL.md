---
name: commit-prep
description: Prepare and make small, reviewable Hakoni Git commits with explicit file lists. Use when the user asks to prepare, review, make or push commits.
---

# Commit Prep

Commits are explicit, reviewable changesets.

## Green-light rule

Commit only when the user asks. A request to commit includes pushing to `origin main` unless the user says otherwise. After pushing, give the user GitHub links: `https://github.com/mdussere/hakoni/commit/<sha>` for commits and `https://github.com/mdussere/hakoni/blob/main/<path>` for files.

## Workflow

1. Inspect the worktree with `git status --porcelain=v1 --untracked-files=all`. Preserve changes that are not part of the request.
2. Group files by one coherent intent: plans and docs, skills, tooling and code go in separate commits unless the user asks otherwise.
3. Stage an explicit file list, one `git add -- <file>` per file. Never `git add .` or `git add -A`.
4. Before committing, check `git diff --cached --check` and review `git diff --cached --name-status`.
5. `git fetch`, then rebase or merge if `origin/main` moved; never force-push.

## Public repository check

This repository is public. Before every commit, read the staged diff for anything confidential: names of employers, colleagues, clients or internal projects, internal tools or configurations, traces, data, secrets, tokens, `.env` files. If something is found, unstage it and tell the user.

Also never commit virtual environments, caches, build outputs, downloaded model weights or private recordings.

## Message style

- English, 5–10 lines: a short first line, a blank line, then 3–8 lines that explain the intent.
- One light garden emoji is welcome (🌱 🪴 🌿 🪨 🏡), as long as the message still describes the change.
- If the commit materially changes a workline plan, include the `docs/LOGBOOK.md` entry.
- End with the attribution line the session's instructions require, if any.

Examples of first lines:

- `🌱 plant the first seed`
- `🪴 pot the sandbox recipe`
- `🪨 set the garden's boundaries`
