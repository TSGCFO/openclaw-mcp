---
source_url: https://docs.openclaw.ai/providers/openrouter
title: "OpenRouter - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/openrouter#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

OpenRouter

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/openrouter#getting-started)
- [Config example](https://docs.openclaw.ai/providers/openrouter#config-example)
- [Model references](https://docs.openclaw.ai/providers/openrouter#model-references)
- [Image generation](https://docs.openclaw.ai/providers/openrouter#image-generation)
- [Video generation](https://docs.openclaw.ai/providers/openrouter#video-generation)
- [Text-to-speech](https://docs.openclaw.ai/providers/openrouter#text-to-speech)
- [Authentication and headers](https://docs.openclaw.ai/providers/openrouter#authentication-and-headers)
- [Advanced configuration](https://docs.openclaw.ai/providers/openrouter#advanced-configuration)
- [Related](https://docs.openclaw.ai/providers/openrouter#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenRouter provides a **unified API** that routes requests to many models behind a single
endpoint and API key. It is OpenAI-compatible, so most OpenAI SDKs work by switching the base URL.

## [​](https://docs.openclaw.ai/providers/openrouter\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/openrouter#)

Get your API key

Create an API key at [openrouter.ai/keys](https://openrouter.ai/keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/openrouter#)

Run onboarding

```
openclaw onboard --auth-choice openrouter-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/openrouter#)

(Optional) Switch to a specific model

Onboarding defaults to `openrouter/auto`. Pick a concrete model later:

```
openclaw models set openrouter/<provider>/<model>
```

## [​](https://docs.openclaw.ai/providers/openrouter\#config-example)  Config example

```
{
  env: { OPENROUTER_API_KEY: "sk-or-..." },
  agents: {
    defaults: {
      model: { primary: "openrouter/auto" },
    },
  },
}
```

## [​](https://docs.openclaw.ai/providers/openrouter\#model-references)  Model references

Model refs follow the pattern `openrouter/<provider>/<model>`. For the full list of
available providers and models, see [/concepts/model-providers](https://docs.openclaw.ai/concepts/model-providers).

Bundled fallback examples:

| Model ref | Notes |
| --- | --- |
| `openrouter/auto` | OpenRouter automatic routing |
| `openrouter/moonshotai/kimi-k2.6` | Kimi K2.6 via MoonshotAI |

## [​](https://docs.openclaw.ai/providers/openrouter\#image-generation)  Image generation

OpenRouter can also back the `image_generate` tool. Use an OpenRouter image model under `agents.defaults.imageGenerationModel`:

```
{
  env: { OPENROUTER_API_KEY: "sk-or-..." },
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "openrouter/google/gemini-3.1-flash-image-preview",
        timeoutMs: 180_000,
      },
    },
  },
}
```

OpenClaw sends image requests to OpenRouter’s chat completions image API with `modalities: ["image", "text"]`. Gemini image models receive supported `aspectRatio` and `resolution` hints through OpenRouter’s `image_config`. Use `agents.defaults.imageGenerationModel.timeoutMs` for slower OpenRouter image models; the `image_generate` tool’s per-call `timeoutMs` parameter still wins.

## [​](https://docs.openclaw.ai/providers/openrouter\#video-generation)  Video generation

OpenRouter can also back the `video_generate` tool through its asynchronous `/videos` API. Use an OpenRouter video model under `agents.defaults.videoGenerationModel`:

```
{
  env: { OPENROUTER_API_KEY: "sk-or-..." },
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "openrouter/google/veo-3.1-fast",
      },
    },
  },
}
```

OpenClaw submits text-to-video and image-to-video jobs to OpenRouter, polls
the returned `polling_url`, and downloads the completed video from
OpenRouter’s `unsigned_urls` or the documented job content endpoint.
Reference images are sent as first/last frame images by default; images
tagged with `reference_image` are sent as OpenRouter input references. The
bundled `google/veo-3.1-fast` default advertises the currently supported 4/6/8
second durations, `720P`/`1080P` resolutions, and `16:9`/`9:16` aspect
ratios. Video-to-video is not registered for OpenRouter because the upstream
video generation API currently accepts text and image references.

## [​](https://docs.openclaw.ai/providers/openrouter\#text-to-speech)  Text-to-speech

OpenRouter can also be used as a TTS provider through its OpenAI-compatible
`/audio/speech` endpoint.

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "openrouter",
      providers: {
        openrouter: {
          model: "hexgrad/kokoro-82m",
          voice: "af_alloy",
          responseFormat: "mp3",
        },
      },
    },
  },
}
```

If `messages.tts.providers.openrouter.apiKey` is omitted, TTS reuses
`models.providers.openrouter.apiKey`, then `OPENROUTER_API_KEY`.

## [​](https://docs.openclaw.ai/providers/openrouter\#authentication-and-headers)  Authentication and headers

OpenRouter uses a Bearer token with your API key under the hood.On real OpenRouter requests (`https://openrouter.ai/api/v1`), OpenClaw also adds
OpenRouter’s documented app-attribution headers:

| Header | Value |
| --- | --- |
| `HTTP-Referer` | `https://openclaw.ai` |
| `X-OpenRouter-Title` | `OpenClaw` |
| `X-OpenRouter-Categories` | `cli-agent` |

If you repoint the OpenRouter provider at some other proxy or base URL, OpenClaw
does **not** inject those OpenRouter-specific headers or Anthropic cache markers.

## [​](https://docs.openclaw.ai/providers/openrouter\#advanced-configuration)  Advanced configuration

Anthropic cache markers

On verified OpenRouter routes, Anthropic model refs keep the
OpenRouter-specific Anthropic `cache_control` markers that OpenClaw uses for
better prompt-cache reuse on system/developer prompt blocks.

Anthropic reasoning prefill

On verified OpenRouter routes, Anthropic model refs with reasoning enabled
drop trailing assistant prefill turns before the request reaches OpenRouter,
matching Anthropic’s requirement that reasoning conversations end with a user
turn.

Thinking / reasoning injection

On supported non-`auto` routes, OpenClaw maps the selected thinking level to
OpenRouter proxy reasoning payloads. Unsupported model hints and
`openrouter/auto` skip that reasoning injection. Hunter Alpha also skips
proxy reasoning for stale configured model refs because OpenRouter could
return final answer text in reasoning fields for that retired route.

DeepSeek V4 reasoning replay

On verified OpenRouter routes, `openrouter/deepseek/deepseek-v4-flash` and
`openrouter/deepseek/deepseek-v4-pro` fill missing `reasoning_content` on
replayed assistant turns so thinking/tool conversations keep DeepSeek V4’s
required follow-up shape.

OpenAI-only request shaping

OpenRouter still runs through the proxy-style OpenAI-compatible path, so
native OpenAI-only request shaping such as `serviceTier`, Responses `store`,
OpenAI reasoning-compat payloads, and prompt-cache hints is not forwarded.

Gemini-backed routes

Gemini-backed OpenRouter refs stay on the proxy-Gemini path: OpenClaw keeps
Gemini thought-signature sanitation there, but does not enable native Gemini
replay validation or bootstrap rewrites.

Provider routing metadata

If you pass OpenRouter provider routing under model params, OpenClaw forwards
it as OpenRouter routing metadata before the shared stream wrappers run.

## [​](https://docs.openclaw.ai/providers/openrouter\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config reference for agents, models, and providers.](https://docs.openclaw.ai/gateway/configuration-reference)

[OpenCode Go](https://docs.openclaw.ai/providers/opencode-go) [Perplexity](https://docs.openclaw.ai/providers/perplexity-provider)

Ctrl+I