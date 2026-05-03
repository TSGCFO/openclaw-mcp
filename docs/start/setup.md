---
source_url: https://docs.openclaw.ai/start/setup
title: "Setup - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/start/setup#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Advanced setup

Setup

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [TL;DR](https://docs.openclaw.ai/start/setup#tldr)
- [Prereqs (from source)](https://docs.openclaw.ai/start/setup#prereqs-from-source)
- [Tailoring strategy (so updates do not hurt)](https://docs.openclaw.ai/start/setup#tailoring-strategy-so-updates-do-not-hurt)
- [Run the Gateway from this repo](https://docs.openclaw.ai/start/setup#run-the-gateway-from-this-repo)
- [Stable workflow (macOS app first)](https://docs.openclaw.ai/start/setup#stable-workflow-macos-app-first)
- [Bleeding edge workflow (Gateway in a terminal)](https://docs.openclaw.ai/start/setup#bleeding-edge-workflow-gateway-in-a-terminal)
- [0) (Optional) Run the macOS app from source too](https://docs.openclaw.ai/start/setup#0-optional-run-the-macos-app-from-source-too)
- [1) Start the dev Gateway](https://docs.openclaw.ai/start/setup#1-start-the-dev-gateway)
- [2) Point the macOS app at your running Gateway](https://docs.openclaw.ai/start/setup#2-point-the-macos-app-at-your-running-gateway)
- [3) Verify](https://docs.openclaw.ai/start/setup#3-verify)
- [Common footguns](https://docs.openclaw.ai/start/setup#common-footguns)
- [Credential storage map](https://docs.openclaw.ai/start/setup#credential-storage-map)
- [Updating (without wrecking your setup)](https://docs.openclaw.ai/start/setup#updating-without-wrecking-your-setup)
- [Linux (systemd user service)](https://docs.openclaw.ai/start/setup#linux-systemd-user-service)
- [Related docs](https://docs.openclaw.ai/start/setup#related-docs)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

If you are setting up for the first time, start with [Getting Started](https://docs.openclaw.ai/start/getting-started).
For onboarding details, see [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard).

## [​](https://docs.openclaw.ai/start/setup\#tldr)  TL;DR

Pick a setup workflow based on how often you want updates and whether you want to run the Gateway yourself:

- **Tailoring lives outside the repo:** keep your config and workspace in `~/.openclaw/openclaw.json` and `~/.openclaw/workspace/` so repo updates don’t touch them.
- **Stable workflow (recommended for most):** install the macOS app and let it run the bundled Gateway.
- **Bleeding edge workflow (dev):** run the Gateway yourself via `pnpm gateway:watch`, then let the macOS app attach in Local mode.

## [​](https://docs.openclaw.ai/start/setup\#prereqs-from-source)  Prereqs (from source)

- Node 24 recommended (Node 22 LTS, currently `22.14+`, still supported)
- `pnpm` required for source checkouts. OpenClaw loads bundled plugins from the
`extensions/*` pnpm workspace packages in dev mode, so root `npm install` does
not prepare the full source tree.
- Docker (optional; only for containerized setup/e2e — see [Docker](https://docs.openclaw.ai/install/docker))

## [​](https://docs.openclaw.ai/start/setup\#tailoring-strategy-so-updates-do-not-hurt)  Tailoring strategy (so updates do not hurt)

If you want “100% tailored to me” _and_ easy updates, keep your customization in:

- **Config:**`~/.openclaw/openclaw.json` (JSON/JSON5-ish)
- **Workspace:**`~/.openclaw/workspace` (skills, prompts, memories; make it a private git repo)

Bootstrap once:

```
openclaw setup
```

From inside this repo, use the local CLI entry:

```
openclaw setup
```

If you don’t have a global install yet, run it via `pnpm openclaw setup`.

## [​](https://docs.openclaw.ai/start/setup\#run-the-gateway-from-this-repo)  Run the Gateway from this repo

After `pnpm build`, you can run the packaged CLI directly:

```
node openclaw.mjs gateway --port 18789 --verbose
```

## [​](https://docs.openclaw.ai/start/setup\#stable-workflow-macos-app-first)  Stable workflow (macOS app first)

1. Install + launch **OpenClaw.app** (menu bar).
2. Complete the onboarding/permissions checklist (TCC prompts).
3. Ensure Gateway is **Local** and running (the app manages it).
4. Link surfaces (example: WhatsApp):

```
openclaw channels login
```

5. Sanity check:

```
openclaw health
```

If onboarding is not available in your build:

- Run `openclaw setup`, then `openclaw channels login`, then start the Gateway manually (`openclaw gateway`).

## [​](https://docs.openclaw.ai/start/setup\#bleeding-edge-workflow-gateway-in-a-terminal)  Bleeding edge workflow (Gateway in a terminal)

Goal: work on the TypeScript Gateway, get hot reload, keep the macOS app UI attached.

### [​](https://docs.openclaw.ai/start/setup\#0-optional-run-the-macos-app-from-source-too)  0) (Optional) Run the macOS app from source too

If you also want the macOS app on the bleeding edge:

```
./scripts/restart-mac.sh
```

### [​](https://docs.openclaw.ai/start/setup\#1-start-the-dev-gateway)  1) Start the dev Gateway

```
pnpm install
# First run only (or after resetting local OpenClaw config/workspace)
pnpm openclaw setup
pnpm gateway:watch
```

`gateway:watch` starts or restarts the Gateway watch process in a named tmux
session and auto-attaches from interactive terminals. Non-interactive shells stay
detached and print `tmux attach -t openclaw-gateway-watch-main`; use
`OPENCLAW_GATEWAY_WATCH_ATTACH=0 pnpm gateway:watch` to keep an interactive run
detached, or `pnpm gateway:watch:raw` for foreground watch mode. The watcher
reloads on relevant source, config, and bundled-plugin metadata changes.
`pnpm openclaw setup` is the one-time local config/workspace initialization step for a fresh checkout.
`pnpm gateway:watch` does not rebuild `dist/control-ui`, so rerun `pnpm ui:build` after `ui/` changes or use `pnpm ui:dev` while developing the Control UI.

### [​](https://docs.openclaw.ai/start/setup\#2-point-the-macos-app-at-your-running-gateway)  2) Point the macOS app at your running Gateway

In **OpenClaw.app**:

- Connection Mode: **Local**
The app will attach to the running gateway on the configured port.

### [​](https://docs.openclaw.ai/start/setup\#3-verify)  3) Verify

- In-app Gateway status should read **“Using existing gateway …”**
- Or via CLI:

```
openclaw health
```

### [​](https://docs.openclaw.ai/start/setup\#common-footguns)  Common footguns

- **Wrong port:** Gateway WS defaults to `ws://127.0.0.1:18789`; keep app + CLI on the same port.
- **Where state lives:**
  - Channel/provider state: `~/.openclaw/credentials/`
  - Model auth profiles: `~/.openclaw/agents/<agentId>/agent/auth-profiles.json`
  - Sessions: `~/.openclaw/agents/<agentId>/sessions/`
  - Logs: `/tmp/openclaw/`

## [​](https://docs.openclaw.ai/start/setup\#credential-storage-map)  Credential storage map

Use this when debugging auth or deciding what to back up:

- **WhatsApp**: `~/.openclaw/credentials/whatsapp/<accountId>/creds.json`
- **Telegram bot token**: config/env or `channels.telegram.tokenFile` (regular file only; symlinks rejected)
- **Discord bot token**: config/env or SecretRef (env/file/exec providers)
- **Slack tokens**: config/env (`channels.slack.*`)
- **Pairing allowlists**:

  - `~/.openclaw/credentials/<channel>-allowFrom.json` (default account)
  - `~/.openclaw/credentials/<channel>-<accountId>-allowFrom.json` (non-default accounts)
- **Model auth profiles**: `~/.openclaw/agents/<agentId>/agent/auth-profiles.json`
- **File-backed secrets payload (optional)**: `~/.openclaw/secrets.json`
- **Legacy OAuth import**: `~/.openclaw/credentials/oauth.json`
More detail: [Security](https://docs.openclaw.ai/gateway/security#credential-storage-map).

## [​](https://docs.openclaw.ai/start/setup\#updating-without-wrecking-your-setup)  Updating (without wrecking your setup)

- Keep `~/.openclaw/workspace` and `~/.openclaw/` as “your stuff”; don’t put personal prompts/config into the `openclaw` repo.
- Updating source: `git pull` \+ `pnpm install` \+ keep using `pnpm gateway:watch`.

## [​](https://docs.openclaw.ai/start/setup\#linux-systemd-user-service)  Linux (systemd user service)

Linux installs use a systemd **user** service. By default, systemd stops user
services on logout/idle, which kills the Gateway. Onboarding attempts to enable
lingering for you (may prompt for sudo). If it’s still off, run:

```
sudo loginctl enable-linger $USER
```

For always-on or multi-user servers, consider a **system** service instead of a
user service (no lingering needed). See [Gateway runbook](https://docs.openclaw.ai/gateway) for the systemd notes.

## [​](https://docs.openclaw.ai/start/setup\#related-docs)  Related docs

- [Gateway runbook](https://docs.openclaw.ai/gateway) (flags, supervision, ports)
- [Gateway configuration](https://docs.openclaw.ai/gateway/configuration) (config schema + examples)
- [Discord](https://docs.openclaw.ai/channels/discord) and [Telegram](https://docs.openclaw.ai/channels/telegram) (reply tags + replyToMode settings)
- [OpenClaw assistant setup](https://docs.openclaw.ai/start/openclaw)
- [macOS app](https://docs.openclaw.ai/platforms/macos) (gateway lifecycle)

[Render](https://docs.openclaw.ai/install/render) [Pi development workflow](https://docs.openclaw.ai/pi-dev)

Ctrl+I