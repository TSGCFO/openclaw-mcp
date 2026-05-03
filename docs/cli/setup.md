---
source_url: https://docs.openclaw.ai/cli/setup
title: "Setup - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/setup#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Gateway and service

Setup

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw setup](https://docs.openclaw.ai/cli/setup#openclaw-setup)
- [Examples](https://docs.openclaw.ai/cli/setup#examples)
- [Options](https://docs.openclaw.ai/cli/setup#options)
- [Related](https://docs.openclaw.ai/cli/setup#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/setup\#openclaw-setup)  `openclaw setup`

Initialize `~/.openclaw/openclaw.json` and the agent workspace.Related:

- Getting started: [Getting started](https://docs.openclaw.ai/start/getting-started)
- CLI onboarding: [Onboarding (CLI)](https://docs.openclaw.ai/start/wizard)

## [​](https://docs.openclaw.ai/cli/setup\#examples)  Examples

```
openclaw setup
openclaw setup --workspace ~/.openclaw/workspace
openclaw setup --wizard
openclaw setup --wizard --import-from hermes --import-source ~/.hermes
openclaw setup --non-interactive --mode remote --remote-url wss://gateway-host:18789 --remote-token <token>
```

## [​](https://docs.openclaw.ai/cli/setup\#options)  Options

- `--workspace <dir>`: agent workspace directory (stored as `agents.defaults.workspace`)
- `--wizard`: run onboarding
- `--non-interactive`: run onboarding without prompts
- `--mode <local|remote>`: onboarding mode
- `--import-from <provider>`: migration provider to run during onboarding
- `--import-source <path>`: source agent home for `--import-from`
- `--import-secrets`: import supported secrets during onboarding migration
- `--remote-url <url>`: remote Gateway WebSocket URL
- `--remote-token <token>`: remote Gateway token

To run onboarding via setup:

```
openclaw setup --wizard
```

Notes:

- Plain `openclaw setup` initializes config + workspace without the full onboarding flow.
- After plain setup, run `openclaw configure` to choose models, channels, Gateway, plugins, skills, or health checks.
- Onboarding auto-runs when any onboarding flags are present (`--wizard`, `--non-interactive`, `--mode`, `--import-from`, `--import-source`, `--import-secrets`, `--remote-url`, `--remote-token`).
- If Hermes state is detected, interactive onboarding can offer migration automatically. Import onboarding requires a fresh setup; use [Migrate](https://docs.openclaw.ai/cli/migrate) for dry-run plans, backups, and overwrite mode outside onboarding.

## [​](https://docs.openclaw.ai/cli/setup\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Install overview](https://docs.openclaw.ai/install)

[Security](https://docs.openclaw.ai/cli/security) [Status](https://docs.openclaw.ai/cli/status)

Ctrl+I