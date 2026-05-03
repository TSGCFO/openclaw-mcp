---
source_url: https://docs.openclaw.ai/providers/zai
title: "Z.AI - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/zai#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Z.AI

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/zai#getting-started)
- [Built-in catalog](https://docs.openclaw.ai/providers/zai#built-in-catalog)
- [Advanced configuration](https://docs.openclaw.ai/providers/zai#advanced-configuration)
- [Related](https://docs.openclaw.ai/providers/zai#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Z.AI is the API platform for **GLM** models. It provides REST APIs for GLM and uses API keys
for authentication. Create your API key in the Z.AI console. OpenClaw uses the `zai` provider
with a Z.AI API key.

- Provider: `zai`
- Auth: `ZAI_API_KEY`
- API: Z.AI Chat Completions (Bearer auth)

## [​](https://docs.openclaw.ai/providers/zai\#getting-started)  Getting started

- Auto-detect endpoint

- Explicit regional endpoint


**Best for:** most users. OpenClaw detects the matching Z.AI endpoint from the key and applies the correct base URL automatically.

1

[Navigate to header](https://docs.openclaw.ai/providers/zai#)

Run onboarding

```
openclaw onboard --auth-choice zai-api-key
```

2

[Navigate to header](https://docs.openclaw.ai/providers/zai#)

Set a default model

```
{
  env: { ZAI_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "zai/glm-5.1" } } },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/zai#)

Verify the model is listed

```
openclaw models list --all --provider zai
```

**Best for:** users who want to force a specific Coding Plan or general API surface.

1

[Navigate to header](https://docs.openclaw.ai/providers/zai#)

Pick the right onboarding choice

```
# Coding Plan Global (recommended for Coding Plan users)
openclaw onboard --auth-choice zai-coding-global

# Coding Plan CN (China region)
openclaw onboard --auth-choice zai-coding-cn

# General API
openclaw onboard --auth-choice zai-global

# General API CN (China region)
openclaw onboard --auth-choice zai-cn
```

2

[Navigate to header](https://docs.openclaw.ai/providers/zai#)

Set a default model

```
{
  env: { ZAI_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "zai/glm-5.1" } } },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/zai#)

Verify the model is listed

```
openclaw models list --all --provider zai
```

## [​](https://docs.openclaw.ai/providers/zai\#built-in-catalog)  Built-in catalog

OpenClaw ships the bundled `zai` provider catalog in the plugin manifest, so read-only
listing can show known GLM rows without loading provider runtime:

```
openclaw models list --all --provider zai
```

The manifest-backed catalog currently includes:

| Model ref | Notes |
| --- | --- |
| `zai/glm-5.1` | Default model |
| `zai/glm-5` |  |
| `zai/glm-5-turbo` |  |
| `zai/glm-5v-turbo` |  |
| `zai/glm-4.7` |  |
| `zai/glm-4.7-flash` |  |
| `zai/glm-4.7-flashx` |  |
| `zai/glm-4.6` |  |
| `zai/glm-4.6v` |  |
| `zai/glm-4.5` |  |
| `zai/glm-4.5-air` |  |
| `zai/glm-4.5-flash` |  |
| `zai/glm-4.5v` |  |

GLM models are available as `zai/<model>` (example: `zai/glm-5`). The default bundled model ref is `zai/glm-5.1`.

## [​](https://docs.openclaw.ai/providers/zai\#advanced-configuration)  Advanced configuration

Forward-resolving unknown GLM-5 models

Unknown `glm-5*` ids still forward-resolve on the bundled provider path by
synthesizing provider-owned metadata from the `glm-4.7` template when the id
matches the current GLM-5 family shape.

Tool-call streaming

`tool_stream` is enabled by default for Z.AI tool-call streaming. To disable it:

```
{
  agents: {
    defaults: {
      models: {
        "zai/<model>": {
          params: { tool_stream: false },
        },
      },
    },
  },
}
```

Thinking and preserved thinking

Z.AI thinking follows OpenClaw’s `/think` controls. With thinking off,
OpenClaw sends `thinking: { type: "disabled" }` to avoid responses that
spend the output budget on `reasoning_content` before visible text.Preserved thinking is opt-in because Z.AI requires the full historical
`reasoning_content` to be replayed, which increases prompt tokens. Enable it
per model:

```
{
  agents: {
    defaults: {
      models: {
        "zai/glm-5.1": {
          params: { preserveThinking: true },
        },
      },
    },
  },
}
```

When enabled and thinking is on, OpenClaw sends
`thinking: { type: "enabled", clear_thinking: false }` and replays prior
`reasoning_content` for the same OpenAI-compatible transcript.Advanced users can still override the exact provider payload with
`params.extra_body.thinking`.

Image understanding

The bundled Z.AI plugin registers image understanding.

| Property | Value |
| --- | --- |
| Model | `glm-4.6v` |

Image understanding is auto-resolved from the configured Z.AI auth — no
additional config is needed.

Auth details

- Z.AI uses Bearer auth with your API key.
- The `zai-api-key` onboarding choice auto-detects the matching Z.AI endpoint from the key prefix.
- Use the explicit regional choices (`zai-coding-global`, `zai-coding-cn`, `zai-global`, `zai-cn`) when you want to force a specific API surface.

## [​](https://docs.openclaw.ai/providers/zai\#related)  Related

[**GLM model family** \\
\\
Model family overview for GLM.](https://docs.openclaw.ai/providers/glm)

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[Xiaomi MiMo](https://docs.openclaw.ai/providers/xiaomi)

Ctrl+I