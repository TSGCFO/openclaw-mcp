---
source_url: https://docs.openclaw.ai/providers/mistral
title: "Mistral - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/mistral#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Mistral

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/mistral#getting-started)
- [Built-in LLM catalog](https://docs.openclaw.ai/providers/mistral#built-in-llm-catalog)
- [Audio transcription (Voxtral)](https://docs.openclaw.ai/providers/mistral#audio-transcription-voxtral)
- [Voice Call streaming STT](https://docs.openclaw.ai/providers/mistral#voice-call-streaming-stt)
- [Advanced configuration](https://docs.openclaw.ai/providers/mistral#advanced-configuration)
- [Related](https://docs.openclaw.ai/providers/mistral#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw supports Mistral for both text/image model routing (`mistral/...`) and
audio transcription via Voxtral in media understanding.
Mistral can also be used for memory embeddings (`memorySearch.provider = "mistral"`).

- Provider: `mistral`
- Auth: `MISTRAL_API_KEY`
- API: Mistral Chat Completions (`https://api.mistral.ai/v1`)

## [​](https://docs.openclaw.ai/providers/mistral\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/mistral#)

Get your API key

Create an API key in the [Mistral Console](https://console.mistral.ai/).

2

[Navigate to header](https://docs.openclaw.ai/providers/mistral#)

Run onboarding

```
openclaw onboard --auth-choice mistral-api-key
```

Or pass the key directly:

```
openclaw onboard --mistral-api-key "$MISTRAL_API_KEY"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/mistral#)

Set a default model

```
{
  env: { MISTRAL_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "mistral/mistral-large-latest" } } },
}
```

4

[Navigate to header](https://docs.openclaw.ai/providers/mistral#)

Verify the model is available

```
openclaw models list --provider mistral
```

## [​](https://docs.openclaw.ai/providers/mistral\#built-in-llm-catalog)  Built-in LLM catalog

OpenClaw currently ships this bundled Mistral catalog:

| Model ref | Input | Context | Max output | Notes |
| --- | --- | --- | --- | --- |
| `mistral/mistral-large-latest` | text, image | 262,144 | 16,384 | Default model |
| `mistral/mistral-medium-2508` | text, image | 262,144 | 8,192 | Mistral Medium 3.1 |
| `mistral/mistral-small-latest` | text, image | 128,000 | 16,384 | Mistral Small 4; adjustable reasoning via API `reasoning_effort` |
| `mistral/pixtral-large-latest` | text, image | 128,000 | 32,768 | Pixtral |
| `mistral/codestral-latest` | text | 256,000 | 4,096 | Coding |
| `mistral/devstral-medium-latest` | text | 262,144 | 32,768 | Devstral 2 |
| `mistral/magistral-small` | text | 128,000 | 40,000 | Reasoning-enabled |

## [​](https://docs.openclaw.ai/providers/mistral\#audio-transcription-voxtral)  Audio transcription (Voxtral)

Use Voxtral for batch audio transcription through the media understanding
pipeline.

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "mistral", model: "voxtral-mini-latest" }],
      },
    },
  },
}
```

The media transcription path uses `/v1/audio/transcriptions`. The default audio model for Mistral is `voxtral-mini-latest`.

## [​](https://docs.openclaw.ai/providers/mistral\#voice-call-streaming-stt)  Voice Call streaming STT

The bundled `mistral` plugin registers Voxtral Realtime as a Voice Call
streaming STT provider.

| Setting | Config path | Default |
| --- | --- | --- |
| API key | `plugins.entries.voice-call.config.streaming.providers.mistral.apiKey` | Falls back to `MISTRAL_API_KEY` |
| Model | `...mistral.model` | `voxtral-mini-transcribe-realtime-2602` |
| Encoding | `...mistral.encoding` | `pcm_mulaw` |
| Sample rate | `...mistral.sampleRate` | `8000` |
| Target delay | `...mistral.targetStreamingDelayMs` | `800` |

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          streaming: {
            enabled: true,
            provider: "mistral",
            providers: {
              mistral: {
                apiKey: "${MISTRAL_API_KEY}",
                targetStreamingDelayMs: 800,
              },
            },
          },
        },
      },
    },
  },
}
```

OpenClaw defaults Mistral realtime STT to `pcm_mulaw` at 8 kHz so Voice Call
can forward Twilio media frames directly. Use `encoding: "pcm_s16le"` and a
matching `sampleRate` only if your upstream stream is already raw PCM.

## [​](https://docs.openclaw.ai/providers/mistral\#advanced-configuration)  Advanced configuration

Adjustable reasoning (mistral-small-latest)

`mistral/mistral-small-latest` maps to Mistral Small 4 and supports [adjustable reasoning](https://docs.mistral.ai/capabilities/reasoning/adjustable) on the Chat Completions API via `reasoning_effort` (`none` minimizes extra thinking in the output; `high` surfaces full thinking traces before the final answer).OpenClaw maps the session **thinking** level to Mistral’s API:

| OpenClaw thinking level | Mistral `reasoning_effort` |
| --- | --- |
| **off** / **minimal** | `none` |
| **low** / **medium** / **high** / **xhigh** / **adaptive** / **max** | `high` |

Other bundled Mistral catalog models do not use this parameter. Keep using `magistral-*` models when you want Mistral’s native reasoning-first behavior.

Memory embeddings

Mistral can serve memory embeddings via `/v1/embeddings` (default model: `mistral-embed`).

```
{
  memorySearch: { provider: "mistral" },
}
```

Auth and base URL

- Mistral auth uses `MISTRAL_API_KEY`.
- Provider base URL defaults to `https://api.mistral.ai/v1`.
- Onboarding default model is `mistral/mistral-large-latest`.
- Z.AI uses Bearer auth with your API key.

## [​](https://docs.openclaw.ai/providers/mistral\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Media understanding** \\
\\
Audio transcription setup and provider selection.](https://docs.openclaw.ai/nodes/media-understanding)

[MiniMax](https://docs.openclaw.ai/providers/minimax) [Moonshot AI](https://docs.openclaw.ai/providers/moonshot)

Ctrl+I