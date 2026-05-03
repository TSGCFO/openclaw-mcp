---
source_url: https://docs.openclaw.ai/providers/xiaomi
title: "Xiaomi MiMo - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/xiaomi#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Xiaomi MiMo

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/xiaomi#getting-started)
- [Built-in catalog](https://docs.openclaw.ai/providers/xiaomi#built-in-catalog)
- [Text-to-speech](https://docs.openclaw.ai/providers/xiaomi#text-to-speech)
- [Config example](https://docs.openclaw.ai/providers/xiaomi#config-example)
- [Related](https://docs.openclaw.ai/providers/xiaomi#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Xiaomi MiMo is the API platform for **MiMo** models. OpenClaw uses the Xiaomi
OpenAI-compatible endpoint with API-key authentication.

| Property | Value |
| --- | --- |
| Provider | `xiaomi` |
| Auth | `XIAOMI_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://api.xiaomimimo.com/v1` |

## [​](https://docs.openclaw.ai/providers/xiaomi\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/xiaomi#)

Get an API key

Create an API key in the [Xiaomi MiMo console](https://platform.xiaomimimo.com/#/console/api-keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/xiaomi#)

Run onboarding

```
openclaw onboard --auth-choice xiaomi-api-key
```

Or pass the key directly:

```
openclaw onboard --auth-choice xiaomi-api-key --xiaomi-api-key "$XIAOMI_API_KEY"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/xiaomi#)

Verify the model is available

```
openclaw models list --provider xiaomi
```

## [​](https://docs.openclaw.ai/providers/xiaomi\#built-in-catalog)  Built-in catalog

| Model ref | Input | Context | Max output | Reasoning | Notes |
| --- | --- | --- | --- | --- | --- |
| `xiaomi/mimo-v2-flash` | text | 262,144 | 8,192 | No | Default model |
| `xiaomi/mimo-v2-pro` | text | 1,048,576 | 32,000 | Yes | Large context |
| `xiaomi/mimo-v2-omni` | text, image | 262,144 | 32,000 | Yes | Multimodal |

The default model ref is `xiaomi/mimo-v2-flash`. The provider is injected automatically when `XIAOMI_API_KEY` is set or an auth profile exists.

## [​](https://docs.openclaw.ai/providers/xiaomi\#text-to-speech)  Text-to-speech

The bundled `xiaomi` plugin also registers Xiaomi MiMo as a speech provider for
`messages.tts`. It calls Xiaomi’s chat-completions TTS contract with the text as
an `assistant` message and optional style guidance as a `user` message.

| Property | Value |
| --- | --- |
| TTS id | `xiaomi` (`mimo` alias) |
| Auth | `XIAOMI_API_KEY` |
| API | `POST /v1/chat/completions` with `audio` |
| Default | `mimo-v2.5-tts`, voice `mimo_default` |
| Output | MP3 by default; WAV when configured |

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "xiaomi",
      providers: {
        xiaomi: {
          apiKey: "xiaomi_api_key",
          model: "mimo-v2.5-tts",
          voice: "mimo_default",
          format: "mp3",
          style: "Bright, natural, conversational tone.",
        },
      },
    },
  },
}
```

Supported built-in voices include `mimo_default`, `default_zh`, `default_en`,
`Mia`, `Chloe`, `Milo`, and `Dean`. `mimo-v2-tts` is supported for older MiMo
TTS accounts; the default uses the current MiMo-V2.5 TTS model. For voice-note
targets such as Feishu and Telegram, OpenClaw transcodes Xiaomi output to 48kHz
Opus with `ffmpeg` before delivery.

## [​](https://docs.openclaw.ai/providers/xiaomi\#config-example)  Config example

```
{
  env: { XIAOMI_API_KEY: "your-key" },
  agents: { defaults: { model: { primary: "xiaomi/mimo-v2-flash" } } },
  models: {
    mode: "merge",
    providers: {
      xiaomi: {
        baseUrl: "https://api.xiaomimimo.com/v1",
        api: "openai-completions",
        apiKey: "XIAOMI_API_KEY",
        models: [\
          {\
            id: "mimo-v2-flash",\
            name: "Xiaomi MiMo V2 Flash",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 262144,\
            maxTokens: 8192,\
          },\
          {\
            id: "mimo-v2-pro",\
            name: "Xiaomi MiMo V2 Pro",\
            reasoning: true,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 1048576,\
            maxTokens: 32000,\
          },\
          {\
            id: "mimo-v2-omni",\
            name: "Xiaomi MiMo V2 Omni",\
            reasoning: true,\
            input: ["text", "image"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 262144,\
            maxTokens: 32000,\
          },\
        ],
      },
    },
  },
}
```

Auto-injection behavior

The `xiaomi` provider is injected automatically when `XIAOMI_API_KEY` is set in your environment or an auth profile exists. You do not need to manually configure the provider unless you want to override model metadata or the base URL.

Model details

- **mimo-v2-flash** — lightweight and fast, ideal for general-purpose text tasks. No reasoning support.
- **mimo-v2-pro** — supports reasoning with a 1M token context window for long-document workloads.
- **mimo-v2-omni** — reasoning-enabled multimodal model that accepts both text and image inputs.

All models use the `xiaomi/` prefix (for example `xiaomi/mimo-v2-pro`).

Troubleshooting

- If models do not appear, confirm `XIAOMI_API_KEY` is set and valid.
- When the Gateway runs as a daemon, ensure the key is available to that process (for example in `~/.openclaw/.env` or via `env.shellEnv`).

Keys set only in your interactive shell are not visible to daemon-managed gateway processes. Use `~/.openclaw/.env` or `env.shellEnv` config for persistent availability.

## [​](https://docs.openclaw.ai/providers/xiaomi\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full OpenClaw configuration reference.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Xiaomi MiMo console** \\
\\
Xiaomi MiMo dashboard and API key management.](https://platform.xiaomimimo.com/)

[xAI](https://docs.openclaw.ai/providers/xai) [Z.AI](https://docs.openclaw.ai/providers/zai)

Ctrl+I