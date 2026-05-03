---
source_url: https://docs.openclaw.ai/install
title: "Install - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/install#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Install overview

Install

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [System requirements](https://docs.openclaw.ai/install#system-requirements)
- [Recommended: installer script](https://docs.openclaw.ai/install#recommended-installer-script)
- [Alternative install methods](https://docs.openclaw.ai/install#alternative-install-methods)
- [Local prefix installer (install-cli.sh)](https://docs.openclaw.ai/install#local-prefix-installer-install-cli-sh)
- [npm, pnpm, or bun](https://docs.openclaw.ai/install#npm-pnpm-or-bun)
- [From source](https://docs.openclaw.ai/install#from-source)
- [Install from GitHub main](https://docs.openclaw.ai/install#install-from-github-main)
- [Containers and package managers](https://docs.openclaw.ai/install#containers-and-package-managers)
- [Verify the install](https://docs.openclaw.ai/install#verify-the-install)
- [Hosting and deployment](https://docs.openclaw.ai/install#hosting-and-deployment)
- [Update, migrate, or uninstall](https://docs.openclaw.ai/install#update-migrate-or-uninstall)
- [Troubleshooting: openclaw not found](https://docs.openclaw.ai/install#troubleshooting-openclaw-not-found)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

## [​](https://docs.openclaw.ai/install\#system-requirements)  System requirements

- **Node 24** (recommended) or Node 22.14+ — the installer script handles this automatically
- **macOS, Linux, or Windows** — both native Windows and WSL2 are supported; WSL2 is more stable. See [Windows](https://docs.openclaw.ai/platforms/windows).
- `pnpm` is only needed if you build from source

## [​](https://docs.openclaw.ai/install\#recommended-installer-script)  Recommended: installer script

The fastest way to install. It detects your OS, installs Node if needed, installs OpenClaw, and launches onboarding.

- macOS / Linux / WSL2

- Windows (PowerShell)


```
curl -fsSL https://openclaw.ai/install.sh | bash
```

```
iwr -useb https://openclaw.ai/install.ps1 | iex
```

To install without running onboarding:

- macOS / Linux / WSL2

- Windows (PowerShell)


```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard
```

```
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -NoOnboard
```

For all flags and CI/automation options, see [Installer internals](https://docs.openclaw.ai/install/installer).

## [​](https://docs.openclaw.ai/install\#alternative-install-methods)  Alternative install methods

### [​](https://docs.openclaw.ai/install\#local-prefix-installer-install-cli-sh)  Local prefix installer (`install-cli.sh`)

Use this when you want OpenClaw and Node kept under a local prefix such as
`~/.openclaw`, without depending on a system-wide Node install:

```
curl -fsSL https://openclaw.ai/install-cli.sh | bash
```

It supports npm installs by default, plus git-checkout installs under the same
prefix flow. Full reference: [Installer internals](https://docs.openclaw.ai/install/installer#install-clish).Already installed? Switch between package and git installs with
`openclaw update --channel dev` and `openclaw update --channel stable`. See
[Updating](https://docs.openclaw.ai/install/updating#switch-between-npm-and-git-installs).

### [​](https://docs.openclaw.ai/install\#npm-pnpm-or-bun)  npm, pnpm, or bun

If you already manage Node yourself:

- npm

- pnpm

- bun


```
npm install -g openclaw@latest
openclaw onboard --install-daemon
```

```
pnpm add -g openclaw@latest
pnpm approve-builds -g
openclaw onboard --install-daemon
```

pnpm requires explicit approval for packages with build scripts. Run `pnpm approve-builds -g` after the first install.

```
bun add -g openclaw@latest
openclaw onboard --install-daemon
```

Bun is supported for the global CLI install path. For the Gateway runtime, Node remains the recommended daemon runtime.

Troubleshooting: sharp build errors (npm)

If `sharp` fails due to a globally installed libvips:

```
SHARP_IGNORE_GLOBAL_LIBVIPS=1 npm install -g openclaw@latest
```

### [​](https://docs.openclaw.ai/install\#from-source)  From source

For contributors or anyone who wants to run from a local checkout:

```
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install && pnpm build && pnpm ui:build
pnpm link --global
openclaw onboard --install-daemon
```

Or skip the link and use `pnpm openclaw ...` from inside the repo. See [Setup](https://docs.openclaw.ai/start/setup) for full development workflows.

### [​](https://docs.openclaw.ai/install\#install-from-github-main)  Install from GitHub main

```
npm install -g github:openclaw/openclaw#main
```

### [​](https://docs.openclaw.ai/install\#containers-and-package-managers)  Containers and package managers

[**Docker** \\
\\
Containerized or headless deployments.](https://docs.openclaw.ai/install/docker)

[**Podman** \\
\\
Rootless container alternative to Docker.](https://docs.openclaw.ai/install/podman)

[**Nix** \\
\\
Declarative install via Nix flake.](https://docs.openclaw.ai/install/nix)

[**Ansible** \\
\\
Automated fleet provisioning.](https://docs.openclaw.ai/install/ansible)

[**Bun** \\
\\
CLI-only usage via the Bun runtime.](https://docs.openclaw.ai/install/bun)

## [​](https://docs.openclaw.ai/install\#verify-the-install)  Verify the install

```
openclaw --version      # confirm the CLI is available
openclaw doctor         # check for config issues
openclaw gateway status # verify the Gateway is running
```

If you want managed startup after install:

- macOS: LaunchAgent via `openclaw onboard --install-daemon` or `openclaw gateway install`
- Linux/WSL2: systemd user service via the same commands
- Native Windows: Scheduled Task first, with a per-user Startup-folder login item fallback if task creation is denied

## [​](https://docs.openclaw.ai/install\#hosting-and-deployment)  Hosting and deployment

Deploy OpenClaw on a cloud server or VPS:

[**VPS** \\
\\
Any Linux VPS](https://docs.openclaw.ai/vps)

[**Docker VM** \\
\\
Shared Docker steps](https://docs.openclaw.ai/install/docker-vm-runtime)

[**Kubernetes** \\
\\
K8s](https://docs.openclaw.ai/install/kubernetes)

[**Fly.io** \\
\\
Fly.io](https://docs.openclaw.ai/install/fly)

[**Hetzner** \\
\\
Hetzner](https://docs.openclaw.ai/install/hetzner)

[**GCP** \\
\\
Google Cloud](https://docs.openclaw.ai/install/gcp)

[**Azure** \\
\\
Azure](https://docs.openclaw.ai/install/azure)

[**Railway** \\
\\
Railway](https://docs.openclaw.ai/install/railway)

[**Render** \\
\\
Render](https://docs.openclaw.ai/install/render)

[**Northflank** \\
\\
Northflank](https://docs.openclaw.ai/install/northflank)

## [​](https://docs.openclaw.ai/install\#update-migrate-or-uninstall)  Update, migrate, or uninstall

[**Updating** \\
\\
Keep OpenClaw up to date.](https://docs.openclaw.ai/install/updating)

[**Migrating** \\
\\
Move to a new machine.](https://docs.openclaw.ai/install/migrating)

[**Uninstall** \\
\\
Remove OpenClaw completely.](https://docs.openclaw.ai/install/uninstall)

## [​](https://docs.openclaw.ai/install\#troubleshooting-openclaw-not-found)  Troubleshooting: `openclaw` not found

If the install succeeded but `openclaw` is not found in your terminal:

```
node -v           # Node installed?
npm prefix -g     # Where are global packages?
echo "$PATH"      # Is the global bin dir in PATH?
```

If `$(npm prefix -g)/bin` is not in your `$PATH`, add it to your shell startup file (`~/.zshrc` or `~/.bashrc`):

```
export PATH="$(npm prefix -g)/bin:$PATH"
```

Then open a new terminal. See [Node setup](https://docs.openclaw.ai/install/node) for more details.

[Installer internals](https://docs.openclaw.ai/install/installer)

Ctrl+I