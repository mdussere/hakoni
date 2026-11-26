# Entire — Agent-native Git and implications for Hakoni

> Technology watch · 26 November 2026  
> Research note, **not an architecture decision**. Public information and hypotheses are separated below. Entire has not been installed or tested inside Hakoni.

## Executive summary

[Entire](https://entire.com/) develops an agent-oriented Git development workflow: capturing sessions and code-change provenance, making project context recoverable, and exploring a Git forge designed for humans and coding agents. The company was founded by former GitHub CEO Thomas Dohmke in 2026. Its [open-source CLI](https://github.com/entireio/cli) is the most relevant starting point for practical investigation.

The overlap with Hakoni is **Git as a durable collaboration and context layer**, not simply a place to store source code. The difference is the delivery unit:

- **Entire:** infrastructure around code, agent work, checkpoints, memory, search, review and Git hosting.
- **Hakoni:** a **small, sandboxed agentic work environment** delivered from a single onboarding link, with tools, skills, instructions, permissions, persistence and collaboration conventions. The target includes non-developers.

Potential relationship: Entire-like capture and search could become an optional provenance/memory service **used by a Hakoni**, not a replacement for Hakoni's sandbox or distribution model.

## 1. Entire's offering — capability map

| Area | Reported capability | Relevance to Hakoni |
| --- | --- | --- |
| **Entire CLI / Checkpoints** | Record coding-agent sessions and connect their activity to Git changes. | Recover what happened and why; complement curated workline logs. |
| **Session provenance** | Link prompts, tool usage, generated changes and session summaries to commits/checkpoints. | Traceability during agent hand-offs and investigations. |
| **Continuous Memory** | Reuse historical development context across agent sessions. | Reduce onboarding repetition; **do not** confuse recorded history with accepted decisions. |
| **Agentic Search** | Query code and related development context, potentially using iterative investigation. | Retrieve *why* a change happened without replaying every transcript. |
| **Code graph** | Explore relationships and structure in a codebase. | Potentially useful for coding agents; not equivalent to knowledge of a business workflow. |
| **Git forge / review workflow** | Agent-oriented Git hosting and review concepts, including Trails. | A possible collaboration substrate, not a Hakoni requirement. |
| **Agent identity** | Emerging ways to distinguish autonomous agents as principals. | Worth evaluating against scoped credentials and sandbox access policies. |

These are product descriptions, not claims that all features are open source, generally available, or available without Entire's service. Confirm versions, licenses and interfaces before any integration.

### Checkpoints versus workline logs

An execution trace, a commit, a project decision and a reusable memory item serve **different purposes**:

1. **Raw agent session:** rich evidence, high volume and potentially sensitive.
2. **Checkpoint/commit attribution:** the link between a change and its provenance.
3. **Distilled memory:** useful context to recover later, with confidence and source links.
4. **Accepted project knowledge:** authoritative instructions, policies, skills and decisions.

Hakoni already has curated documentation and a logbook. Automatic capture could complement it, but should never silently overwrite authoritative instructions or publish private session content.

## 2. Comparison against Hakoni

Hakoni's [README](../README.md) identifies the **seed** (one Git onboarding link), **box** (sandbox), **Git** (recipe, knowledge and collaboration) and interchangeable **agent**. The [vision](../worklines/first_step/docs/0000_01_vision.md) distinguishes a reusable *essence*, an installed *instance* and individual *persistence*.

| Dimension | Entire | Hakoni |
| --- | --- | --- |
| Main question | How should Git workflows and repositories evolve for coding agents? | How can someone receive, run and grow a useful agentic environment from one link? |
| Git | Code + agent session provenance / search / review. | Recipe + work plan + knowledge + contribution and continuity substrate. |
| Environment | Tooling around agent-assisted development. | Sandboxed container is foundational. |
| User | Primarily software development teams. | Also non-technical people using specialised agentic packages. |
| Distribution | Git platform and CLI. | Bootstrap instructions, reproducible package and explicit installation approval. |
| Memory | Captured session context and searchable history. | Versioned shared knowledge; raw versus distilled memory remains a design question. |
| Security boundary | Access controls on tooling, agents and repositories. | Explicit host/bootstrap versus sandbox trust boundary and provider permissions. |

**Important:** Entire does **not** replace the Hakoni box. Conversely, Hakoni should not reinvent a developer forge if one can serve as an optional backend.

## 3. A possible integration boundary

```mermaid
flowchart TD
    U["User + general-purpose coding agent"] --> S["Hakoni seed (Git link)"]
    S --> H["Approved host bootstrap"]
    H --> B["Sandboxed Hakoni box"]
    B <--> G["Git: essence / worklines / knowledge"]
    B --> A["Agent tasks, changes and tools"]
    A -. "optional capture" .-> C["Entire CLI / Checkpoints"]
    C -. "linked provenance" .-> G
    C -. "optional indexing" .-> M["Continuous Memory / Agentic Search"]
    M -. "source-linked retrieval" .-> B
```

Dashed connections are **proposals, not implemented interfaces**.

Potential benefit: a fresh agent joining a Hakoni workline could ask what was attempted, what changed, why a decision was made and where supporting evidence lives, instead of reloading an ever-growing transcript. The agent would still use versioned Hakoni instructions as the authority.

## 4. Critical constraints for Hakoni

- **Private by default.** Agent transcripts may contain credentials, local paths, tool results and sensitive content. Hakoni's public *essence* must never receive such information accidentally.
- **Separate lifetimes and ownership.** Shared reference knowledge, local/personal memory, raw session telemetry and short-lived tokens require distinct storage, retention and access policies.
- **Git portability.** A seed should still work using standard Git and an interchangeable coding agent if Entire is absent.
- **Scoped permissions.** A sandbox should not obtain broad repository tokens or unrestricted outbound traffic merely to enable checkpoints.
- **No hidden dependency.** An optional capture/indexing layer must not make a one-link installation less reproducible or harder to explain.
- **Truth and authority.** An agent's retrospective account is evidence, not necessarily a validated reason for a code change.
- **Outside coding.** Hakoni should investigate whether session capture generalises to business tasks (reporting, investigation, domain procedures), rather than assuming code-specific signals will transfer.

## 5. Suggested research and experiments (not authorised implementation work)

| Priority | Experiment | What to measure |
| --- | --- | --- |
| P0 | Inspect the open-source Entire CLI and its checkpoint/commit data format in a throwaway fictional repository. | Portability, readable metadata, dependencies, retention. |
| P0 | Threat-model session capture in a sandbox with synthetic secrets. | What can leave the box, what reaches a Git remote, redaction and opt-out. |
| P1 | Compare a Claude Code → Codex hand-off using Hakoni workline docs alone versus docs plus indexed checkpoints. | Correct resumption, context lost, setup steps, time and tokens. |
| P1 | Try branch changes, rebase, squash and work with no commits. | Provenance continuity and failure modes. |
| P1 | Explore **private** provenance storage alongside a public package repository. | Isolation between public essence and personal persistence. |
| P2 | Answer “why was this changed?” with curated logbook, Git history and agentic search. | Evidence quality and unsupported interpretations. |
| P2 | Assess whether Entire components could be optional tools in a Hakoni image. | Additional complexity, permissions, resource usage. |

**Suggested first move:** inspect formats and local behavior before adopting another forge, enabling automatic uploads or changing Hakoni's architecture. Use synthetic data and an isolated test repository.

## 6. Questions worth keeping open

1. Can a checkpoint be attached to a workline independently of a Git commit?
2. Can a project restart without Entire's hosted service and still interpret the record?
3. Can public project instructions and private session telemetry be safely separated?
4. What is the minimum concise, verifiable hand-off format between different coding agents?
5. Who decides which retrieved memories are useful, current and trustworthy?
6. Do the capture and search tools work for non-code business-agent workflows?
7. Does Entire improve a one-link Hakoni setup enough to justify another dependency?
8. How do provider identity, local sandbox policy and Git permissions interact?

## 7. Access points and references

**Official / primary entry points:**

- Entire product and company: https://entire.com/
- Entire source organisation: https://github.com/entireio
- Open-source Entire CLI: https://github.com/entireio/cli
- Entire documentation (follow current links from the product homepage and CLI README): https://docs.entire.com/
- Entire engineering posts: https://entire.com/blog
- Entire company announcements: https://entire.com/news

**Hakoni sources used for the comparison:**

- [README — project orientation](../README.md)
- [Vision and open questions](../worklines/first_step/docs/0000_01_vision.md)
- [Agent rules](../AGENTS.md)
- [Logbook](LOGBOOK.md)

External source pages and product claims can change; follow the official release and CLI documentation when testing. Specific marketing performance claims should not be treated as independently validated Hakoni outcomes.

## Conclusion

**Entire signals an important industrial shift:** Git-based development is accumulating agent provenance, searchable intent and persistent context. Hakoni has a related but distinct product thesis: **a small, distributable, sandboxed agentic environment that starts from a single link**.

The most promising intersection is an optional **provenance and searchable memory layer** that helps agents resume and collaborate while preserving Hakoni's separate, explicit trust boundary. Evaluate such a layer empirically; do not conflate it with the box itself.


## 8. Individual developer access, commercial model, and coding-agent integrations (2026-11-26 follow-up)

This section records the follow-up discussion about using Entire as an **independent developer**, without changing the Hakoni project architecture. Commercial terms and CLI commands are time-sensitive: verify them against the linked official material before a real installation.

### Early-access pricing and account options

- Entire presents the **CLI** as an open-source, locally usable entry point; installing and trying it does not require moving a repository from GitHub to Entire's hosted forge.
- At the time of this discussion, Entire advertised an **early-access free offering** for its platform features such as Git hosting, checkpoint storage, Continuous Memory and Agentic Search. **Free during early access is not a permanent pricing guarantee.** Some advanced/hosted features may require signup or service availability.
- Distinguish **Entire's platform and session infrastructure** from **LLM inference executed/billed through Entire**. Hosted agent inference can be metered by tokens, independently of the user's existing Claude Code or Codex subscription.
- Promotional credits, payment-card verification, quotas, free-tier limits, data-retention restrictions and future commercial tiers should be checked on the official pricing and terms pages at trial time. **Do not assume a permanent zero-cost brain for an unrestricted corpus.**
- A solo developer can evaluate the CLI using an existing Git repository, but should not conflate local CLI support with access to every hosted memory/search feature.

**Pricing reference:** https://entire.com/pricing  
**Product:** https://entire.com/  
**CLI source / usage and releases:** https://github.com/entireio/cli

### Claude Code and OpenAI Codex CLI integration

Both **Claude Code** and **Codex CLI** are named as supported integration targets for the Entire CLI. The general mechanism is agent-specific session **hooks/adapters** coupled with Git checkpoint provenance. This is different from transferring a live conversation between vendors: the actual interoperable content and resume behavior must be tested.

Capabilities to inspect in both clients:

| Concern | What to verify |
| --- | --- |
| Onboarding | Whether `entire enable` detects both agents and writes per-agent hooks without altering unrelated repository policies. |
| Session capture | Which prompts, tool events, files and summaries are actually captured; what is omitted. |
| Git attribution | Whether checkpoints can be traced to commits and recovered after checkout, rebase and merge. |
| Cross-agent continuity | Whether Codex can retrieve a Claude Code session's **relevant evidence**, and vice versa, rather than merely seeing a summary or unrelated index. |
| Search / brain | Which search commands require hosted indexing, account access or extra components. |
| Data safety | Whether capture remains local, which references/remotes are pushed, and how to disable or redirect session synchronization. |
| Client compatibility | Current supported versions of Codex and Claude Code, including desktop/remote-SSH environments. |

### Suggested commands — inspect official help before executing

The previously discussed Mac starting point is:

```bash
brew install --cask entireio/tap/entire
cd /path/to/disposable-private-test-repository
entire enable
entire status
```

Checkpoint-related CLI actions discussed as examples include `entire checkpoint list`, `entire checkpoint explain <id>`, `entire session resume <branch>`, and `entire search`. **Treat these as commands to validate against `entire --help` and the installed release, not a tested runbook.** CLI syntax evolves.

A command discussed for preventing Git session synchronization during local experiments was:

```bash
entire configure --local --skip-push-sessions
```

**Do not rely on that flag as a security guarantee without checking the current CLI help and inspecting the resulting Git references, config and network behavior.** Keep the first experiments on fictional data and a disposable private repository; do not start with Hakoni's public repository.

### Practical recommendation for the Hakoni development agent

1. **Read only:** compare Entire's documented data structures, checkpoint storage and current CLI license with Hakoni's workline and logbook conventions.
2. **Isolated local trial:** use a disposable private Git repository with synthetic prompts; install the CLI and enable one agent at a time.
3. **Cross-client hand-off:** run a short, non-sensitive task in Claude Code and attempt to resume or retrieve its relevant history from Codex; repeat in reverse.
4. **Inspect persistence and cost:** list Git refs, push settings, files written and outbound requests; determine what requires an Entire account, hosted Brain/Search, a paid model or credits.
5. **Evaluate fit, not adoption by default:** compare useful recovered context, latency, tokens and setup complexity with plain Hakoni workline docs and Git history.
6. **Feed findings into Hakoni's planning process:** preserve the existing sandbox boundary and respect the repository's workline green-light protocol before implementing anything.

**Guiding hypothesis:** Entire's project-wide “brain” aligns closely with the desire for a reusable, cross-session project memory. The specific question for Hakoni is whether that brain can be a **replaceable optional service**, usable across both Claude Code and Codex without compromising the one-link seed or leaking private execution history.

**Further references:** https://github.com/entireio/cli · https://entire.com/pricing · https://entire.com/blog · https://docs.entire.com/
