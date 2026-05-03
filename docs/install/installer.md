---
source_url: https://docs.openclaw.ai/install/installer
title: "Installer internals - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/install/installer#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Install overview

Installer internals

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Quick commands](https://docs.openclaw.ai/install/installer#quick-commands)
- [install.sh](https://docs.openclaw.ai/install/installer#install-sh)
- [Flow (install.sh)](https://docs.openclaw.ai/install/installer#flow-install-sh)
- [Source checkout detection](https://docs.openclaw.ai/install/installer#source-checkout-detection)
- [Examples (install.sh)](https://docs.openclaw.ai/install/installer#examples-install-sh)
- [install-cli.sh](https://docs.openclaw.ai/install/installer#install-cli-sh)
- [Flow (install-cli.sh)](https://docs.openclaw.ai/install/installer#flow-install-cli-sh)
- [Examples (install-cli.sh)](https://docs.openclaw.ai/install/installer#examples-install-cli-sh)
- [install.ps1](https://docs.openclaw.ai/install/installer#install-ps1)
- [Flow (install.ps1)](https://docs.openclaw.ai/install/installer#flow-install-ps1)
- [Examples (install.ps1)](https://docs.openclaw.ai/install/installer#examples-install-ps1)
- [CI and automation](https://docs.openclaw.ai/install/installer#ci-and-automation)
- [Troubleshooting](https://docs.openclaw.ai/install/installer#troubleshooting)
- [Related](https://docs.openclaw.ai/install/installer#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw ships three installer scripts, served from `openclaw.ai`.

| Script | Platform | What it does |
| --- | --- | --- |
| [`install.sh`](https://docs.openclaw.ai/install/installer#installsh) | macOS / Linux / WSL | Installs Node if needed, installs OpenClaw via npm (default) or git, and can run onboarding. |
| [`install-cli.sh`](https://docs.openclaw.ai/install/installer#install-clish) | macOS / Linux / WSL | Installs Node + OpenClaw into a local prefix (`~/.openclaw`) with npm or git checkout modes. No root required. |
| [`install.ps1`](https://docs.openclaw.ai/install/installer#installps1) | Windows (PowerShell) | Installs Node if needed, installs OpenClaw via npm (default) or git, and can run onboarding. |

## [​](https://docs.openclaw.ai/install/installer\#quick-commands)  Quick commands

- install.sh

- install-cli.sh

- install.ps1


```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash
```

```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash -s -- --help
```

```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install-cli.sh | bash
```

```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install-cli.sh | bash -s -- --help
```

```
iwr -useb https://openclaw.ai/install.ps1 | iex
```

```
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -Tag beta -NoOnboard -DryRun
```

If install succeeds but `openclaw` is not found in a new terminal, see [Node.js troubleshooting](https://docs.openclaw.ai/install/node#troubleshooting).

* * *

## [​](https://docs.openclaw.ai/install/installer\#install-sh)  install.sh

Recommended for most interactive installs on macOS/Linux/WSL.

### [​](https://docs.openclaw.ai/install/installer\#flow-install-sh)  Flow (install.sh)

1

[Navigate to header](https://docs.openclaw.ai/install/installer#)

Detect OS

Supports macOS and Linux (including WSL). If macOS is detected, installs Homebrew if missing.

2

[Navigate to header](https://docs.openclaw.ai/install/installer#)

Ensure Node.js 24 by default

Checks Node version and installs Node 24 if needed (Homebrew on macOS, NodeSource setup scripts on Linux apt/dnf/yum). OpenClaw still supports Node 22 LTS, currently `22.14+`, for compatibility.

3

[Navigate to header](https://docs.openclaw.ai/install/installer#)

Ensure Git

Installs Git if missing.

4

[Navigate to header](https://docs.openclaw.ai/install/installer#)

Install OpenClaw

- `npm` method (default): global npm install
- `git` method: clone/update repo, install deps with pnpm, build, then install wrapper at `~/.local/bin/openclaw`

5

[Navigate to header](https://docs.openclaw.ai/install/installer#)

Post-install tasks

- Refreshes a loaded gateway service best-effort (`openclaw gateway install --force`, then restart)
- Runs `openclaw doctor --non-interactive` on upgrades and git installs (best effort)
- Attempts onboarding when appropriate (TTY available, onboarding not disabled, and bootstrap/config checks pass)
- Defaults `SHARP_IGNORE_GLOBAL_LIBVIPS=1`

### [​](https://docs.openclaw.ai/install/installer\#source-checkout-detection)  Source checkout detection

If run inside an OpenClaw checkout (`package.json` \+ `pnpm-workspace.yaml`), the script offers:

- use checkout (`git`), or
- use global install (`npm`)

If no TTY is available and no install method is set, it defaults to `npm` and warns.The script exits with code `2` for invalid method selection or invalid `--install-method` values.

### [​](https://docs.openclaw.ai/install/installer\#examples-install-sh)  Examples (install.sh)

- Default

- Skip onboarding

- Git install

- GitHub main via npm

- Dry run


```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash
```

```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash -s -- --no-onboard
```

```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash -s -- --install-method git
```

```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash -s -- --version main
```

```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash -s -- --dry-run
```

Flags reference

| Flag | Description |
| --- | --- |
| `--install-method npm|git` | Choose install method (default: `npm`). Alias: `--method` |
| `--npm` | Shortcut for npm method |
| `--git` | Shortcut for git method. Alias: `--github` |
| `--version <version|dist-tag|spec>` | npm version, dist-tag, or package spec (default: `latest`) |
| `--beta` | Use beta dist-tag if available, else fallback to `latest` |
| `--git-dir <path>` | Checkout directory (default: `~/openclaw`). Alias: `--dir` |
| `--no-git-update` | Skip `git pull` for existing checkout |
| `--no-prompt` | Disable prompts |
| `--no-onboard` | Skip onboarding |
| `--onboard` | Enable onboarding |
| `--dry-run` | Print actions without applying changes |
| `--verbose` | Enable debug output (`set -x`, npm notice-level logs) |
| `--help` | Show usage (`-h`) |

Environment variables reference

| Variable | Description |
| --- | --- |
| `OPENCLAW_INSTALL_METHOD=git|npm` | Install method |
| `OPENCLAW_VERSION=latest|next|main|<semver>|<spec>` | npm version, dist-tag, or package spec |
| `OPENCLAW_BETA=0|1` | Use beta if available |
| `OPENCLAW_GIT_DIR=<path>` | Checkout directory |
| `OPENCLAW_GIT_UPDATE=0|1` | Toggle git updates |
| `OPENCLAW_NO_PROMPT=1` | Disable prompts |
| `OPENCLAW_NO_ONBOARD=1` | Skip onboarding |
| `OPENCLAW_DRY_RUN=1` | Dry run mode |
| `OPENCLAW_VERBOSE=1` | Debug mode |
| `OPENCLAW_NPM_LOGLEVEL=error|warn|notice` | npm log level |
| `SHARP_IGNORE_GLOBAL_LIBVIPS=0|1` | Control sharp/libvips behavior (default: `1`) |

* * *

## [​](https://docs.openclaw.ai/install/installer\#install-cli-sh)  install-cli.sh

Designed for environments where you want everything under a local prefix
(default `~/.openclaw`) and no system Node dependency. Supports npm installs
by default, plus git-checkout installs under the same prefix flow.

### [​](https://docs.openclaw.ai/install/installer\#flow-install-cli-sh)  Flow (install-cli.sh)

1

[Navigate to header](https://docs.openclaw.ai/install/installer#)

Install local Node runtime

Downloads a pinned supported Node LTS tarball (the version is embedded in the script and updated independently) to `<prefix>/tools/node-v<version>` and verifies SHA-256.

2

[Navigate to header](https://docs.openclaw.ai/install/installer#)

Ensure Git

If Git is missing, attempts install via apt/dnf/yum on Linux or Homebrew on macOS.

3

[Navigate to header](https://docs.openclaw.ai/install/installer#)

Install OpenClaw under prefix

- `npm` method (default): installs under the prefix with npm, then writes wrapper to `<prefix>/bin/openclaw`
- `git` method: clones/updates a checkout (default `~/openclaw`) and still writes the wrapper to `<prefix>/bin/openclaw`

4

[Navigate to header](https://docs.openclaw.ai/install/installer#)

Refresh loaded gateway service

If a gateway service is already loaded from that same prefix, the script runs
`openclaw gateway install --force`, then `openclaw gateway restart`, and
probes gateway health best-effort.

### [​](https://docs.openclaw.ai/install/installer\#examples-install-cli-sh)  Examples (install-cli.sh)

- Default

- Custom prefix + version

- Git install

- Automation JSON output

- Run onboarding


```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install-cli.sh | bash
```

```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install-cli.sh | bash -s -- --prefix /opt/openclaw --version latest
```

```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install-cli.sh | bash -s -- --install-method git --git-dir ~/openclaw
```

```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install-cli.sh | bash -s -- --json --prefix /opt/openclaw
```

```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install-cli.sh | bash -s -- --onboard
```

Flags reference

| Flag | Description |
| --- | --- |
| `--prefix <path>` | Install prefix (default: `~/.openclaw`) |
| `--install-method npm|git` | Choose install method (default: `npm`). Alias: `--method` |
| `--npm` | Shortcut for npm method |
| `--git`, `--github` | Shortcut for git method |
| `--git-dir <path>` | Git checkout directory (default: `~/openclaw`). Alias: `--dir` |
| `--version <ver>` | OpenClaw version or dist-tag (default: `latest`) |
| `--node-version <ver>` | Node version (default: `22.22.0`) |
| `--json` | Emit NDJSON events |
| `--onboard` | Run `openclaw onboard` after install |
| `--no-onboard` | Skip onboarding (default) |
| `--set-npm-prefix` | On Linux, force npm prefix to `~/.npm-global` if current prefix is not writable |
| `--help` | Show usage (`-h`) |

Environment variables reference

| Variable | Description |
| --- | --- |
| `OPENCLAW_PREFIX=<path>` | Install prefix |
| `OPENCLAW_INSTALL_METHOD=git|npm` | Install method |
| `OPENCLAW_VERSION=<ver>` | OpenClaw version or dist-tag |
| `OPENCLAW_NODE_VERSION=<ver>` | Node version |
| `OPENCLAW_GIT_DIR=<path>` | Git checkout directory for git installs |
| `OPENCLAW_GIT_UPDATE=0|1` | Toggle git updates for existing checkouts |
| `OPENCLAW_NO_ONBOARD=1` | Skip onboarding |
| `OPENCLAW_NPM_LOGLEVEL=error|warn|notice` | npm log level |
| `SHARP_IGNORE_GLOBAL_LIBVIPS=0|1` | Control sharp/libvips behavior (default: `1`) |

* * *

## [​](https://docs.openclaw.ai/install/installer\#install-ps1)  install.ps1

### [​](https://docs.openclaw.ai/install/installer\#flow-install-ps1)  Flow (install.ps1)

1

[Navigate to header](https://docs.openclaw.ai/install/installer#)

Ensure PowerShell + Windows environment

Requires PowerShell 5+.

2

[Navigate to header](https://docs.openclaw.ai/install/installer#)

Ensure Node.js 24 by default

If missing, attempts install via winget, then Chocolatey, then Scoop. Node 22 LTS, currently `22.14+`, remains supported for compatibility.

3

[Navigate to header](https://docs.openclaw.ai/install/installer#)

Install OpenClaw

- `npm` method (default): global npm install using selected `-Tag`, launched from a writable installer temp directory so shells opened in protected folders such as `C:\` still work
- `git` method: clone/update repo, install/build with pnpm, and install wrapper at `%USERPROFILE%\.local\bin\openclaw.cmd`

4

[Navigate to header](https://docs.openclaw.ai/install/installer#)

Post-install tasks

- Adds needed bin directory to user PATH when possible
- Refreshes a loaded gateway service best-effort (`openclaw gateway install --force`, then restart)
- Runs `openclaw doctor --non-interactive` on upgrades and git installs (best effort)

5

[Navigate to header](https://docs.openclaw.ai/install/installer#)

Handle failures

`iwr ... | iex` and scriptblock installs report a terminating error without closing the current PowerShell session. Direct `powershell -File` / `pwsh -File` installs still exit non-zero for automation.

### [​](https://docs.openclaw.ai/install/installer\#examples-install-ps1)  Examples (install.ps1)

- Default

- Git install

- GitHub main via npm

- Custom git directory

- Dry run

- Debug trace


```
iwr -useb https://openclaw.ai/install.ps1 | iex
```

```
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -InstallMethod git
```

```
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -Tag main
```

```
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -InstallMethod git -GitDir "C:\openclaw"
```

```
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -DryRun
```

```
# install.ps1 has no dedicated -Verbose flag yet.
Set-PSDebug -Trace 1
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -NoOnboard
Set-PSDebug -Trace 0
```

Flags reference

| Flag | Description |
| --- | --- |
| `-InstallMethod npm|git` | Install method (default: `npm`) |
| `-Tag <tag|version|spec>` | npm dist-tag, version, or package spec (default: `latest`) |
| `-GitDir <path>` | Checkout directory (default: `%USERPROFILE%\openclaw`) |
| `-NoOnboard` | Skip onboarding |
| `-NoGitUpdate` | Skip `git pull` |
| `-DryRun` | Print actions only |

Environment variables reference

| Variable | Description |
| --- | --- |
| `OPENCLAW_INSTALL_METHOD=git|npm` | Install method |
| `OPENCLAW_GIT_DIR=<path>` | Checkout directory |
| `OPENCLAW_NO_ONBOARD=1` | Skip onboarding |
| `OPENCLAW_GIT_UPDATE=0` | Disable git pull |
| `OPENCLAW_DRY_RUN=1` | Dry run mode |

If `-InstallMethod git` is used and Git is missing, the script exits and prints the Git for Windows link.

* * *

## [​](https://docs.openclaw.ai/install/installer\#ci-and-automation)  CI and automation

Use non-interactive flags/env vars for predictable runs.

- install.sh (non-interactive npm)

- install.sh (non-interactive git)

- install-cli.sh (JSON)

- install.ps1 (skip onboarding)


```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash -s -- --no-prompt --no-onboard
```

```
OPENCLAW_INSTALL_METHOD=git OPENCLAW_NO_PROMPT=1 \
  curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash
```

```
curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install-cli.sh | bash -s -- --json --prefix /opt/openclaw
```

```
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -NoOnboard
```

* * *

## [​](https://docs.openclaw.ai/install/installer\#troubleshooting)  Troubleshooting

Why is Git required?

Git is required for `git` install method. For `npm` installs, Git is still checked/installed to avoid `spawn git ENOENT` failures when dependencies use git URLs.

Why does npm hit EACCES on Linux?

Some Linux setups point npm global prefix to root-owned paths. `install.sh` can switch prefix to `~/.npm-global` and append PATH exports to shell rc files (when those files exist).

sharp/libvips issues

The scripts default `SHARP_IGNORE_GLOBAL_LIBVIPS=1` to avoid sharp building against system libvips. To override:

```
SHARP_IGNORE_GLOBAL_LIBVIPS=0 curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install.sh | bash
```

Windows: "npm error spawn git / ENOENT"

Install Git for Windows, reopen PowerShell, rerun installer.

Windows: "openclaw is not recognized"

Run `npm config get prefix` and add that directory to your user PATH (no `\bin` suffix needed on Windows), then reopen PowerShell.

Windows: how to get verbose installer output

`install.ps1` does not currently expose a `-Verbose` switch.
Use PowerShell tracing for script-level diagnostics:

```
Set-PSDebug -Trace 1
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -NoOnboard
Set-PSDebug -Trace 0
```

openclaw not found after install

Usually a PATH issue. See [Node.js troubleshooting](https://docs.openclaw.ai/install/node#troubleshooting).

## [​](https://docs.openclaw.ai/install/installer\#related)  Related

- [Install overview](https://docs.openclaw.ai/install)
- [Updating](https://docs.openclaw.ai/install/updating)
- [Uninstall](https://docs.openclaw.ai/install/uninstall)

[Install](https://docs.openclaw.ai/install) [Node.js](https://docs.openclaw.ai/install/node)

Ctrl+I