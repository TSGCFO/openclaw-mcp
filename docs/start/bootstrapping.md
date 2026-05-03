---
source_url: https://docs.openclaw.ai/start/bootstrapping
title: "Agent bootstrapping - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/start/bootstrapping#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Fundamentals

Agent bootstrapping

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [What bootstrapping does](https://docs.openclaw.ai/start/bootstrapping#what-bootstrapping-does)
- [Skipping bootstrapping](https://docs.openclaw.ai/start/bootstrapping#skipping-bootstrapping)
- [Where it runs](https://docs.openclaw.ai/start/bootstrapping#where-it-runs)
- [Related docs](https://docs.openclaw.ai/start/bootstrapping#related-docs)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Bootstrapping is the **first‑run** ritual that prepares an agent workspace and
collects identity details. It happens after onboarding, when the agent starts
for the first time.

## [​](https://docs.openclaw.ai/start/bootstrapping\#what-bootstrapping-does)  What bootstrapping does

On the first agent run, OpenClaw bootstraps the workspace (default
`~/.openclaw/workspace`):

- Seeds `AGENTS.md`, `BOOTSTRAP.md`, `IDENTITY.md`, `USER.md`.
- Runs a short Q&A ritual (one question at a time).
- Writes identity + preferences to `IDENTITY.md`, `USER.md`, `SOUL.md`.
- Removes `BOOTSTRAP.md` when finished so it only runs once.

For embedded/local model runs, OpenClaw keeps `BOOTSTRAP.md` out of the
privileged system context. On the primary interactive first run, it still passes
the file contents in the user prompt so models that do not reliably call the
`read` tool can complete the ritual. If the current run cannot safely access the
workspace, the agent gets a limited bootstrap note instead of a generic greeting.

## [​](https://docs.openclaw.ai/start/bootstrapping\#skipping-bootstrapping)  Skipping bootstrapping

To skip this for a pre-seeded workspace, run `openclaw onboard --skip-bootstrap`.

## [​](https://docs.openclaw.ai/start/bootstrapping\#where-it-runs)  Where it runs

Bootstrapping always runs on the **gateway host**. If the macOS app connects to
a remote Gateway, the workspace and bootstrapping files live on that remote
machine.

When the Gateway runs on another machine, edit workspace files on the gateway
host (for example, `user@gateway-host:~/.openclaw/workspace`).

## [​](https://docs.openclaw.ai/start/bootstrapping\#related-docs)  Related docs

- macOS app onboarding: [Onboarding](https://docs.openclaw.ai/start/onboarding)
- Workspace layout: [Agent workspace](https://docs.openclaw.ai/concepts/agent-workspace)

[OAuth](https://docs.openclaw.ai/concepts/oauth) [Experimental features](https://docs.openclaw.ai/concepts/experimental-features)

Ctrl+I