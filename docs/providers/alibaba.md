---
source_url: https://docs.openclaw.ai/providers/alibaba
title: "Alibaba Model Studio - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/alibaba#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Alibaba Model Studio

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/alibaba#getting-started)
- [Built-in Wan models](https://docs.openclaw.ai/providers/alibaba#built-in-wan-models)
- [Current limits](https://docs.openclaw.ai/providers/alibaba#current-limits)
- [Advanced configuration](https://docs.openclaw.ai/providers/alibaba#advanced-configuration)
- [Related](https://docs.openclaw.ai/providers/alibaba#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw ships a bundled `alibaba` video-generation provider for Wan models on
Alibaba Model Studio / DashScope.

- Provider: `alibaba`
- Preferred auth: `MODELSTUDIO_API_KEY`
- Also accepted: `DASHSCOPE_API_KEY`, `QWEN_API_KEY`
- API: DashScope / Model Studio async video generation

## [​](https://docs.openclaw.ai/providers/alibaba\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/alibaba#)

Set an API key

```
openclaw onboard --auth-choice qwen-standard-api-key
```

2

[Navigate to header](https://docs.openclaw.ai/providers/alibaba#)

Set a default video model

```
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "alibaba/wan2.6-t2v",
      },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/alibaba#)

Verify the provider is available

```
openclaw models list --provider alibaba
```

Any of the accepted auth keys (`MODELSTUDIO_API_KEY`, `DASHSCOPE_API_KEY`, `QWEN_API_KEY`) will work. The `qwen-standard-api-key` onboarding choice configures the shared DashScope credential.

## [​](https://docs.openclaw.ai/providers/alibaba\#built-in-wan-models)  Built-in Wan models

The bundled `alibaba` provider currently registers:

| Model ref | Mode |
| --- | --- |
| `alibaba/wan2.6-t2v` | Text-to-video |
| `alibaba/wan2.6-i2v` | Image-to-video |
| `alibaba/wan2.6-r2v` | Reference-to-video |
| `alibaba/wan2.6-r2v-flash` | Reference-to-video (fast) |
| `alibaba/wan2.7-r2v` | Reference-to-video |

## [​](https://docs.openclaw.ai/providers/alibaba\#current-limits)  Current limits

| Parameter | Limit |
| --- | --- |
| Output videos | Up to **1** per request |
| Input images | Up to **1** |
| Input videos | Up to **4** |
| Duration | Up to **10 seconds** |
| Supported controls | `size`, `aspectRatio`, `resolution`, `audio`, `watermark` |
| Reference image/video | Remote `http(s)` URLs only |

Reference image/video mode currently requires **remote http(s) URLs**. Local file paths are not supported for reference inputs.

## [​](https://docs.openclaw.ai/providers/alibaba\#advanced-configuration)  Advanced configuration

Relationship to Qwen

The bundled `qwen` provider also uses Alibaba-hosted DashScope endpoints for
Wan video generation. Use:

- `qwen/...` when you want the canonical Qwen provider surface
- `alibaba/...` when you want the direct vendor-owned Wan video surface

See the [Qwen provider docs](https://docs.openclaw.ai/providers/qwen) for more detail.

Auth key priority

OpenClaw checks for auth keys in this order:

1. `MODELSTUDIO_API_KEY` (preferred)
2. `DASHSCOPE_API_KEY`
3. `QWEN_API_KEY`

Any of these will authenticate the `alibaba` provider.

## [​](https://docs.openclaw.ai/providers/alibaba\#related)  Related

[**Video generation** \\
\\
Shared video tool parameters and provider selection.](https://docs.openclaw.ai/tools/video-generation)

[**Qwen** \\
\\
Qwen provider setup and DashScope integration.](https://docs.openclaw.ai/providers/qwen)

[**Configuration reference** \\
\\
Agent defaults and model configuration.](https://docs.openclaw.ai/gateway/config-agents#agent-defaults)

[Model failover](https://docs.openclaw.ai/concepts/model-failover) [Amazon Bedrock](https://docs.openclaw.ai/providers/bedrock)

Ctrl+I