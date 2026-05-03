---
source_url: https://docs.openclaw.ai/install/bun
title: "Bun (experimental) - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/install/bun#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Containers

Bun (experimental)

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Install](https://docs.openclaw.ai/install/bun#install)
- [Lifecycle scripts](https://docs.openclaw.ai/install/bun#lifecycle-scripts)
- [Caveats](https://docs.openclaw.ai/install/bun#caveats)
- [Related](https://docs.openclaw.ai/install/bun#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Bun is **not recommended for gateway runtime** (known issues with WhatsApp and Telegram). Use Node for production.

Bun is an optional local runtime for running TypeScript directly (`bun run ...`, `bun --watch ...`). The default package manager remains `pnpm`, which is fully supported and used by docs tooling. Bun cannot use `pnpm-lock.yaml` and will ignore it.

## [​](https://docs.openclaw.ai/install/bun\#install)  Install

1

[Navigate to header](https://docs.openclaw.ai/install/bun#)

Install dependencies

```
bun install
```

`bun.lock` / `bun.lockb` are gitignored, so there is no repo churn. To skip lockfile writes entirely:

```
bun install --no-save
```

2

[Navigate to header](https://docs.openclaw.ai/install/bun#)

Build and test

```
bun run build
bun run vitest run
```

## [​](https://docs.openclaw.ai/install/bun\#lifecycle-scripts)  Lifecycle scripts

Bun blocks dependency lifecycle scripts unless explicitly trusted. For this repo, the commonly blocked scripts are not required:

- `@whiskeysockets/baileys``preinstall` — checks Node major >= 20 (OpenClaw defaults to Node 24 and still supports Node 22 LTS, currently `22.14+`)
- `protobufjs``postinstall` — emits warnings about incompatible version schemes (no build artifacts)

If you hit a runtime issue that requires these scripts, trust them explicitly:

```
bun pm trust @whiskeysockets/baileys protobufjs
```

## [​](https://docs.openclaw.ai/install/bun\#caveats)  Caveats

Some scripts still hardcode pnpm (for example `docs:build`, `ui:*`, `protocol:check`). Run those via pnpm for now.

## [​](https://docs.openclaw.ai/install/bun\#related)  Related

- [Install overview](https://docs.openclaw.ai/install)
- [Node.js](https://docs.openclaw.ai/install/node)
- [Updating](https://docs.openclaw.ai/install/updating)

[Ansible](https://docs.openclaw.ai/install/ansible) [ClawDock](https://docs.openclaw.ai/install/clawdock)

Ctrl+I