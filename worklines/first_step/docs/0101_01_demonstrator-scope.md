# First demonstrator: scope, tests and vocabulary

**Question this document answers:** what must the first Hakoni demonstrator prove, on which machine and with which agent client, how do we check it, and which words do we use?

Written on 2026-11-24 for Step 1 of the [plan](../plan_first_step.md) (sub-steps 1.2 to 1.4). Status: **approved by Michaël on 2026-11-24**, including the three proposals below, which are now decisions.

Sources are marked: **(decided)** for Michaël's decisions, **(observed)** for facts checked on the machine with the command given, **(proposed)** for proposals to approve.

## 1. What the demonstrator must prove

**(decided)** Starting from the seed, reached from the Hakoni site, a coding agent sets up the project: it installs what is missing, creates the box (an OpenShell container), and makes **a contribution to the Hakoni repository from inside the box**.

**Acceptance (decided):** a contribution in the Hakoni repository that provably comes from the box.

Hakoni builds Hakoni: the project planted by the seed is Hakoni itself.

## 2. For whom, where, with what

| Item | Value | Source |
|---|---|---|
| Recipient | Michaël, playing the recipient of his own seed | decided |
| Machine | Michaël's workstation, the one this workline runs on | decided |
| OS | Ubuntu 26.04.1 LTS, kernel 7.0.0-43-generic | observed: `/etc/os-release`, `uname -r` |
| Agent client | Claude Code 2.1.119, local, in VS Code 1.138.0 | observed: `claude --version`, `code --version` |
| Sandbox runtime | OpenShell | decided (facts checked in Step 2) |
| Container engine | none installed yet (`docker`, `podman` and `openshell` are absent) | observed: `command -v` |
| Repository | `mdussere/hakoni` (public) | decided |

**Not in this demonstrator (decided):** Codex, a second machine and a second recipient. The vision's success criteria ask for two clients on two installs; this first demonstrator proves the path once, with one client. The second client stays in Step 5 of the plan (5.2) as the next run.

## 3. Checklist of observable tests

Each test names its expected evidence. Tests T1 to T7 make the acceptance; T8 and T9 come from the vision's criteria and are proposed for the same run; the security checks stay in Step 6.

| # | Test | Expected evidence |
|---|---|---|
| T1 | The seed is reachable from the site: the site links to the seed file in the repository. | The site's URL and the link it shows. |
| T2 | Given only the seed link, the agent summarises what it will do before acting, and asks before every installation or privileged command. | The session transcript or notes: the summary, then each request and its answer. |
| T3 | The missing prerequisites (OpenShell, container engine) are installed with Michaël's approval. | The commands run and their output, in the run report. |
| T4 | The box is built and started from the recipe in the repository. | The OpenShell command and its status output. |
| T5 | The agent connects to the box (SSH as the first candidate) and runs a command inside it. | Output of a command run in the box, showing a host name that differs from the workstation's. |
| T6 | Inside the box, the agent clones Hakoni and prepares it: `uv sync`, then `uv run pytest` passes. | Command output inside the box. |
| T7 | The agent makes a contribution from inside the box: a commit pushed with Michaël's own identity, provided at run time. | The commit on GitHub, and proof of origin (see below). |
| T8 | A new session in the box resumes: `git fetch`, a review of what changed, then work picks up from the plan. | Transcript of the restarted session. |
| T9 | No secret ends up in the image, the repository or the logs. | A search of the image, the pushed commits and the run report for tokens and keys; nothing found. |

**Proof of origin for T7 (decided):** the commit carries a trailer naming the box, for example `Hakoni-Instance: <box name>`, and the run report shows the same commit created in a session whose host name is the box's.

## 4. Vocabulary

**(decided)** Keep the generic agentic terms, and add as few of our own as possible.

| Term | Meaning | Source |
|---|---|---|
| **Hakoni** | A germinating project: a project packaged so that it can be planted elsewhere from its seed. Also the name of this repository. | decided |
| **seed** | The starting point: the onboarding document behind a Git link. | decided |
| **box** | The container, which is also the sandbox: "box" and "sandbox" mean the same thing. | decided |
| **agent**, **harness**, **skill**, **provider** | Used in their usual agentic and OpenShell meanings. | decided |

**Not adopted for now (decided):** the vision's projected scenario also uses **essence** (the original repository), **instance** (a box created from it on one computer) and **persistence** (a person's own copy of the repository). Keep them as vision terms for now, and adopt them in the repository only if the demonstrator needs them. The verbs **plant** and **grow** are not adopted: the generic "set up" and "work" are enough.
