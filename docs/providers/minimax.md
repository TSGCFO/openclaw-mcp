---
source_url: https://docs.openclaw.ai/providers/minimax
title: "MiniMax - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/minimax#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

MiniMax

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Built-in catalog](https://docs.openclaw.ai/providers/minimax#built-in-catalog)
- [Getting started](https://docs.openclaw.ai/providers/minimax#getting-started)
- [Configure via openclaw configure](https://docs.openclaw.ai/providers/minimax#configure-via-openclaw-configure)
- [Capabilities](https://docs.openclaw.ai/providers/minimax#capabilities)
- [Image generation](https://docs.openclaw.ai/providers/minimax#image-generation)
- [Text-to-speech](https://docs.openclaw.ai/providers/minimax#text-to-speech)
- [Music generation](https://docs.openclaw.ai/providers/minimax#music-generation)
- [Video generation](https://docs.openclaw.ai/providers/minimax#video-generation)
- [Image understanding](https://docs.openclaw.ai/providers/minimax#image-understanding)
- [Web search](https://docs.openclaw.ai/providers/minimax#web-search)
- [Advanced configuration](https://docs.openclaw.ai/providers/minimax#advanced-configuration)
- [Notes](https://docs.openclaw.ai/providers/minimax#notes)
- [Troubleshooting](https://docs.openclaw.ai/providers/minimax#troubleshooting)
- [Related](https://docs.openclaw.ai/providers/minimax#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw’s MiniMax provider defaults to **MiniMax M2.7**.MiniMax also provides:

- Bundled speech synthesis via T2A v2
- Bundled image understanding via `MiniMax-VL-01`
- Bundled music generation via `music-2.6`
- Bundled `web_search` through the MiniMax Token Plan search API

Provider split:

| Provider ID | Auth | Capabilities |
| --- | --- | --- |
| `minimax` | API key | Text, image generation, music generation, video generation, image understanding, speech, web search |
| `minimax-portal` | OAuth | Text, image generation, music generation, video generation, image understanding, speech |

## [​](https://docs.openclaw.ai/providers/minimax\#built-in-catalog)  Built-in catalog

| Model | Type | Description |
| --- | --- | --- |
| `MiniMax-M2.7` | Chat (reasoning) | Default hosted reasoning model |
| `MiniMax-M2.7-highspeed` | Chat (reasoning) | Faster M2.7 reasoning tier |
| `MiniMax-VL-01` | Vision | Image understanding model |
| `image-01` | Image generation | Text-to-image and image-to-image editing |
| `music-2.6` | Music generation | Default music model |
| `music-2.5` | Music generation | Previous music generation tier |
| `music-2.0` | Music generation | Legacy music generation tier |
| `MiniMax-Hailuo-2.3` | Video generation | Text-to-video and image reference flows |

## [​](https://docs.openclaw.ai/providers/minimax\#getting-started)  Getting started

Choose your preferred auth method and follow the setup steps.

- OAuth (Coding Plan)

- API key


**Best for:** quick setup with MiniMax Coding Plan via OAuth, no API key required.

- International

- China


1

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Run onboarding

```
openclaw onboard --auth-choice minimax-global-oauth
```

This authenticates against `api.minimax.io`.

2

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Verify the model is available

```
openclaw models list --provider minimax-portal
```

1

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Run onboarding

```
openclaw onboard --auth-choice minimax-cn-oauth
```

This authenticates against `api.minimaxi.com`.

2

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Verify the model is available

```
openclaw models list --provider minimax-portal
```

OAuth setups use the `minimax-portal` provider id. Model refs follow the form `minimax-portal/MiniMax-M2.7`.

Referral link for MiniMax Coding Plan (10% off): [MiniMax Coding Plan](https://platform.minimax.io/subscribe/coding-plan?code=DbXJTRClnb&source=link)

**Best for:** hosted MiniMax with Anthropic-compatible API.

- International

- China


1

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Run onboarding

```
openclaw onboard --auth-choice minimax-global-api
```

This configures `api.minimax.io` as the base URL.

2

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Verify the model is available

```
openclaw models list --provider minimax
```

1

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Run onboarding

```
openclaw onboard --auth-choice minimax-cn-api
```

This configures `api.minimaxi.com` as the base URL.

2

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Verify the model is available

```
openclaw models list --provider minimax
```

### [​](https://docs.openclaw.ai/providers/minimax\#config-example)  Config example

```
{
  env: { MINIMAX_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "minimax/MiniMax-M2.7" } } },
  models: {
    mode: "merge",
    providers: {
      minimax: {
        baseUrl: "https://api.minimax.io/anthropic",
        apiKey: "${MINIMAX_API_KEY}",
        api: "anthropic-messages",
        models: [\
          {\
            id: "MiniMax-M2.7",\
            name: "MiniMax M2.7",\
            reasoning: true,\
            input: ["text"],\
            cost: { input: 0.3, output: 1.2, cacheRead: 0.06, cacheWrite: 0.375 },\
            contextWindow: 204800,\
            maxTokens: 131072,\
          },\
          {\
            id: "MiniMax-M2.7-highspeed",\
            name: "MiniMax M2.7 Highspeed",\
            reasoning: true,\
            input: ["text"],\
            cost: { input: 0.6, output: 2.4, cacheRead: 0.06, cacheWrite: 0.375 },\
            contextWindow: 204800,\
            maxTokens: 131072,\
          },\
        ],
      },
    },
  },
}
```

On the Anthropic-compatible streaming path, OpenClaw disables MiniMax thinking by default unless you explicitly set `thinking` yourself. MiniMax’s streaming endpoint emits `reasoning_content` in OpenAI-style delta chunks instead of native Anthropic thinking blocks, which can leak internal reasoning into visible output if left enabled implicitly.

API-key setups use the `minimax` provider id. Model refs follow the form `minimax/MiniMax-M2.7`.

## [​](https://docs.openclaw.ai/providers/minimax\#configure-via-openclaw-configure)  Configure via `openclaw configure`

Use the interactive config wizard to set MiniMax without editing JSON:

1

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Launch the wizard

```
openclaw configure
```

2

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Select Model/auth

Choose **Model/auth** from the menu.

3

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Choose a MiniMax auth option

Pick one of the available MiniMax options:

| Auth choice | Description |
| --- | --- |
| `minimax-global-oauth` | International OAuth (Coding Plan) |
| `minimax-cn-oauth` | China OAuth (Coding Plan) |
| `minimax-global-api` | International API key |
| `minimax-cn-api` | China API key |

4

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Pick your default model

Select your default model when prompted.

## [​](https://docs.openclaw.ai/providers/minimax\#capabilities)  Capabilities

### [​](https://docs.openclaw.ai/providers/minimax\#image-generation)  Image generation

The MiniMax plugin registers the `image-01` model for the `image_generate` tool. It supports:

- **Text-to-image generation** with aspect ratio control
- **Image-to-image editing** (subject reference) with aspect ratio control
- Up to **9 output images** per request
- Up to **1 reference image** per edit request
- Supported aspect ratios: `1:1`, `16:9`, `4:3`, `3:2`, `2:3`, `3:4`, `9:16`, `21:9`

To use MiniMax for image generation, set it as the image generation provider:

```
{
  agents: {
    defaults: {
      imageGenerationModel: { primary: "minimax/image-01" },
    },
  },
}
```

The plugin uses the same `MINIMAX_API_KEY` or OAuth auth as the text models. No additional configuration is needed if MiniMax is already set up.Both `minimax` and `minimax-portal` register `image_generate` with the same
`image-01` model. API-key setups use `MINIMAX_API_KEY`; OAuth setups can use
the bundled `minimax-portal` auth path instead.Image generation always uses MiniMax’s dedicated image endpoint
(`/v1/image_generation`) and ignores `models.providers.minimax.baseUrl`,
since that field configures the chat/Anthropic-compatible base URL. Set
`MINIMAX_API_HOST=https://api.minimaxi.com` to route image generation
through the CN endpoint; the default global endpoint is
`https://api.minimax.io`.When onboarding or API-key setup writes explicit `models.providers.minimax`
entries, OpenClaw materializes `MiniMax-M2.7` and
`MiniMax-M2.7-highspeed` as text-only chat models. Image understanding is
exposed separately through the plugin-owned `MiniMax-VL-01` media provider.

See [Image Generation](https://docs.openclaw.ai/tools/image-generation) for shared tool parameters, provider selection, and failover behavior.

### [​](https://docs.openclaw.ai/providers/minimax\#text-to-speech)  Text-to-speech

The bundled `minimax` plugin registers MiniMax T2A v2 as a speech provider for
`messages.tts`.

- Default TTS model: `speech-2.8-hd`
- Default voice: `English_expressive_narrator`
- Supported bundled model ids include `speech-2.8-hd`, `speech-2.8-turbo`,
`speech-2.6-hd`, `speech-2.6-turbo`, `speech-02-hd`,
`speech-02-turbo`, `speech-01-hd`, and `speech-01-turbo`.
- Auth resolution is `messages.tts.providers.minimax.apiKey`, then
`minimax-portal` OAuth/token auth profiles, then Token Plan environment
keys (`MINIMAX_OAUTH_TOKEN`, `MINIMAX_CODE_PLAN_KEY`,
`MINIMAX_CODING_API_KEY`), then `MINIMAX_API_KEY`.
- If no TTS host is configured, OpenClaw reuses the configured
`minimax-portal` OAuth host and strips Anthropic-compatible path suffixes
such as `/anthropic`.
- Normal audio attachments stay MP3.
- Voice-note targets such as Feishu and Telegram are transcoded from MiniMax
MP3 to 48kHz Opus with `ffmpeg`, because the Feishu/Lark file API only
accepts `file_type: "opus"` for native audio messages.
- MiniMax T2A accepts fractional `speed` and `vol`, but `pitch` is sent as an
integer; OpenClaw truncates fractional `pitch` values before the API request.

| Setting | Env var | Default | Description |
| --- | --- | --- | --- |
| `messages.tts.providers.minimax.baseUrl` | `MINIMAX_API_HOST` | `https://api.minimax.io` | MiniMax T2A API host. |
| `messages.tts.providers.minimax.model` | `MINIMAX_TTS_MODEL` | `speech-2.8-hd` | TTS model id. |
| `messages.tts.providers.minimax.voiceId` | `MINIMAX_TTS_VOICE_ID` | `English_expressive_narrator` | Voice id used for speech output. |
| `messages.tts.providers.minimax.speed` |  | `1.0` | Playback speed, `0.5..2.0`. |
| `messages.tts.providers.minimax.vol` |  | `1.0` | Volume, `(0, 10]`. |
| `messages.tts.providers.minimax.pitch` |  | `0` | Integer pitch shift, `-12..12`. |

### [​](https://docs.openclaw.ai/providers/minimax\#music-generation)  Music generation

The bundled MiniMax plugin registers music generation through the shared
`music_generate` tool for both `minimax` and `minimax-portal`.

- Default music model: `minimax/music-2.6`
- OAuth music model: `minimax-portal/music-2.6`
- Also supports `minimax/music-2.5` and `minimax/music-2.0`
- Prompt controls: `lyrics`, `instrumental`, `durationSeconds`
- Output format: `mp3`
- Session-backed runs detach through the shared task/status flow, including `action: "status"`

To use MiniMax as the default music provider:

```
{
  agents: {
    defaults: {
      musicGenerationModel: {
        primary: "minimax/music-2.6",
      },
    },
  },
}
```

See [Music Generation](https://docs.openclaw.ai/tools/music-generation) for shared tool parameters, provider selection, and failover behavior.

### [​](https://docs.openclaw.ai/providers/minimax\#video-generation)  Video generation

The bundled MiniMax plugin registers video generation through the shared
`video_generate` tool for both `minimax` and `minimax-portal`.

- Default video model: `minimax/MiniMax-Hailuo-2.3`
- OAuth video model: `minimax-portal/MiniMax-Hailuo-2.3`
- Modes: text-to-video and single-image reference flows
- Supports `aspectRatio` and `resolution`

To use MiniMax as the default video provider:

```
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "minimax/MiniMax-Hailuo-2.3",
      },
    },
  },
}
```

See [Video Generation](https://docs.openclaw.ai/tools/video-generation) for shared tool parameters, provider selection, and failover behavior.

### [​](https://docs.openclaw.ai/providers/minimax\#image-understanding)  Image understanding

The MiniMax plugin registers image understanding separately from the text
catalog:

| Provider ID | Default image model |
| --- | --- |
| `minimax` | `MiniMax-VL-01` |
| `minimax-portal` | `MiniMax-VL-01` |

That is why automatic media routing can use MiniMax image understanding even
when the bundled text-provider catalog still shows text-only M2.7 chat refs.

### [​](https://docs.openclaw.ai/providers/minimax\#web-search)  Web search

The MiniMax plugin also registers `web_search` through the MiniMax Token Plan
search API.

- Provider id: `minimax`
- Structured results: titles, URLs, snippets, related queries
- Preferred env var: `MINIMAX_CODE_PLAN_KEY`
- Accepted env aliases: `MINIMAX_CODING_API_KEY`, `MINIMAX_OAUTH_TOKEN`
- Compatibility fallback: `MINIMAX_API_KEY` when it already points at a token-plan credential
- Region reuse: `plugins.entries.minimax.config.webSearch.region`, then `MINIMAX_API_HOST`, then MiniMax provider base URLs
- Search stays on provider id `minimax`; OAuth CN/global setup can steer region indirectly through `models.providers.minimax-portal.baseUrl` and can provide bearer auth through `MINIMAX_OAUTH_TOKEN`

Config lives under `plugins.entries.minimax.config.webSearch.*`.

See [MiniMax Search](https://docs.openclaw.ai/tools/minimax-search) for full web search configuration and usage.

## [​](https://docs.openclaw.ai/providers/minimax\#advanced-configuration)  Advanced configuration

Configuration options

| Option | Description |
| --- | --- |
| `models.providers.minimax.baseUrl` | Prefer `https://api.minimax.io/anthropic` (Anthropic-compatible); `https://api.minimax.io/v1` is optional for OpenAI-compatible payloads |
| `models.providers.minimax.api` | Prefer `anthropic-messages`; `openai-completions` is optional for OpenAI-compatible payloads |
| `models.providers.minimax.apiKey` | MiniMax API key (`MINIMAX_API_KEY`) |
| `models.providers.minimax.models` | Define `id`, `name`, `reasoning`, `contextWindow`, `maxTokens`, `cost` |
| `agents.defaults.models` | Alias models you want in the allowlist |
| `models.mode` | Keep `merge` if you want to add MiniMax alongside built-ins |

Thinking defaults

On `api: "anthropic-messages"`, OpenClaw injects `thinking: { type: "disabled" }` unless thinking is already explicitly set in params/config.This prevents MiniMax’s streaming endpoint from emitting `reasoning_content` in OpenAI-style delta chunks, which would leak internal reasoning into visible output.

Fast mode

`/fast on` or `params.fastMode: true` rewrites `MiniMax-M2.7` to `MiniMax-M2.7-highspeed` on the Anthropic-compatible stream path.

Fallback example

**Best for:** keep your strongest latest-generation model as primary, fail over to MiniMax M2.7. Example below uses Opus as a concrete primary; swap to your preferred latest-gen primary model.

```
{
  env: { MINIMAX_API_KEY: "sk-..." },
  agents: {
    defaults: {
      models: {
        "anthropic/claude-opus-4-6": { alias: "primary" },
        "minimax/MiniMax-M2.7": { alias: "minimax" },
      },
      model: {
        primary: "anthropic/claude-opus-4-6",
        fallbacks: ["minimax/MiniMax-M2.7"],
      },
    },
  },
}
```

Coding Plan usage details

- Coding Plan usage API: `https://api.minimaxi.com/v1/token_plan/remains` or `https://api.minimax.io/v1/token_plan/remains` (requires a coding plan key).
- Usage polling derives the host from `models.providers.minimax-portal.baseUrl` or `models.providers.minimax.baseUrl` when configured, so global setups using `https://api.minimax.io/anthropic` poll `api.minimax.io`. Missing or malformed base URLs keep the CN fallback for compatibility.
- OpenClaw normalizes MiniMax coding-plan usage to the same `% left` display used by other providers. MiniMax’s raw `usage_percent` / `usagePercent` fields are remaining quota, not consumed quota, so OpenClaw inverts them. Count-based fields win when present.
- When the API returns `model_remains`, OpenClaw prefers the chat-model entry, derives the window label from `start_time` / `end_time` when needed, and includes the selected model name in the plan label so coding-plan windows are easier to distinguish.
- Usage snapshots treat `minimax`, `minimax-cn`, and `minimax-portal` as the same MiniMax quota surface, and prefer stored MiniMax OAuth before falling back to Coding Plan key env vars.

## [​](https://docs.openclaw.ai/providers/minimax\#notes)  Notes

- Model refs follow the auth path:
  - API-key setup: `minimax/<model>`
  - OAuth setup: `minimax-portal/<model>`
- Default chat model: `MiniMax-M2.7`
- Alternate chat model: `MiniMax-M2.7-highspeed`
- Onboarding and direct API-key setup write text-only model definitions for both M2.7 variants
- Image understanding uses the plugin-owned `MiniMax-VL-01` media provider
- Update pricing values in `models.json` if you need exact cost tracking
- Use `openclaw models list` to confirm the current provider id, then switch with `openclaw models set minimax/MiniMax-M2.7` or `openclaw models set minimax-portal/MiniMax-M2.7`

Referral link for MiniMax Coding Plan (10% off): [MiniMax Coding Plan](https://platform.minimax.io/subscribe/coding-plan?code=DbXJTRClnb&source=link)

See [Model providers](https://docs.openclaw.ai/concepts/model-providers) for provider rules.

## [​](https://docs.openclaw.ai/providers/minimax\#troubleshooting)  Troubleshooting

"Unknown model: minimax/MiniMax-M2.7"

This usually means the **MiniMax provider is not configured** (no matching provider entry and no MiniMax auth profile/env key found). A fix for this detection is in **2026.1.12**. Fix by:

- Upgrading to **2026.1.12** (or run from source `main`), then restarting the gateway.
- Running `openclaw configure` and selecting a **MiniMax** auth option, or
- Adding the matching `models.providers.minimax` or `models.providers.minimax-portal` block manually, or
- Setting `MINIMAX_API_KEY`, `MINIMAX_OAUTH_TOKEN`, or a MiniMax auth profile so the matching provider can be injected.

Make sure the model id is **case-sensitive**:

- API-key path: `minimax/MiniMax-M2.7` or `minimax/MiniMax-M2.7-highspeed`
- OAuth path: `minimax-portal/MiniMax-M2.7` or `minimax-portal/MiniMax-M2.7-highspeed`

Then recheck with:

```
openclaw models list
```

More help: [Troubleshooting](https://docs.openclaw.ai/help/troubleshooting) and [FAQ](https://docs.openclaw.ai/help/faq).

## [​](https://docs.openclaw.ai/providers/minimax\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Image generation** \\
\\
Shared image tool parameters and provider selection.](https://docs.openclaw.ai/tools/image-generation)

[**Music generation** \\
\\
Shared music tool parameters and provider selection.](https://docs.openclaw.ai/tools/music-generation)

[**Video generation** \\
\\
Shared video tool parameters and provider selection.](https://docs.openclaw.ai/tools/video-generation)

[**MiniMax Search** \\
\\
Web search configuration via MiniMax Token Plan.](https://docs.openclaw.ai/tools/minimax-search)

[**Troubleshooting** \\
\\
General troubleshooting and FAQ.](https://docs.openclaw.ai/help/troubleshooting)

[LM Studio](https://docs.openclaw.ai/providers/lmstudio) [Mistral](https://docs.openclaw.ai/providers/mistral)

Ctrl+I