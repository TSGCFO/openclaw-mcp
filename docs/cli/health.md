---
source_url: https://docs.openclaw.ai/cli/health
title: "Health - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/health#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Gateway and service

Health

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw health](https://docs.openclaw.ai/cli/health#openclaw-health)
- [Related](https://docs.openclaw.ai/cli/health#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/health\#openclaw-health)  `openclaw health`

Fetch health from the running Gateway.Options:

- `--json`: machine-readable output
- `--timeout <ms>`: connection timeout in milliseconds (default `10000`)
- `--verbose`: verbose logging
- `--debug`: alias for `--verbose`

Examples:

```
openclaw health
openclaw health --json
openclaw health --timeout 2500
openclaw health --verbose
openclaw health --debug
```

Notes:

- Default `openclaw health` asks the running gateway for its health snapshot. When the
gateway already has a fresh cached snapshot, it can return that cached payload and
refresh in the background.
- `--verbose` forces a live probe, prints gateway connection details, and expands the
human-readable output across all configured accounts and agents.
- Output includes per-agent session stores when multiple agents are configured.

## [​](https://docs.openclaw.ai/cli/health\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Gateway health](https://docs.openclaw.ai/gateway/health)

[Gateway](https://docs.openclaw.ai/cli/gateway) [Logs](https://docs.openclaw.ai/cli/logs)

Ctrl+I