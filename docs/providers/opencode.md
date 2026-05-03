---
source_url: https://docs.openclaw.ai/providers/opencode
title: "OpenCode - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/opencode#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

OpenCode

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/opencode#getting-started)
- [Config example](https://docs.openclaw.ai/providers/opencode#config-example)
- [Built-in catalogs](https://docs.openclaw.ai/providers/opencode#built-in-catalogs)
- [Zen](https://docs.openclaw.ai/providers/opencode#zen)
- [Go](https://docs.openclaw.ai/providers/opencode#go)
- [Advanced configuration](https://docs.openclaw.ai/providers/opencode#advanced-configuration)
- [Related](https://docs.openclaw.ai/providers/opencode#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenCode exposes two hosted catalogs in OpenClaw:

| Catalog | Prefix | Runtime provider |
| --- | --- | --- |
| **Zen** | `opencode/...` | `opencode` |
| **Go** | `opencode-go/...` | `opencode-go` |

Both catalogs use the same OpenCode API key. OpenClaw keeps the runtime provider ids
split so upstream per-model routing stays correct, but onboarding and docs treat them
as one OpenCode setup.

## [​](https://docs.openclaw.ai/providers/opencode\#getting-started)  Getting started

- Zen catalog

- Go catalog


**Best for:** the curated OpenCode multi-model proxy (Claude, GPT, Gemini).

1

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Run onboarding

```
openclaw onboard --auth-choice opencode-zen
```

Or pass the key directly:

```
openclaw onboard --opencode-zen-api-key "$OPENCODE_API_KEY"
```

2

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Set a Zen model as the default

```
openclaw config set agents.defaults.model.primary "opencode/claude-opus-4-6"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Verify models are available

```
openclaw models list --provider opencode
```

**Best for:** the OpenCode-hosted Kimi, GLM, and MiniMax lineup.

1

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Run onboarding

```
openclaw onboard --auth-choice opencode-go
```

Or pass the key directly:

```
openclaw onboard --opencode-go-api-key "$OPENCODE_API_KEY"
```

2

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Set a Go model as the default

```
openclaw config set agents.defaults.model.primary "opencode-go/kimi-k2.6"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Verify models are available

```
openclaw models list --provider opencode-go
```

## [​](https://docs.openclaw.ai/providers/opencode\#config-example)  Config example

```
{
  env: { OPENCODE_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "opencode/claude-opus-4-6" } } },
}
```

## [​](https://docs.openclaw.ai/providers/opencode\#built-in-catalogs)  Built-in catalogs

### [​](https://docs.openclaw.ai/providers/opencode\#zen)  Zen

| Property | Value |
| --- | --- |
| Runtime provider | `opencode` |
| Example models | `opencode/claude-opus-4-6`, `opencode/gpt-5.5`, `opencode/gemini-3-pro` |

### [​](https://docs.openclaw.ai/providers/opencode\#go)  Go

| Property | Value |
| --- | --- |
| Runtime provider | `opencode-go` |
| Example models | `opencode-go/kimi-k2.6`, `opencode-go/glm-5`, `opencode-go/minimax-m2.5` |

## [​](https://docs.openclaw.ai/providers/opencode\#advanced-configuration)  Advanced configuration

API key aliases

`OPENCODE_ZEN_API_KEY` is also supported as an alias for `OPENCODE_API_KEY`.

Shared credentials

Entering one OpenCode key during setup stores credentials for both runtime
providers. You do not need to onboard each catalog separately.

Billing and dashboard

You sign in to OpenCode, add billing details, and copy your API key. Billing
and catalog availability are managed from the OpenCode dashboard.

Gemini replay behavior

Gemini-backed OpenCode refs stay on the proxy-Gemini path, so OpenClaw keeps
Gemini thought-signature sanitation there without enabling native Gemini
replay validation or bootstrap rewrites.

Non-Gemini replay behavior

Non-Gemini OpenCode refs keep the minimal OpenAI-compatible replay policy.

Entering one OpenCode key during setup stores credentials for both the Zen and
Go runtime providers, so you only need to onboard once.

## [​](https://docs.openclaw.ai/providers/opencode\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config reference for agents, models, and providers.](https://docs.openclaw.ai/gateway/configuration-reference)

[OpenAI](https://docs.openclaw.ai/providers/openai) [OpenCode Go](https://docs.openclaw.ai/providers/opencode-go)

Ctrl+I