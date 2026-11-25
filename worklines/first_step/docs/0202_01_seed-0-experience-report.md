# Seed 0: experience report

**Question this document answers:** what happened when the local Claude Code ran [seed 0](0201_01_seed-0.md) on Michaël's workstation, which OpenShell facts held against its documentation, and what the seed v0 should change?

Written on 2026-11-25 for Step 2.3 of the [plan](../plan_first_step.md), from the host agent's end-of-run summary. The run took about 30 minutes, early on 2026-11-25.

---

## Outcome

The run met the acceptance criteria of Step 2. One part was not observed directly: how Claude Code logs in inside the box (see "Claude Code inside the box").

- `ssh openshell-hakoni-seed0.default` from a host terminal opens a shell in the box. It answers `sandbox-hakoni-seed0` / `sandbox` / `/sandbox`, and its host name differs from the workstation's.
- Hakoni is cloned in `/sandbox/hakoni` (at `d7a9ad2`), with `uv sync` done. `claude --version` gives `2.1.325 (Claude Code)`.
- Michaël then saved the box as an SSH connection (`hakoni-seed0`) in the Claude desktop app. A session opened there in remote control and explored the box without trouble.
- **The box is not fully open to the Internet.** OpenShell 0.1.10 has no "allow all" policy. The trial policy allows HTTPS to an explicit list of hosts (see "Network policy").

## Run context

- **Host agent:** Claude Code in the Claude desktop app (Code tab), not in VS Code as the seed assumed. The session had no project folder.
- **Working folder:** Michaël asked for everything to stay in `/tmp/hakoni-seed0`, apart from the existing workspace. So the recipe and the policy were written in `/tmp/hakoni-seed0/seed0/` instead of `worklines/first_step/scripts/seed0/`.
- **`sudo`:** Michaël ran every `sudo` command himself, in the app's terminal. The agent did not type any password.

## Host facts (step 1)

| Item | Value |
|---|---|
| OS | Ubuntu 26.04.1 LTS, kernel 7.0.0-43-generic, x86_64 |
| glibc | 2.43 |
| ssh | OpenSSH 10.2p1 |
| docker, podman, openshell | not installed |
| cgroups | v2 |
| KVM | `/dev/kvm` usable by the user (ACL) |
| subuid / subgid | ranges present for the user (rootless containers possible) |
| Resources | 4 CPUs, 7.2 GiB RAM (about 2.2 GiB free), `/tmp` is a 3.4 GiB tmpfs |

The user is not in the `docker` group. The user is also in `lxd`, which is also root-equivalent on the host; this is outside the trial's scope.

## Timeline: commands, results, approvals

| Step | Action | Result | Approval |
|---|---|---|---|
| 0 | Summary of the plan, before any action | — | Michaël's go, with "work in `/tmp`" |
| 1 | Read-only checks (`/etc/os-release`, `ldd`, `command -v`, `podman` candidate, `/etc/subuid`) | See "Host facts" | Not needed (read-only) |
| 2 | Proposal: **rootless Podman 5.7** from Ubuntu universe, rather than Docker (the `docker` group is root-equivalent) | — | — |
| 2 | `sudo apt install podman` | Podman 5.7.0 and 18 dependencies (buildah, crun, conmon, netavark, aardvark-dns, passt…) | Michaël ran it |
| 2 | `systemctl --user enable --now podman.socket` | `rootless=true cgroup=v2`, socket `/run/user/1000/podman/podman.sock` | Run by the agent, as part of the approved Podman option |
| 3 | Download `install.sh` at tag `v0.1.10` (identical to `main`, sha256 `2cd0e9ac…`), read it in full; download the `.deb`, check its checksum, unpack it and read its maintainer scripts and systemd unit | See "OpenShell install" | Not needed (no execution) |
| 3 | `OPENSHELL_VERSION=v0.1.10 sh install-v0.1.10.sh` | Package installed, user service started, gateway registered, `openshell status`: *Connected* | Michaël ran it. **Michaël did not answer the telemetry question, so telemetry stayed on.** |
| 4 | Docs read (custom images, SSH, policy schema, telemetry, lifecycle); Claude Code's `install.sh` downloaded and read (sha256 `2c5506d1…`) | See "Facts observed" | — |
| 4 | `Dockerfile` written and shown | — | — |
| 5 | `policy.yaml` written and shown; `openshell policy lint` | "Policy is valid (offline)" | — |
| 4 | `podman build -t localhost/hakoni-seed0:0 /tmp/hakoni-seed0/seed0` | Image of 608 MB. Check run in a container: `sandbox` user, git 2.43.0, Python 3.12.3, uv 0.13.10, Claude Code 2.1.325 | Michaël: "oui, lance le build" |
| 6 | `openshell sandbox create --name hakoni-seed0 --from localhost/hakoni-seed0:0 --policy …/policy.yaml --no-auto-providers --detach --no-tty` | `Ready`: "Supervisor session connected", "ConfigurationAccepted". Two Podman containers: workload and supervisor | **Not asked again**: the agent treated it as a step of the accepted plan, not a host change |
| 7 | Check `~/.ssh/config` for global options (`Host *`, `ForwardAgent`, `Include`) | None, apart from two global `ServerAlive*` lines | — |
| 7 | Back up `~/.ssh/config`, write `~/.ssh/openshell-hakoni-seed0.conf`, add `Include` on line 1 | `ssh … 'hostname; whoami; pwd'` gives `sandbox-hakoni-seed0` / `sandbox` / `/sandbox` | Michaël: "fais la version avec Include" |
| 8 | `git clone https://github.com/mdussere/hakoni.git` in `/sandbox` | `d7a9ad2`; no Git identity or credentials in the box | — |
| 8 | `uv sync`, `uv run pytest` | 7 packages installed (pytest, playwright…). **0 tests collected** (exit code 5) | — |
| 8 | Network probe from the box | `api.anthropic.com`: HTTP 404 (reached). `example.com`: "Couldn't connect to server" (denied). Writing to `/home/sandbox` works | — |
| 9 | Hand-over; then the box saved as an SSH connection in the Claude desktop app | A remote-control session works in the box (reported by Michaël) | Michaël did it himself |

## OpenShell install (step 3)

What the installer does on Ubuntu amd64:

- It downloads `openshell_0.1.10-1_amd64.deb` (85 MB) from the GitHub release and checks its sha256 against the release checksum file.
- **`sudo` is used only for `apt-get install -y` of that `.deb`.** The package installs `/usr/bin/openshell`, `openshell-gateway`, `openshell-prover`, `/usr/libexec/openshell/openshell-driver-vm`, and the user unit `/usr/lib/systemd/user/openshell-gateway.service`. Its `postinst` and `postrm` scripts only run `systemctl daemon-reload` (system and `--global`).
- As the user, without `sudo`, it enables and starts `openshell-gateway`. The gateway listens on `127.0.0.1:17670` with mTLS and generates its PKI in `~/.local/state/openshell/tls`.
- It registers the gateway in `~/.config/openshell/` (`openshell gateway add https://127.0.0.1:17670 --local --name openshell`) and waits for `openshell status`.
- The service reads an optional `~/.config/openshell/gateway.env`. This is where `OPENSHELL_TELEMETRY_ENABLED=false` can go **before** the first start.
- `OPENSHELL_VERSION=v0.1.10` pins the release. Snap installs ignore the pin and need Docker.

## Facts observed against the documentation

### Version and container engine

- **Version:** OpenShell **0.1.10**, released 2026-11-24, the latest tag on the day. The repository is `ai-openshell/openshell`.
- **Container engine:** the gateway picks the Podman driver by itself when it finds the user's Podman socket. Its log reads `Using compute driver driver=podman` and `rootless=true rootless_network_cmd=pasta`, and it creates a Podman network named `openshell`. No configuration was needed, and Docker never came up.

### Custom image

- `--from` takes an image reference. With Podman the reference carries the `localhost/` prefix.
- The image needs a non-root `USER`: the Docker and Podman drivers reject root.
- `/sandbox`, or an existing `WORKDIR`, becomes the workspace, so the clone lives in `/sandbox/hakoni` and not in `~/hakoni`.
- The supervisor replaces `CMD` and `ENTRYPOINT`. `--detach` works without a trailing command.
- **No openssh in the image:** SSH is served by the supervisor.

### SSH

- `openshell sandbox ssh-config <name>` prints a `Host openshell-<name>.default` entry with `User sandbox` and a `ProxyCommand /usr/bin/openshell ssh-proxy --gateway-name openshell --name <name> --workspace default`.
- No port is exposed: the traffic goes through the local gateway, over mTLS.
- **Host keys are not checked** (`StrictHostKeyChecking no`, `UserKnownHostsFile /dev/null`).
- `ssh-config` has **no `--editor` flag**. `--editor vscode|cursor` belongs to `sandbox create` and `sandbox connect`, where it installs an OpenShell-managed include file.

### Network policy

- **No "allow all".** `host: "*"` is invalid. A wildcard host needs at least three DNS labels (`*.example.com`), so the box cannot be made fully open.
- Every `network_policies` entry needs a `binaries` list: an empty list matches nothing.
- The proxy terminates TLS with a per-sandbox CA and injects it through `SSL_CERT_FILE`, `NODE_EXTRA_CA_CERTS`, `GIT_SSL_CAINFO` and similar variables. `git`, `curl`, `uv` and Claude Code worked through it with no extra setup.
- Writing a `filesystem_policy` replaces the default one, so the user's home must be listed explicitly when it has to be writable.

### Telemetry

- Telemetry is **on by default**. The gateway binary holds the endpoint `events.telemetry.data.nvidia.com`, which the telemetry page does not name. It is turned off with `OPENSHELL_TELEMETRY_ENABLED=false` on the gateway.
- The docs describe the data as anonymous categories and counts: no names, paths, prompts or credentials.

### Lifecycle

The commands below exist in 0.1.10 but were not run in the trial.

- `openshell sandbox stop` keeps the policy, the settings, the SSH host key and the workspace.
- `start` brings back the same box without process memory.
- `delete` removes everything, workspace and host key included.

The docs describe workspace persistence for Docker only, so it remains to check with Podman.

## Network policy (step 5)

The policy used for the trial:

- **Hosts:** HTTPS on port 443 only.
  - GitHub: `github.com`, `api.github.com`, `codeload.github.com`, `*.githubusercontent.com`.
  - Anthropic and Claude: `api.anthropic.com`, `console.anthropic.com`, `*.anthropic.com`, `claude.ai`, `*.claude.ai`, `claude.com`, `*.claude.com`.
  - Python packages: `pypi.org`, `files.pythonhosted.org`.
- **Binaries:** `"/**"`, so any binary. The docs warn against this; it was a trial choice.
- **Filesystem:**
  - read-write: the workspace, `/tmp`, `/dev/null` and `/home/sandbox` (Claude Code config, uv cache);
  - read-only: `/usr`, `/lib`, `/lib64`, `/bin`, `/sbin`, `/etc`, `/opt`, `/proc`, `/dev/urandom`, `/var/log`.

Everything else is denied, including `apt` inside the box and Playwright's browser downloads. The wildcards also open a small DNS exfiltration channel, which the docs point out.

## Image recipe (step 4)

- **Base:** `docker.io/library/ubuntu:24.04`, with `ca-certificates`, `curl`, `git`, `python3`, `less` and `procps`.
- **uv 0.13.10:** copied from `ghcr.io/astral-sh/uv:0.13.10` (`COPY --from`), so no install script is run.
- **User:** `sandbox`, UID/GID 1000. It replaces Ubuntu's default `ubuntu` user and owns `/sandbox`. It is the image's `USER`, and `WORKDIR` is `/sandbox`.
- **Claude Code 2.1.325**, the "stable" channel on the day (latest was 2.1.331).
  - It is installed by Anthropic's `install.sh`, downloaded and read on the host first, then `COPY`ed into the build. The script checks the binary's sha256 from a manifest and installs it in `~/.local/bin`.
  - The script itself is not pinned: it always comes from `claude.ai/install.sh`.
- **No secrets, keys, providers or `--env`.**

## Claude Code inside the box

- **Running a session:** the Claude desktop app's SSH connection, with host `openshell-hakoni-seed0.default` and folder `/sandbox/hakoni`, runs a session in the box in remote control. It needed no extra network host beyond the policy.
- **Not observed by the host agent:** whether that session logged in through an interactive login inside the box, or with credentials handled by the desktop app. The session in the box answered the first question afterwards (see "Seen from inside the box"). `claude` started from a plain terminal in the box, with its interactive login, was still not tested.
- **No API key was set up.**

## Seen from inside the box

Added on 2026-11-25 by the Claude Code session running in the box (desktop app, SSH connection `hakoni-seed0`), after Michaël asked it to look around. Only read-only commands were run; values that identify a person or an account are left out on purpose.

### Isolation

- The session runs as `sandbox` (UID/GID 1000), in a rootless Podman container whose PID 1 is `openshell-sandbox --bootstrap …`.
- No `sudo`. `CapEff` and `CapBnd` are `0`, `NoNewPrivs` is `1`, and seccomp is in filter mode.
- `/sandbox`, `/home/sandbox` and `/tmp` can be written (`touch` then `rm`), as the policy says. `/etc` and `/usr/local` cannot. `/run/secrets`, `/.openshell` and `/.openshell/channel` cannot be listed.
- There is no Docker or Podman socket, and nothing from the host user's home is mounted. Only the workspace volume (`/sandbox`) and the supervisor's own files are mounted.
- The box sees the host's 4 CPUs and 7.2 GiB of RAM, and 157 GiB free on the workspace volume. No resource limit was set.

### Network, probed from the box

`curl` to each URL, 5-second timeout. The results match the policy above.

| Reached (HTTP status) | Refused at connect |
|---|---|
| `github.com` 200, `api.github.com` 200, `raw.githubusercontent.com` 301 | `example.com`, `www.google.com`, `chatgpt.com` |
| `pypi.org` 200, `files.pythonhosted.org` 404 | `registry.npmjs.org`, `storage.googleapis.com` |
| `api.anthropic.com` 404, `claude.ai` 403 | `http://github.com` (port 80) |

`git ls-remote origin` and `git pull --ff-only` from `/sandbox/hakoni` both work over HTTPS.

### How the desktop-app session runs and authenticates

- **The desktop app brings its own Claude Code.** Over SSH it installs a server and its own CLI under `/sandbox/.claude/remote/` (about 266 MB): a `server` process in `--serve` and `--bridge` modes, and `ccd-cli/2.1.299-…`, which runs the session. The image's Claude Code 2.1.325 (`/home/sandbox/.local/bin/claude`) is not used by this session.
- **No login inside the box.** There is no `.credentials.json` and no API key in the environment. The session's environment names a host-side session and says the host refreshes the authentication (`CLAUDE_CODE_SDK_HAS_HOST_AUTH_REFRESH`, `CLAUDE_CODE_ENTRYPOINT=claude-desktop`, `CLAUDE_SSH_DAEMON_CHILD=1`), and `ANTHROPIC_BASE_URL` points to `https://api.anthropic.com`. So the credentials are handled by the desktop app on the host and passed to the session; they are not stored in the workspace. How they travel was not inspected.
- **`HOME` is `/sandbox`, not `/home/sandbox`**, for the server and the session (`/etc/passwd` says `/home/sandbox`; which component sets it was not found). So the session's Claude Code state lives in the workspace: `/sandbox/.claude/`, `/sandbox/.claude.json` and `/sandbox/.cache/`. `/sandbox/.claude.json` holds the account profile (name, email, organisation). It is outside the Git clone and is not committed, but anything that copies or shares the workspace volume carries it.

### Instructions and skills (input for 2.4)

For Claude Code, through the desktop app, with `/sandbox/hakoni` as the folder:

- `CLAUDE.md` is loaded and its `@AGENTS.md` import is followed: the session receives `AGENTS.md` in full as project instructions.
- The four repository skills (`workline`, `commit-prep`, `shared-chat-discussion`, `clean-verbatim`) are discovered through the `.claude/skills/` links, next to the desktop app's own skills.
- Codex was not tried in the box; whether it finds `.agents/skills/` there stays open.

### Tooling and Git

- git 2.43.0, uv 0.13.10, Python 3.12.3, curl 8.5.0. No `node`, `npm`, `gh`, `docker` or `podman`.
- At first there was no Git identity, no `~/.ssh` and no credential helper: the box could pull but not push.
- Then, at Michaël's request, the session gave the box a push path:
  - `gh` 2.105.0 was installed without `sudo`: the official `linux_amd64` tarball from the GitHub release, checked against the release's sha256 file, then copied to `~/.local/bin`;
  - Michaël logged in interactively with `gh auth login --web` (device code entered in his own browser), and `gh auth setup-git` made `gh` Git's credential helper for github.com;
  - the Git identity was set in the box's `~/.gitconfig`.
- **This puts a secret in the box:** an OAuth token, stored in plain text in `/sandbox/.config/gh/hosts.yml` (mode 600), so in the workspace volume. Its scopes are `repo`, `read:org` and `gist`, and the account is admin of the repository, so the box can write to every repository of that account. Revoke it with `gh auth logout` in the box or from GitHub's authorised applications. For v0, prefer a fine-grained token limited to the one repository and to contents and pull requests.
- `uv run pytest` still collects 0 tests: the repository has no `test/` folder yet.

## Where seed 0 was wrong, unclear or missing something

| Seed 0 says | What happened |
|---|---|
| The agent runs in VS Code | It ran in the Claude desktop app; nothing in the seed depended on VS Code |
| Write the recipe in `worklines/first_step/scripts/seed0/` | Written in `/tmp/hakoni-seed0/seed0/` at Michaël's request |
| Install `openssh` in the image if needed | Not needed: the supervisor serves SSH |
| `~/hakoni` | The workspace is `/sandbox`, so the clone is `/sandbox/hakoni` |
| Use a non-root user "if OpenShell expects one" | Required: root images are rejected |
| `docker build -t hakoni-seed0:0` | With Podman the image is referenced as `localhost/hakoni-seed0:0` |
| `ssh-config --editor vscode` writes an include file | That flag does not exist on `ssh-config`. The include was written by hand, with `ForwardAgent no` and `IdentitiesOnly yes` added |
| "Fully open" box: look for a host wildcard | Impossible: `*` is invalid, wildcards need three labels, and `binaries` is mandatory. The seed did not mention the `binaries` requirement or the filesystem policy |
| Claude Code installer "host" to allow | The installer and its updates use `downloads.claude.ai`; the login also uses `claude.com` hosts. The seed's list missed both |
| Install Claude Code in the image | Its official install path is `curl … \| bash`. The seed's "no blind remote code" rule held only because the script was read on the host first. The seed v0 should name that path |
| Telemetry: tell the user and let them decide | The question stayed unanswered when Michaël started the install, so the default (on) applied. The seed v0 should ask before step 3 and write `gateway.env` first |
| `uv run pytest` | 0 tests in the repository; exit code 5 is expected for now |
| The seed does not mention host resources | About 2.2 GiB of free RAM was enough, but tight; the seed v0 could check it |

The seed's own rules held:

- a summary came first;
- every install and `sudo` command was shown and run by Michaël;
- both install scripts were downloaded and read before running;
- no secret went into the box;
- nothing was committed by the host agent.

One step was taken without a fresh question: the box creation (6).

## What remains on the host, and how to remove it

| Item | Removal |
|---|---|
| Box `hakoni-seed0`: two Podman containers and its workspace volume | `openshell sandbox delete hakoni-seed0` |
| Images `localhost/hakoni-seed0:0`, `ghcr.io/astral-sh/uv:0.13.10`, `docker.io/library/ubuntu:24.04` | `podman rmi …`, or `podman system reset` to wipe all Podman state |
| Package `openshell`; user service `openshell-gateway` (enabled) | `systemctl --user disable --now openshell-gateway`, `sudo apt purge openshell` |
| OpenShell state: `~/.config/openshell`, `~/.local/state/openshell`, maybe `~/.local/share/openshell` | `rm -rf` those folders |
| Package `podman` and its 18 dependencies; user socket `podman.socket` (enabled) | `systemctl --user disable --now podman.socket`, `sudo apt purge podman && sudo apt autoremove` |
| `~/.ssh/openshell-hakoni-seed0.conf`, the `Include` line at the top of `~/.ssh/config`, the backup `~/.ssh/config.bak-seed0` | Remove the line (or restore the backup), then remove the file |
| The `hakoni-seed0` SSH connection in the Claude desktop app | Remove it in the app |
| `/tmp/hakoni-seed0` (downloads, recipe, policy, this report) | Gone at the next reboot (tmpfs) |

No group membership or system service was changed.

## Inputs for 2.4, 2.5 and the seed v0

- **Runtime:** OpenShell 0.1.10 with rootless Podman worked end to end on Ubuntu 26.04, without Docker and without root-equivalent groups. This is a candidate for v0.
- **Connection path:** SSH through `openshell ssh-proxy` works from a terminal and from the Claude desktop app. VS Code Remote-SSH was not tried.
- **Policy for Step 4:**
  - replace `"/**"` with the real binaries (`git`, `curl`, `uv`, `python3`, Claude Code's versioned binary under `~/.local/share/claude/`);
  - decide whether wildcards stay;
  - add the hosts Playwright needs if its browsers are wanted in the box.
- **Recipe for Step 4:**
  - pin the Claude Code installer, or copy a checked binary instead;
  - decide whether Claude Code may auto-update inside the box. Its binary hash is pinned by the policy on first use.
- **Open checks:**
  - workspace persistence across `stop` and `start` with Podman;
  - Claude Code's interactive login from a terminal inside the box;
  - how the desktop app passes its credentials to the session (the session itself holds none; see "Seen from inside the box");
  - Codex's discovery of `AGENTS.md` and `.agents/skills/` inside the box (2.4). Claude Code's is answered in "Seen from inside the box";
  - whether `HOME=/sandbox` for desktop-app sessions is wanted: it puts the account profile in the workspace volume;
  - Git identity and a push path for contributions from the box (Step 5).
- **Seed v0 wording:** see the table above. In particular: Podman as the default engine, the `localhost/` prefix, `/sandbox` as the workspace, the mandatory `binaries`, the include file written by hand, and the telemetry question asked before the install.
