---
source_url: https://docs.openclaw.ai/providers/together
title: "Together AI - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/together#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Together AI

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/together#getting-started)
- [Non-interactive example](https://docs.openclaw.ai/providers/together#non-interactive-example)
- [Built-in catalog](https://docs.openclaw.ai/providers/together#built-in-catalog)
- [Video generation](https://docs.openclaw.ai/providers/together#video-generation)
- [Related](https://docs.openclaw.ai/providers/together#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

[Together AI](https://together.ai/) provides access to leading open-source
models including Llama, DeepSeek, Kimi, and more through a unified API.

| Property | Value |
| --- | --- |
| Provider | `together` |
| Auth | `TOGETHER_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://api.together.xyz/v1` |

## [​](https://docs.openclaw.ai/providers/together\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/together#)

Get an API key

Create an API key at
[api.together.ai/settings/api-keys](https://api.together.ai/settings/api-keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/together#)

Run onboarding

```
openclaw onboard --auth-choice together-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/together#)

Set a default model

```
{
  agents: {
    defaults: {
      model: { primary: "together/moonshotai/Kimi-K2.5" },
    },
  },
}
```

### [​](https://docs.openclaw.ai/providers/together\#non-interactive-example)  Non-interactive example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice together-api-key \
  --together-api-key "$TOGETHER_API_KEY"
```

The onboarding preset sets `together/moonshotai/Kimi-K2.5` as the default
model.

## [​](https://docs.openclaw.ai/providers/together\#built-in-catalog)  Built-in catalog

OpenClaw ships this bundled Together catalog:

| Model ref | Name | Input | Context | Notes |
| --- | --- | --- | --- | --- |
| `together/moonshotai/Kimi-K2.5` | Kimi K2.5 | text, image | 262,144 | Default model; reasoning enabled |
| `together/zai-org/GLM-4.7` | GLM 4.7 Fp8 | text | 202,752 | General-purpose text model |
| `together/meta-llama/Llama-3.3-70B-Instruct-Turbo` | Llama 3.3 70B Instruct Turbo | text | 131,072 | Fast instruction model |
| `together/meta-llama/Llama-4-Scout-17B-16E-Instruct` | Llama 4 Scout 17B 16E Instruct | text, image | 10,000,000 | Multimodal |
| `together/meta-llama/Llama-4-Maverick-17B-128E-Instruct-FP8` | Llama 4 Maverick 17B 128E Instruct FP8 | text, image | 20,000,000 | Multimodal |
| `together/deepseek-ai/DeepSeek-V3.1` | DeepSeek V3.1 | text | 131,072 | General text model |
| `together/deepseek-ai/DeepSeek-R1` | DeepSeek R1 | text | 131,072 | Reasoning model |
| `together/moonshotai/Kimi-K2-Instruct-0905` | Kimi K2-Instruct 0905 | text | 262,144 | Secondary Kimi text model |

## [​](https://docs.openclaw.ai/providers/together\#video-generation)  Video generation

The bundled `together` plugin also registers video generation through the
shared `video_generate` tool.

| Property | Value |
| --- | --- |
| Default video model | `together/Wan-AI/Wan2.2-T2V-A14B` |
| Modes | text-to-video, single-image reference |
| Supported parameters | `aspectRatio`, `resolution` |

To use Together as the default video provider:

```
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "together/Wan-AI/Wan2.2-T2V-A14B",
      },
    },
  },
}
```

See [Video Generation](https://docs.openclaw.ai/tools/video-generation) for the shared tool parameters,
provider selection, and failover behavior.

Environment note

If the Gateway runs as a daemon (launchd/systemd), make sure
`TOGETHER_API_KEY` is available to that process (for example, in
`~/.openclaw/.env` or via `env.shellEnv`).

Keys set only in your interactive shell are not visible to daemon-managed
gateway processes. Use `~/.openclaw/.env` or `env.shellEnv` config for
persistent availability.

Troubleshooting

- Verify your key works: `openclaw models list --provider together`
- If models are not appearing, confirm the API key is set in the correct
environment for your Gateway process.
- Model refs use the form `together/<model-id>`.

## [​](https://docs.openclaw.ai/providers/together\#related)  Related

[**Model selection** \\
\\
Provider rules, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Video generation** \\
\\
Shared video generation tool parameters and provider selection.](https://docs.openclaw.ai/tools/video-generation)

[**Configuration reference** \\
\\
Full config schema including provider settings.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Together AI** \\
\\
Together AI dashboard, API docs, and pricing.](https://together.ai/)

[Tencent Cloud (TokenHub)](https://docs.openclaw.ai/providers/tencent) [Venice AI](https://docs.openclaw.ai/providers/venice)

Ctrl+I