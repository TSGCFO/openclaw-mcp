---
source_url: https://docs.openclaw.ai/cli/dashboard
title: "Dashboard - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/cli/dashboard#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Interfaces

Dashboard

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [openclaw dashboard](https://docs.openclaw.ai/cli/dashboard#openclaw-dashboard)
- [Related](https://docs.openclaw.ai/cli/dashboard#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/cli/dashboard\#openclaw-dashboard)  `openclaw dashboard`

Open the Control UI using your current auth.

```
openclaw dashboard
openclaw dashboard --no-open
```

Notes:

- `dashboard` resolves configured `gateway.auth.token` SecretRefs when possible.
- `dashboard` follows `gateway.tls.enabled`: TLS-enabled gateways print/open
`https://` Control UI URLs and connect over `wss://`.
- For SecretRef-managed tokens (resolved or unresolved), `dashboard` prints/copies/opens a non-tokenized URL to avoid exposing external secrets in terminal output, clipboard history, or browser-launch arguments.
- If `gateway.auth.token` is SecretRef-managed but unresolved in this command path, the command prints a non-tokenized URL and explicit remediation guidance instead of embedding an invalid token placeholder.

## [​](https://docs.openclaw.ai/cli/dashboard\#related)  Related

- [CLI reference](https://docs.openclaw.ai/cli)
- [Dashboard](https://docs.openclaw.ai/web/dashboard)

[Skills](https://docs.openclaw.ai/cli/skills) [TUI](https://docs.openclaw.ai/cli/tui)

Ctrl+I