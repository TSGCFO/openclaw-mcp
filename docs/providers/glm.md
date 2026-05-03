---
source_url: https://docs.openclaw.ai/providers/glm
title: "GLM (Zhipu) - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/glm#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

GLM (Zhipu)

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [GLM models](https://docs.openclaw.ai/providers/glm#glm-models)
- [Getting started](https://docs.openclaw.ai/providers/glm#getting-started)
- [Config example](https://docs.openclaw.ai/providers/glm#config-example)
- [Built-in catalog](https://docs.openclaw.ai/providers/glm#built-in-catalog)
- [Advanced configuration](https://docs.openclaw.ai/providers/glm#advanced-configuration)
- [Related](https://docs.openclaw.ai/providers/glm#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/providers/glm\#glm-models)  GLM models

GLM is a **model family** (not a company) available through the Z.AI platform. In OpenClaw, GLM
models are accessed via the `zai` provider and model IDs like `zai/glm-5`.

## [​](https://docs.openclaw.ai/providers/glm\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/glm#)

Choose an auth route and run onboarding

Pick the onboarding choice that matches your Z.AI plan and region:

| Auth choice | Best for |
| --- | --- |
| `zai-api-key` | Generic API-key setup with endpoint auto-detection |
| `zai-coding-global` | Coding Plan users (global) |
| `zai-coding-cn` | Coding Plan users (China region) |
| `zai-global` | General API (global) |
| `zai-cn` | General API (China region) |

```
# Example: generic auto-detect
openclaw onboard --auth-choice zai-api-key

# Example: Coding Plan global
openclaw onboard --auth-choice zai-coding-global
```

2

[Navigate to header](https://docs.openclaw.ai/providers/glm#)

Set GLM as the default model

```
openclaw config set agents.defaults.model.primary "zai/glm-5.1"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/glm#)

Verify models are available

```
openclaw models list --provider zai
```

## [​](https://docs.openclaw.ai/providers/glm\#config-example)  Config example

```
{
  env: { ZAI_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "zai/glm-5.1" } } },
}
```

`zai-api-key` lets OpenClaw detect the matching Z.AI endpoint from the key and
apply the correct base URL automatically. Use the explicit regional choices when
you want to force a specific Coding Plan or general API surface.

## [​](https://docs.openclaw.ai/providers/glm\#built-in-catalog)  Built-in catalog

OpenClaw currently seeds the bundled `zai` provider with these GLM refs:

| Model | Model |
| --- | --- |
| `glm-5.1` | `glm-4.7` |
| `glm-5` | `glm-4.7-flash` |
| `glm-5-turbo` | `glm-4.7-flashx` |
| `glm-5v-turbo` | `glm-4.6` |
| `glm-4.5` | `glm-4.6v` |
| `glm-4.5-air` |  |
| `glm-4.5-flash` |  |
| `glm-4.5v` |  |

The default bundled model ref is `zai/glm-5.1`. GLM versions and availability
can change; check Z.AI’s docs for the latest.

## [​](https://docs.openclaw.ai/providers/glm\#advanced-configuration)  Advanced configuration

Endpoint auto-detection

When you use the `zai-api-key` auth choice, OpenClaw inspects the key format
to determine the correct Z.AI base URL. Explicit regional choices
(`zai-coding-global`, `zai-coding-cn`, `zai-global`, `zai-cn`) override
auto-detection and pin the endpoint directly.

Provider details

GLM models are served by the `zai` runtime provider. For full provider
configuration, regional endpoints, and additional capabilities, see
[Z.AI provider docs](https://docs.openclaw.ai/providers/zai).

## [​](https://docs.openclaw.ai/providers/glm\#related)  Related

[**Z.AI provider** \\
\\
Full Z.AI provider configuration and regional endpoints.](https://docs.openclaw.ai/providers/zai)

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[GitHub Copilot](https://docs.openclaw.ai/providers/github-copilot) [Google (Gemini)](https://docs.openclaw.ai/providers/google)

Ctrl+I