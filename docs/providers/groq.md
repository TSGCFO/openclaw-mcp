---
source_url: https://docs.openclaw.ai/providers/groq
title: "Groq - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/groq#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Groq

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/groq#getting-started)
- [Config file example](https://docs.openclaw.ai/providers/groq#config-file-example)
- [Built-in catalog](https://docs.openclaw.ai/providers/groq#built-in-catalog)
- [Reasoning models](https://docs.openclaw.ai/providers/groq#reasoning-models)
- [Audio transcription](https://docs.openclaw.ai/providers/groq#audio-transcription)
- [Related](https://docs.openclaw.ai/providers/groq#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

[Groq](https://groq.com/) provides ultra-fast inference on open-source models
(Llama, Gemma, Mistral, and more) using custom LPU hardware. OpenClaw connects
to Groq through its OpenAI-compatible API.

| Property | Value |
| --- | --- |
| Provider | `groq` |
| Auth | `GROQ_API_KEY` |
| API | OpenAI-compatible |

## [​](https://docs.openclaw.ai/providers/groq\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/groq#)

Get an API key

Create an API key at [console.groq.com/keys](https://console.groq.com/keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/groq#)

Set the API key

```
export GROQ_API_KEY="gsk_..."
```

3

[Navigate to header](https://docs.openclaw.ai/providers/groq#)

Set a default model

```
{
  agents: {
    defaults: {
      model: { primary: "groq/llama-3.3-70b-versatile" },
    },
  },
}
```

### [​](https://docs.openclaw.ai/providers/groq\#config-file-example)  Config file example

```
{
  env: { GROQ_API_KEY: "gsk_..." },
  agents: {
    defaults: {
      model: { primary: "groq/llama-3.3-70b-versatile" },
    },
  },
}
```

## [​](https://docs.openclaw.ai/providers/groq\#built-in-catalog)  Built-in catalog

OpenClaw ships a manifest-backed Groq catalog for fast provider-filtered model
listing. Run `openclaw models list --all --provider groq` to see the bundled
rows, or check
[console.groq.com/docs/models](https://console.groq.com/docs/models).

| Model | Notes |
| --- | --- |
| **Llama 3.3 70B Versatile** | General-purpose, large context |
| **Llama 3.1 8B Instant** | Fast, lightweight |
| **Gemma 2 9B** | Compact, efficient |
| **Mixtral 8x7B** | MoE architecture, strong reasoning |

Use `openclaw models list --all --provider groq` for the manifest-backed Groq
rows known to this OpenClaw version.

## [​](https://docs.openclaw.ai/providers/groq\#reasoning-models)  Reasoning models

OpenClaw maps its shared `/think` levels to Groq’s model-specific
`reasoning_effort` values. For `qwen/qwen3-32b`, disabled thinking sends
`none` and enabled thinking sends `default`. For Groq GPT-OSS reasoning models,
OpenClaw sends `low`, `medium`, or `high`; disabled thinking omits
`reasoning_effort` because those models do not support a disabled value.

## [​](https://docs.openclaw.ai/providers/groq\#audio-transcription)  Audio transcription

Groq also provides fast Whisper-based audio transcription. When configured as a
media-understanding provider, OpenClaw uses Groq’s `whisper-large-v3-turbo`
model to transcribe voice messages through the shared `tools.media.audio`
surface.

```
{
  tools: {
    media: {
      audio: {
        models: [{ provider: "groq" }],
      },
    },
  },
}
```

Audio transcription details

| Property | Value |
| --- | --- |
| Shared config path | `tools.media.audio` |
| Default base URL | `https://api.groq.com/openai/v1` |
| Default model | `whisper-large-v3-turbo` |
| API endpoint | OpenAI-compatible `/audio/transcriptions` |

Environment note

If the Gateway runs as a daemon (launchd/systemd), make sure `GROQ_API_KEY` is
available to that process (for example, in `~/.openclaw/.env` or via
`env.shellEnv`).

Keys set only in your interactive shell are not visible to daemon-managed
gateway processes. Use `~/.openclaw/.env` or `env.shellEnv` config for
persistent availability.

## [​](https://docs.openclaw.ai/providers/groq\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config schema including provider and audio settings.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Groq Console** \\
\\
Groq dashboard, API docs, and pricing.](https://console.groq.com/)

[**Groq model list** \\
\\
Official Groq model catalog.](https://console.groq.com/docs/models)

[Gradium](https://docs.openclaw.ai/providers/gradium) [Hugging Face (inference)](https://docs.openclaw.ai/providers/huggingface)

Ctrl+I