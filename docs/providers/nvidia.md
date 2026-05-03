---
source_url: https://docs.openclaw.ai/providers/nvidia
title: "NVIDIA - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/nvidia#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

NVIDIA

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/nvidia#getting-started)
- [Config example](https://docs.openclaw.ai/providers/nvidia#config-example)
- [Built-in catalog](https://docs.openclaw.ai/providers/nvidia#built-in-catalog)
- [Advanced configuration](https://docs.openclaw.ai/providers/nvidia#advanced-configuration)
- [Related](https://docs.openclaw.ai/providers/nvidia#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

NVIDIA provides an OpenAI-compatible API at `https://integrate.api.nvidia.com/v1` for
open models for free. Authenticate with an API key from
[build.nvidia.com](https://build.nvidia.com/settings/api-keys).

## [​](https://docs.openclaw.ai/providers/nvidia\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/nvidia#)

Get your API key

Create an API key at [build.nvidia.com](https://build.nvidia.com/settings/api-keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/nvidia#)

Export the key and run onboarding

```
export NVIDIA_API_KEY="nvapi-..."
openclaw onboard --auth-choice nvidia-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/nvidia#)

Set an NVIDIA model

```
openclaw models set nvidia/nvidia/nemotron-3-super-120b-a12b
```

If you pass `--nvidia-api-key` instead of the env var, the value lands in shell
history and `ps` output. Prefer the `NVIDIA_API_KEY` environment variable when
possible.

For non-interactive setup, you can also pass the key directly:

```
openclaw onboard --auth-choice nvidia-api-key --nvidia-api-key "nvapi-..."
```

## [​](https://docs.openclaw.ai/providers/nvidia\#config-example)  Config example

```
{
  env: { NVIDIA_API_KEY: "nvapi-..." },
  models: {
    providers: {
      nvidia: {
        baseUrl: "https://integrate.api.nvidia.com/v1",
        api: "openai-completions",
      },
    },
  },
  agents: {
    defaults: {
      model: { primary: "nvidia/nvidia/nemotron-3-super-120b-a12b" },
    },
  },
}
```

## [​](https://docs.openclaw.ai/providers/nvidia\#built-in-catalog)  Built-in catalog

| Model ref | Name | Context | Max output |
| --- | --- | --- | --- |
| `nvidia/nvidia/nemotron-3-super-120b-a12b` | NVIDIA Nemotron 3 Super 120B | 262,144 | 8,192 |
| `nvidia/moonshotai/kimi-k2.5` | Kimi K2.5 | 262,144 | 8,192 |
| `nvidia/minimaxai/minimax-m2.5` | Minimax M2.5 | 196,608 | 8,192 |
| `nvidia/z-ai/glm5` | GLM 5 | 202,752 | 8,192 |

## [​](https://docs.openclaw.ai/providers/nvidia\#advanced-configuration)  Advanced configuration

Auto-enable behavior

The provider auto-enables when the `NVIDIA_API_KEY` environment variable is set.
No explicit provider config is required beyond the key.

Catalog and pricing

The bundled catalog is static. Costs default to `0` in source since NVIDIA
currently offers free API access for the listed models.

OpenAI-compatible endpoint

NVIDIA uses the standard `/v1` completions endpoint. Any OpenAI-compatible
tooling should work out of the box with the NVIDIA base URL.

NVIDIA models are currently free to use. Check
[build.nvidia.com](https://build.nvidia.com/) for the latest availability and
rate-limit details.

## [​](https://docs.openclaw.ai/providers/nvidia\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config reference for agents, models, and providers.](https://docs.openclaw.ai/gateway/configuration-reference)

[Moonshot AI](https://docs.openclaw.ai/providers/moonshot) [Ollama](https://docs.openclaw.ai/providers/ollama)

Ctrl+I