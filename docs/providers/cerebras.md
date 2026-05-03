---
source_url: https://docs.openclaw.ai/providers/cerebras
title: "Cerebras - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/cerebras#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Cerebras

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting Started](https://docs.openclaw.ai/providers/cerebras#getting-started)
- [Non-Interactive Setup](https://docs.openclaw.ai/providers/cerebras#non-interactive-setup)
- [Built-In Catalog](https://docs.openclaw.ai/providers/cerebras#built-in-catalog)
- [Manual Config](https://docs.openclaw.ai/providers/cerebras#manual-config)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

[Cerebras](https://www.cerebras.ai/) provides high-speed OpenAI-compatible inference.

| Property | Value |
| --- | --- |
| Provider | `cerebras` |
| Auth | `CEREBRAS_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://api.cerebras.ai/v1` |

## [​](https://docs.openclaw.ai/providers/cerebras\#getting-started)  Getting Started

1

[Navigate to header](https://docs.openclaw.ai/providers/cerebras#)

Get an API key

Create an API key in the [Cerebras Cloud Console](https://cloud.cerebras.ai/).

2

[Navigate to header](https://docs.openclaw.ai/providers/cerebras#)

Run onboarding

```
openclaw onboard --auth-choice cerebras-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/cerebras#)

Verify models are available

```
openclaw models list --provider cerebras
```

### [​](https://docs.openclaw.ai/providers/cerebras\#non-interactive-setup)  Non-Interactive Setup

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice cerebras-api-key \
  --cerebras-api-key "$CEREBRAS_API_KEY"
```

## [​](https://docs.openclaw.ai/providers/cerebras\#built-in-catalog)  Built-In Catalog

OpenClaw ships a static Cerebras catalog for the public OpenAI-compatible endpoint:

| Model ref | Name | Notes |
| --- | --- | --- |
| `cerebras/zai-glm-4.7` | Z.ai GLM 4.7 | Default model; preview reasoning model |
| `cerebras/gpt-oss-120b` | GPT OSS 120B | Production reasoning model |
| `cerebras/qwen-3-235b-a22b-instruct-2507` | Qwen 3 235B Instruct | Preview non-reasoning model |
| `cerebras/llama3.1-8b` | Llama 3.1 8B | Production speed-focused model |

Cerebras marks `zai-glm-4.7` and `qwen-3-235b-a22b-instruct-2507` as preview models, and `llama3.1-8b` / `qwen-3-235b-a22b-instruct-2507` are documented for deprecation on May 27, 2026. Check Cerebras’ supported-models page before relying on them for production.

## [​](https://docs.openclaw.ai/providers/cerebras\#manual-config)  Manual Config

The bundled plugin usually means you only need the API key. Use explicit
`models.providers.cerebras` config when you want to override model metadata:

```
{
  env: { CEREBRAS_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "cerebras/zai-glm-4.7" },
    },
  },
  models: {
    mode: "merge",
    providers: {
      cerebras: {
        baseUrl: "https://api.cerebras.ai/v1",
        apiKey: "${CEREBRAS_API_KEY}",
        api: "openai-completions",
        models: [\
          { id: "zai-glm-4.7", name: "Z.ai GLM 4.7" },\
          { id: "gpt-oss-120b", name: "GPT OSS 120B" },\
        ],
      },
    },
  },
}
```

If the Gateway runs as a daemon (launchd/systemd), make sure `CEREBRAS_API_KEY`
is available to that process, for example in `~/.openclaw/.env` or through
`env.shellEnv`.

[Azure Speech](https://docs.openclaw.ai/providers/azure-speech) [Chutes](https://docs.openclaw.ai/providers/chutes)

Ctrl+I