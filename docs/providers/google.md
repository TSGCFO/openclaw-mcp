---
source_url: https://docs.openclaw.ai/providers/google
title: "Google (Gemini) - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/google#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Google (Gemini)

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/google#getting-started)
- [Capabilities](https://docs.openclaw.ai/providers/google#capabilities)
- [Web search](https://docs.openclaw.ai/providers/google#web-search)
- [Image generation](https://docs.openclaw.ai/providers/google#image-generation)
- [Video generation](https://docs.openclaw.ai/providers/google#video-generation)
- [Music generation](https://docs.openclaw.ai/providers/google#music-generation)
- [Text-to-speech](https://docs.openclaw.ai/providers/google#text-to-speech)
- [Realtime voice](https://docs.openclaw.ai/providers/google#realtime-voice)
- [Advanced configuration](https://docs.openclaw.ai/providers/google#advanced-configuration)
- [Related](https://docs.openclaw.ai/providers/google#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

The Google plugin provides access to Gemini models through Google AI Studio, plus
image generation, media understanding (image/audio/video), text-to-speech, and web search via
Gemini Grounding.

- Provider: `google`
- Auth: `GEMINI_API_KEY` or `GOOGLE_API_KEY`
- API: Google Gemini API
- Runtime option: `agents.defaults.agentRuntime.id: "google-gemini-cli"`
reuses Gemini CLI OAuth while keeping model refs canonical as `google/*`.

## [​](https://docs.openclaw.ai/providers/google\#getting-started)  Getting started

Choose your preferred auth method and follow the setup steps.

- API key

- Gemini CLI (OAuth)


**Best for:** standard Gemini API access through Google AI Studio.

1

[Navigate to header](https://docs.openclaw.ai/providers/google#)

Run onboarding

```
openclaw onboard --auth-choice gemini-api-key
```

Or pass the key directly:

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice gemini-api-key \
  --gemini-api-key "$GEMINI_API_KEY"
```

2

[Navigate to header](https://docs.openclaw.ai/providers/google#)

Set a default model

```
{
  agents: {
    defaults: {
      model: { primary: "google/gemini-3.1-pro-preview" },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/google#)

Verify the model is available

```
openclaw models list --provider google
```

The environment variables `GEMINI_API_KEY` and `GOOGLE_API_KEY` are both accepted. Use whichever you already have configured.

**Best for:** reusing an existing Gemini CLI login via PKCE OAuth instead of a separate API key.

The `google-gemini-cli` provider is an unofficial integration. Some users
report account restrictions when using OAuth this way. Use at your own risk.

1

[Navigate to header](https://docs.openclaw.ai/providers/google#)

Install the Gemini CLI

The local `gemini` command must be available on `PATH`.

```
# Homebrew
brew install gemini-cli

# or npm
npm install -g @google/gemini-cli
```

OpenClaw supports both Homebrew installs and global npm installs, including
common Windows/npm layouts.

2

[Navigate to header](https://docs.openclaw.ai/providers/google#)

Log in via OAuth

```
openclaw models auth login --provider google-gemini-cli --set-default
```

3

[Navigate to header](https://docs.openclaw.ai/providers/google#)

Verify the model is available

```
openclaw models list --provider google
```

- Default model: `google/gemini-3.1-pro-preview`
- Runtime: `google-gemini-cli`
- Alias: `gemini-cli`

Gemini 3.1 Pro’s Gemini API model id is `gemini-3.1-pro-preview`. OpenClaw accepts the shorter `google/gemini-3.1-pro` as a convenience alias and normalizes it before provider calls.**Environment variables:**

- `OPENCLAW_GEMINI_OAUTH_CLIENT_ID`
- `OPENCLAW_GEMINI_OAUTH_CLIENT_SECRET`

(Or the `GEMINI_CLI_*` variants.)

If Gemini CLI OAuth requests fail after login, set `GOOGLE_CLOUD_PROJECT` or
`GOOGLE_CLOUD_PROJECT_ID` on the gateway host and retry.

If login fails before the browser flow starts, make sure the local `gemini`
command is installed and on `PATH`.

`google-gemini-cli/*` model refs are legacy compatibility aliases. New
configs should use `google/*` model refs plus the `google-gemini-cli`
runtime when they want local Gemini CLI execution.

## [​](https://docs.openclaw.ai/providers/google\#capabilities)  Capabilities

| Capability | Supported |
| --- | --- |
| Chat completions | Yes |
| Image generation | Yes |
| Music generation | Yes |
| Text-to-speech | Yes |
| Realtime voice | Yes (Google Live API) |
| Image understanding | Yes |
| Audio transcription | Yes |
| Video understanding | Yes |
| Web search (Grounding) | Yes |
| Thinking/reasoning | Yes (Gemini 2.5+ / Gemini 3+) |
| Gemma 4 models | Yes |

## [​](https://docs.openclaw.ai/providers/google\#web-search)  Web search

The bundled `gemini` web-search provider uses Gemini Google Search grounding.
Configure a dedicated search key under `plugins.entries.google.config.webSearch`,
or let it reuse `models.providers.google.apiKey` after `GEMINI_API_KEY`:

```
{
  plugins: {
    entries: {
      google: {
        config: {
          webSearch: {
            apiKey: "AIza...", // optional if GEMINI_API_KEY or models.providers.google.apiKey is set
            baseUrl: "https://generativelanguage.googleapis.com/v1beta", // falls back to models.providers.google.baseUrl
            model: "gemini-2.5-flash",
          },
        },
      },
    },
  },
}
```

Credential precedence is dedicated `webSearch.apiKey`, then `GEMINI_API_KEY`,
then `models.providers.google.apiKey`. `webSearch.baseUrl` is optional and
exists for operator proxies or compatible Gemini API endpoints; when omitted,
Gemini web search reuses `models.providers.google.baseUrl`. See
[Gemini search](https://docs.openclaw.ai/tools/gemini-search) for the provider-specific tool behavior.

Gemini 3 models use `thinkingLevel` rather than `thinkingBudget`. OpenClaw maps
Gemini 3, Gemini 3.1, and `gemini-*-latest` alias reasoning controls to
`thinkingLevel` so default/low-latency runs do not send disabled
`thinkingBudget` values.`/think adaptive` keeps Google’s dynamic thinking semantics instead of choosing
a fixed OpenClaw level. Gemini 3 and Gemini 3.1 omit a fixed `thinkingLevel` so
Google can choose the level; Gemini 2.5 sends Google’s dynamic sentinel
`thinkingBudget: -1`.Gemma 4 models (for example `gemma-4-26b-a4b-it`) support thinking mode. OpenClaw
rewrites `thinkingBudget` to a supported Google `thinkingLevel` for Gemma 4.
Setting thinking to `off` preserves thinking disabled instead of mapping to
`MINIMAL`.

## [​](https://docs.openclaw.ai/providers/google\#image-generation)  Image generation

The bundled `google` image-generation provider defaults to
`google/gemini-3.1-flash-image-preview`.

- Also supports `google/gemini-3-pro-image-preview`
- Generate: up to 4 images per request
- Edit mode: enabled, up to 5 input images
- Geometry controls: `size`, `aspectRatio`, and `resolution`

To use Google as the default image provider:

```
{
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "google/gemini-3.1-flash-image-preview",
      },
    },
  },
}
```

See [Image Generation](https://docs.openclaw.ai/tools/image-generation) for shared tool parameters, provider selection, and failover behavior.

## [​](https://docs.openclaw.ai/providers/google\#video-generation)  Video generation

The bundled `google` plugin also registers video generation through the shared
`video_generate` tool.

- Default video model: `google/veo-3.1-fast-generate-preview`
- Modes: text-to-video, image-to-video, and single-video reference flows
- Supports `aspectRatio`, `resolution`, and `audio`
- Current duration clamp: **4 to 8 seconds**

To use Google as the default video provider:

```
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "google/veo-3.1-fast-generate-preview",
      },
    },
  },
}
```

See [Video Generation](https://docs.openclaw.ai/tools/video-generation) for shared tool parameters, provider selection, and failover behavior.

## [​](https://docs.openclaw.ai/providers/google\#music-generation)  Music generation

The bundled `google` plugin also registers music generation through the shared
`music_generate` tool.

- Default music model: `google/lyria-3-clip-preview`
- Also supports `google/lyria-3-pro-preview`
- Prompt controls: `lyrics` and `instrumental`
- Output format: `mp3` by default, plus `wav` on `google/lyria-3-pro-preview`
- Reference inputs: up to 10 images
- Session-backed runs detach through the shared task/status flow, including `action: "status"`

To use Google as the default music provider:

```
{
  agents: {
    defaults: {
      musicGenerationModel: {
        primary: "google/lyria-3-clip-preview",
      },
    },
  },
}
```

See [Music Generation](https://docs.openclaw.ai/tools/music-generation) for shared tool parameters, provider selection, and failover behavior.

## [​](https://docs.openclaw.ai/providers/google\#text-to-speech)  Text-to-speech

The bundled `google` speech provider uses the Gemini API TTS path with
`gemini-3.1-flash-tts-preview`.

- Default voice: `Kore`
- Auth: `messages.tts.providers.google.apiKey`, `models.providers.google.apiKey`, `GEMINI_API_KEY`, or `GOOGLE_API_KEY`
- Output: WAV for regular TTS attachments, Opus for voice-note targets, PCM for Talk/telephony
- Voice-note output: Google PCM is wrapped as WAV and transcoded to 48 kHz Opus with `ffmpeg`

To use Google as the default TTS provider:

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "google",
      providers: {
        google: {
          model: "gemini-3.1-flash-tts-preview",
          voiceName: "Kore",
          audioProfile: "Speak professionally with a calm tone.",
        },
      },
    },
  },
}
```

Gemini API TTS uses natural-language prompting for style control. Set
`audioProfile` to prepend a reusable style prompt before the spoken text. Set
`speakerName` when your prompt text refers to a named speaker.Gemini API TTS also accepts expressive square-bracket audio tags in the text,
such as `[whispers]` or `[laughs]`. To keep tags out of the visible chat reply
while sending them to TTS, put them inside a `[[tts:text]]...[[/tts:text]]`
block:

```
Here is the clean reply text.

[[tts:text]][whispers] Here is the spoken version.[[/tts:text]]
```

A Google Cloud Console API key restricted to the Gemini API is valid for this
provider. This is not the separate Cloud Text-to-Speech API path.

## [​](https://docs.openclaw.ai/providers/google\#realtime-voice)  Realtime voice

The bundled `google` plugin registers a realtime voice provider backed by the
Gemini Live API for backend audio bridges such as Voice Call and Google Meet.

| Setting | Config path | Default |
| --- | --- | --- |
| Model | `plugins.entries.voice-call.config.realtime.providers.google.model` | `gemini-2.5-flash-native-audio-preview-12-2025` |
| Voice | `...google.voice` | `Kore` |
| Temperature | `...google.temperature` | (unset) |
| VAD start sensitivity | `...google.startSensitivity` | (unset) |
| VAD end sensitivity | `...google.endSensitivity` | (unset) |
| Silence duration | `...google.silenceDurationMs` | (unset) |
| Activity handling | `...google.activityHandling` | Google default, `start-of-activity-interrupts` |
| Turn coverage | `...google.turnCoverage` | Google default, `only-activity` |
| Disable auto VAD | `...google.automaticActivityDetectionDisabled` | `false` |
| API key | `...google.apiKey` | Falls back to `models.providers.google.apiKey`, `GEMINI_API_KEY`, or `GOOGLE_API_KEY` |

Example Voice Call realtime config:

```
{
  plugins: {
    entries: {
      "voice-call": {
        enabled: true,
        config: {
          realtime: {
            enabled: true,
            provider: "google",
            providers: {
              google: {
                model: "gemini-2.5-flash-native-audio-preview-12-2025",
                voice: "Kore",
                activityHandling: "start-of-activity-interrupts",
                turnCoverage: "only-activity",
              },
            },
          },
        },
      },
    },
  },
}
```

Google Live API uses bidirectional audio and function calling over a WebSocket.
OpenClaw adapts telephony/Meet bridge audio to Gemini’s PCM Live API stream and
keeps tool calls on the shared realtime voice contract. Leave `temperature`
unset unless you need sampling changes; OpenClaw omits non-positive values
because Google Live can return transcripts without audio for `temperature: 0`.
Gemini API transcription is enabled without `languageCodes`; the current Google
SDK rejects language-code hints on this API path.

Control UI Talk supports Google Live browser sessions with constrained one-use
tokens. Backend-only realtime voice providers can also run through the generic
Gateway relay transport, which keeps provider credentials on the Gateway.

For maintainer live verification, run
`OPENAI_API_KEY=... GEMINI_API_KEY=... node --import tsx scripts/dev/realtime-talk-live-smoke.ts`.
The Google leg mints the same constrained Live API token shape used by Control
UI Talk, opens the browser WebSocket endpoint, sends the initial setup payload,
and waits for `setupComplete`.

## [​](https://docs.openclaw.ai/providers/google\#advanced-configuration)  Advanced configuration

Direct Gemini cache reuse

For direct Gemini API runs (`api: "google-generative-ai"`), OpenClaw
passes a configured `cachedContent` handle through to Gemini requests.

- Configure per-model or global params with either
`cachedContent` or legacy `cached_content`
- If both are present, `cachedContent` wins
- Example value: `cachedContents/prebuilt-context`
- Gemini cache-hit usage is normalized into OpenClaw `cacheRead` from
upstream `cachedContentTokenCount`

```
{
  agents: {
    defaults: {
      models: {
        "google/gemini-2.5-pro": {
          params: {
            cachedContent: "cachedContents/prebuilt-context",
          },
        },
      },
    },
  },
}
```

Gemini CLI JSON usage notes

When using the `google-gemini-cli` OAuth provider, OpenClaw normalizes
the CLI JSON output as follows:

- Reply text comes from the CLI JSON `response` field.
- Usage falls back to `stats` when the CLI leaves `usage` empty.
- `stats.cached` is normalized into OpenClaw `cacheRead`.
- If `stats.input` is missing, OpenClaw derives input tokens from
`stats.input_tokens - stats.cached`.

Environment and daemon setup

If the Gateway runs as a daemon (launchd/systemd), make sure `GEMINI_API_KEY`
is available to that process (for example, in `~/.openclaw/.env` or via
`env.shellEnv`).

## [​](https://docs.openclaw.ai/providers/google\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Image generation** \\
\\
Shared image tool parameters and provider selection.](https://docs.openclaw.ai/tools/image-generation)

[**Video generation** \\
\\
Shared video tool parameters and provider selection.](https://docs.openclaw.ai/tools/video-generation)

[**Music generation** \\
\\
Shared music tool parameters and provider selection.](https://docs.openclaw.ai/tools/music-generation)

[GLM (Zhipu)](https://docs.openclaw.ai/providers/glm) [Gradium](https://docs.openclaw.ai/providers/gradium)

Ctrl+I