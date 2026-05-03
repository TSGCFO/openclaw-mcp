---
source_url: https://docs.openclaw.ai/providers/opencode-go
title: "OpenCode Go - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/opencode-go#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

OpenCode Go

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Built-in catalog](https://docs.openclaw.ai/providers/opencode-go#built-in-catalog)
- [Getting started](https://docs.openclaw.ai/providers/opencode-go#getting-started)
- [Config example](https://docs.openclaw.ai/providers/opencode-go#config-example)
- [Advanced configuration](https://docs.openclaw.ai/providers/opencode-go#advanced-configuration)
- [Related](https://docs.openclaw.ai/providers/opencode-go#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenCode Go is the Go catalog within [OpenCode](https://docs.openclaw.ai/providers/opencode).
It uses the same `OPENCODE_API_KEY` as the Zen catalog, but keeps the runtime
provider id `opencode-go` so upstream per-model routing stays correct.

| Property | Value |
| --- | --- |
| Runtime provider | `opencode-go` |
| Auth | `OPENCODE_API_KEY` |
| Parent setup | [OpenCode](https://docs.openclaw.ai/providers/opencode) |

## [​](https://docs.openclaw.ai/providers/opencode-go\#built-in-catalog)  Built-in catalog

OpenClaw sources most Go catalog rows from the bundled pi model registry and
supplements current upstream rows while the registry catches up. Run
`openclaw models list --provider opencode-go` for the current model list.The provider includes:

| Model ref | Name |
| --- | --- |
| `opencode-go/glm-5` | GLM-5 |
| `opencode-go/glm-5.1` | GLM-5.1 |
| `opencode-go/kimi-k2.5` | Kimi K2.5 |
| `opencode-go/kimi-k2.6` | Kimi K2.6 (3x limits) |
| `opencode-go/deepseek-v4-pro` | DeepSeek V4 Pro |
| `opencode-go/deepseek-v4-flash` | DeepSeek V4 Flash |
| `opencode-go/mimo-v2-omni` | MiMo V2 Omni |
| `opencode-go/mimo-v2-pro` | MiMo V2 Pro |
| `opencode-go/minimax-m2.5` | MiniMax M2.5 |
| `opencode-go/minimax-m2.7` | MiniMax M2.7 |
| `opencode-go/qwen3.5-plus` | Qwen3.5 Plus |
| `opencode-go/qwen3.6-plus` | Qwen3.6 Plus |

## [​](https://docs.openclaw.ai/providers/opencode-go\#getting-started)  Getting started

- Interactive

- Non-interactive


1

[Navigate to header](https://docs.openclaw.ai/providers/opencode-go#)

Run onboarding

```
openclaw onboard --auth-choice opencode-go
```

2

[Navigate to header](https://docs.openclaw.ai/providers/opencode-go#)

Set a Go model as default

```
openclaw config set agents.defaults.model.primary "opencode-go/kimi-k2.6"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/opencode-go#)

Verify models are available

```
openclaw models list --provider opencode-go
```

1

[Navigate to header](https://docs.openclaw.ai/providers/opencode-go#)

Pass the key directly

```
openclaw onboard --opencode-go-api-key "$OPENCODE_API_KEY"
```

2

[Navigate to header](https://docs.openclaw.ai/providers/opencode-go#)

Verify models are available

```
openclaw models list --provider opencode-go
```

## [​](https://docs.openclaw.ai/providers/opencode-go\#config-example)  Config example

```
{
  env: { OPENCODE_API_KEY: "YOUR_API_KEY_HERE" }, // pragma: allowlist secret
  agents: { defaults: { model: { primary: "opencode-go/kimi-k2.6" } } },
}
```

## [​](https://docs.openclaw.ai/providers/opencode-go\#advanced-configuration)  Advanced configuration

Routing behavior

OpenClaw handles per-model routing automatically when the model ref uses
`opencode-go/...`. No additional provider config is required.

Runtime ref convention

Runtime refs stay explicit: `opencode/...` for Zen, `opencode-go/...` for Go.
This keeps upstream per-model routing correct across both catalogs.

Shared credentials

The same `OPENCODE_API_KEY` is used by both the Zen and Go catalogs. Entering
the key during setup stores credentials for both runtime providers.

See [OpenCode](https://docs.openclaw.ai/providers/opencode) for the shared onboarding overview and the full
Zen + Go catalog reference.

## [​](https://docs.openclaw.ai/providers/opencode-go\#related)  Related

[**OpenCode (parent)** \\
\\
Shared onboarding, catalog overview, and advanced notes.](https://docs.openclaw.ai/providers/opencode)

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[OpenCode](https://docs.openclaw.ai/providers/opencode) [OpenRouter](https://docs.openclaw.ai/providers/openrouter)

Ctrl+I