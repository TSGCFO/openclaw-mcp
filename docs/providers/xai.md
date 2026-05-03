---
source_url: https://docs.openclaw.ai/providers/xai
title: "xAI - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/xai#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

xAI

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/xai#getting-started)
- [Built-in catalog](https://docs.openclaw.ai/providers/xai#built-in-catalog)
- [OpenClaw feature coverage](https://docs.openclaw.ai/providers/xai#openclaw-feature-coverage)
- [Fast-mode mappings](https://docs.openclaw.ai/providers/xai#fast-mode-mappings)
- [Legacy compatibility aliases](https://docs.openclaw.ai/providers/xai#legacy-compatibility-aliases)
- [Features](https://docs.openclaw.ai/providers/xai#features)
- [Live testing](https://docs.openclaw.ai/providers/xai#live-testing)
- [Related](https://docs.openclaw.ai/providers/xai#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw ships a bundled `xai` provider plugin for Grok models.

## [​](https://docs.openclaw.ai/providers/xai\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/xai#)

Create an API key

Create an API key in the [xAI console](https://console.x.ai/).

2

[Navigate to header](https://docs.openclaw.ai/providers/xai#)

Set your API key

Set `XAI_API_KEY`, or run:

```
openclaw onboard --auth-choice xai-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/xai#)

Pick a model

```
{
  agents: { defaults: { model: { primary: "xai/grok-4.3" } } },
}
```

OpenClaw uses the xAI Responses API as the bundled xAI transport. The same
`XAI_API_KEY` can also power Grok-backed `web_search`, first-class `x_search`,
and remote `code_execution`.
If you store an xAI key under `plugins.entries.xai.config.webSearch.apiKey`,
the bundled xAI model provider reuses that key as a fallback too.
Set `plugins.entries.xai.config.webSearch.baseUrl` to route Grok `web_search`
and, by default, `x_search` through an operator xAI Responses proxy.
`code_execution` tuning lives under `plugins.entries.xai.config.codeExecution`.

## [​](https://docs.openclaw.ai/providers/xai\#built-in-catalog)  Built-in catalog

OpenClaw includes these xAI model families out of the box:

| Family | Model ids |
| --- | --- |
| Grok 3 | `grok-3`, `grok-3-fast`, `grok-3-mini`, `grok-3-mini-fast` |
| Grok 4.3 | `grok-4.3` |
| Grok 4 | `grok-4`, `grok-4-0709` |
| Grok 4 Fast | `grok-4-fast`, `grok-4-fast-non-reasoning` |
| Grok 4.1 Fast | `grok-4-1-fast`, `grok-4-1-fast-non-reasoning` |
| Grok 4.20 Beta | `grok-4.20-beta-latest-reasoning`, `grok-4.20-beta-latest-non-reasoning` |
| Grok Code | `grok-code-fast-1` |

The plugin also forward-resolves newer `grok-4*` and `grok-code-fast*` ids when
they follow the same API shape.

`grok-4.3`, `grok-4-fast`, `grok-4-1-fast`, and the `grok-4.20-beta-*`
variants are the current image-capable Grok refs in the bundled catalog.

## [​](https://docs.openclaw.ai/providers/xai\#openclaw-feature-coverage)  OpenClaw feature coverage

The bundled plugin maps xAI’s current public API surface onto OpenClaw’s shared
provider and tool contracts. Capabilities that don’t fit the shared contract
(for example streaming TTS and realtime voice) are not exposed — see the table
below.

| xAI capability | OpenClaw surface | Status |
| --- | --- | --- |
| Chat / Responses | `xai/<model>` model provider | Yes |
| Server-side web search | `web_search` provider `grok` | Yes |
| Server-side X search | `x_search` tool | Yes |
| Server-side code execution | `code_execution` tool | Yes |
| Images | `image_generate` | Yes |
| Videos | `video_generate` | Yes |
| Batch text-to-speech | `messages.tts.provider: "xai"` / `tts` | Yes |
| Streaming TTS | — | Not exposed; OpenClaw’s TTS contract returns complete audio buffers |
| Batch speech-to-text | `tools.media.audio` / media understanding | Yes |
| Streaming speech-to-text | Voice Call `streaming.provider: "xai"` | Yes |
| Realtime voice | — | Not exposed yet; different session/WebSocket contract |
| Files / batches | Generic model API compatibility only | Not a first-class OpenClaw tool |

OpenClaw uses xAI’s REST image/video/TTS/STT APIs for media generation,
speech, and batch transcription, xAI’s streaming STT WebSocket for live
voice-call transcription, and the Responses API for model, search, and
code-execution tools. Features that need different OpenClaw contracts, such as
Realtime voice sessions, are documented here as upstream capabilities rather
than hidden plugin behavior.

### [​](https://docs.openclaw.ai/providers/xai\#fast-mode-mappings)  Fast-mode mappings

`/fast on` or `agents.defaults.models["xai/<model>"].params.fastMode: true`
rewrites native xAI requests as follows:

| Source model | Fast-mode target |
| --- | --- |
| `grok-3` | `grok-3-fast` |
| `grok-3-mini` | `grok-3-mini-fast` |
| `grok-4` | `grok-4-fast` |
| `grok-4-0709` | `grok-4-fast` |

### [​](https://docs.openclaw.ai/providers/xai\#legacy-compatibility-aliases)  Legacy compatibility aliases

Legacy aliases still normalize to the canonical bundled ids:

| Legacy alias | Canonical id |
| --- | --- |
| `grok-4-fast-reasoning` | `grok-4-fast` |
| `grok-4-1-fast-reasoning` | `grok-4-1-fast` |
| `grok-4.20-reasoning` | `grok-4.20-beta-latest-reasoning` |
| `grok-4.20-non-reasoning` | `grok-4.20-beta-latest-non-reasoning` |

## [​](https://docs.openclaw.ai/providers/xai\#features)  Features

Web search

The bundled `grok` web-search provider uses `XAI_API_KEY` too:

```
openclaw config set tools.web.search.provider grok
```

Video generation

The bundled `xai` plugin registers video generation through the shared
`video_generate` tool.

- Default video model: `xai/grok-imagine-video`
- Modes: text-to-video, image-to-video, reference-image generation, remote
video edit, and remote video extension
- Aspect ratios: `1:1`, `16:9`, `9:16`, `4:3`, `3:4`, `3:2`, `2:3`
- Resolutions: `480P`, `720P`
- Duration: 1-15 seconds for generation/image-to-video, 1-10 seconds when
using `reference_image` roles, 2-10 seconds for extension
- Reference-image generation: set `imageRoles` to `reference_image` for
every supplied image; xAI accepts up to 7 such images

Local video buffers are not accepted. Use remote `http(s)` URLs for
video edit/extend inputs. Image-to-video accepts local image buffers because
OpenClaw can encode those as data URLs for xAI.

To use xAI as the default video provider:

```
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "xai/grok-imagine-video",
      },
    },
  },
}
```

See [Video Generation](https://docs.openclaw.ai/tools/video-generation) for shared tool parameters,
provider selection, and failover behavior.

Image generation

The bundled `xai` plugin registers image generation through the shared
`image_generate` tool.

- Default image model: `xai/grok-imagine-image`
- Additional model: `xai/grok-imagine-image-pro`
- Modes: text-to-image and reference-image edit
- Reference inputs: one `image` or up to five `images`
- Aspect ratios: `1:1`, `16:9`, `9:16`, `4:3`, `3:4`, `2:3`, `3:2`
- Resolutions: `1K`, `2K`
- Count: up to 4 images

OpenClaw asks xAI for `b64_json` image responses so generated media can be
stored and delivered through the normal channel attachment path. Local
reference images are converted to data URLs; remote `http(s)` references are
passed through.To use xAI as the default image provider:

```
{
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "xai/grok-imagine-image",
      },
    },
  },
}
```

xAI also documents `quality`, `mask`, `user`, and additional native ratios
such as `1:2`, `2:1`, `9:20`, and `20:9`. OpenClaw forwards only the
shared cross-provider image controls today; unsupported native-only knobs
are intentionally not exposed through `image_generate`.

Text-to-speech

The bundled `xai` plugin registers text-to-speech through the shared `tts`
provider surface.

- Voices: `eve`, `ara`, `rex`, `sal`, `leo`, `una`
- Default voice: `eve`
- Formats: `mp3`, `wav`, `pcm`, `mulaw`, `alaw`
- Language: BCP-47 code or `auto`
- Speed: provider-native speed override
- Native Opus voice-note format is not supported

To use xAI as the default TTS provider:

```
{
  messages: {
    tts: {
      provider: "xai",
      providers: {
        xai: {
          voiceId: "eve",
        },
      },
    },
  },
}
```

OpenClaw uses xAI’s batch `/v1/tts` endpoint. xAI also offers streaming TTS
over WebSocket, but the OpenClaw speech provider contract currently expects
a complete audio buffer before reply delivery.

Speech-to-text

The bundled `xai` plugin registers batch speech-to-text through OpenClaw’s
media-understanding transcription surface.

- Default model: `grok-stt`
- Endpoint: xAI REST `/v1/stt`
- Input path: multipart audio file upload
- Supported by OpenClaw wherever inbound audio transcription uses
`tools.media.audio`, including Discord voice-channel segments and
channel audio attachments

To force xAI for inbound audio transcription:

```
{
  tools: {
    media: {
      audio: {
        models: [\
          {\
            type: "provider",\
            provider: "xai",\
            model: "grok-stt",\
          },\
        ],
      },
    },
  },
}
```

Language can be supplied through the shared audio media config or per-call
transcription request. Prompt hints are accepted by the shared OpenClaw
surface, but the xAI REST STT integration only forwards file, model, and
language because those map cleanly to the current public xAI endpoint.

Streaming speech-to-text

The bundled `xai` plugin also registers a realtime transcription provider
for live voice-call audio.

- Endpoint: xAI WebSocket `wss://api.x.ai/v1/stt`
- Default encoding: `mulaw`
- Default sample rate: `8000`
- Default endpointing: `800ms`
- Interim transcripts: enabled by default

Voice Call’s Twilio media stream sends G.711 µ-law audio frames, so the
xAI provider can forward those frames directly without transcoding:

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          streaming: {
            enabled: true,
            provider: "xai",
            providers: {
              xai: {
                apiKey: "${XAI_API_KEY}",
                endpointingMs: 800,
                language: "en",
              },
            },
          },
        },
      },
    },
  },
}
```

Provider-owned config lives under
`plugins.entries.voice-call.config.streaming.providers.xai`. Supported
keys are `apiKey`, `baseUrl`, `sampleRate`, `encoding` (`pcm`, `mulaw`, or
`alaw`), `interimResults`, `endpointingMs`, and `language`.

This streaming provider is for Voice Call’s realtime transcription path.
Discord voice currently records short segments and uses the batch
`tools.media.audio` transcription path instead.

x\_search configuration

The bundled xAI plugin exposes `x_search` as an OpenClaw tool for searching
X (formerly Twitter) content via Grok.Config path: `plugins.entries.xai.config.xSearch`

| Key | Type | Default | Description |
| --- | --- | --- | --- |
| `enabled` | boolean | — | Enable or disable x\_search |
| `model` | string | `grok-4-1-fast` | Model used for x\_search requests |
| `baseUrl` | string | — | xAI Responses base URL override |
| `inlineCitations` | boolean | — | Include inline citations in results |
| `maxTurns` | number | — | Maximum conversation turns |
| `timeoutSeconds` | number | — | Request timeout in seconds |
| `cacheTtlMinutes` | number | — | Cache time-to-live in minutes |

```
{
  plugins: {
    entries: {
      xai: {
        config: {
          xSearch: {
            enabled: true,
            model: "grok-4-1-fast",
            baseUrl: "https://api.x.ai/v1",
            inlineCitations: true,
          },
        },
      },
    },
  },
}
```

Code execution configuration

The bundled xAI plugin exposes `code_execution` as an OpenClaw tool for
remote code execution in xAI’s sandbox environment.Config path: `plugins.entries.xai.config.codeExecution`

| Key | Type | Default | Description |
| --- | --- | --- | --- |
| `enabled` | boolean | `true` (if key available) | Enable or disable code execution |
| `model` | string | `grok-4-1-fast` | Model used for code execution requests |
| `maxTurns` | number | — | Maximum conversation turns |
| `timeoutSeconds` | number | — | Request timeout in seconds |

This is remote xAI sandbox execution, not local [`exec`](https://docs.openclaw.ai/tools/exec).

```
{
  plugins: {
    entries: {
      xai: {
        config: {
          codeExecution: {
            enabled: true,
            model: "grok-4-1-fast",
          },
        },
      },
    },
  },
}
```

Known limits

- Auth is API-key only today. There is no xAI OAuth or device-code flow in
OpenClaw yet.
- `grok-4.20-multi-agent-experimental-beta-0304` is not supported on the
normal xAI provider path because it requires a different upstream API
surface than the standard OpenClaw xAI transport.
- xAI Realtime voice is not registered as an OpenClaw provider yet. It
needs a different bidirectional voice session contract than batch STT or
streaming transcription.
- xAI image `quality`, image `mask`, and extra native-only aspect ratios are
not exposed until the shared `image_generate` tool has corresponding
cross-provider controls.

Advanced notes

- OpenClaw applies xAI-specific tool-schema and tool-call compatibility fixes
automatically on the shared runner path.
- Native xAI requests default `tool_stream: true`. Set
`agents.defaults.models["xai/<model>"].params.tool_stream` to `false` to
disable it.
- The bundled xAI wrapper strips unsupported strict tool-schema flags and
reasoning payload keys before sending native xAI requests.
- `web_search`, `x_search`, and `code_execution` are exposed as OpenClaw
tools. OpenClaw enables the specific xAI built-in it needs inside each tool
request instead of attaching all native tools to every chat turn.
- Grok `web_search` reads `plugins.entries.xai.config.webSearch.baseUrl`.
`x_search` reads `plugins.entries.xai.config.xSearch.baseUrl`, then
falls back to the Grok web-search base URL.
- `x_search` and `code_execution` are owned by the bundled xAI plugin rather
than hardcoded into the core model runtime.
- `code_execution` is remote xAI sandbox execution, not local
[`exec`](https://docs.openclaw.ai/tools/exec).

## [​](https://docs.openclaw.ai/providers/xai\#live-testing)  Live testing

The xAI media paths are covered by unit tests and opt-in live suites. The live
commands load secrets from your login shell, including `~/.profile`, before
probing `XAI_API_KEY`.

```
pnpm test extensions/xai
OPENCLAW_LIVE_TEST=1 OPENCLAW_LIVE_TEST_QUIET=1 pnpm test:live -- extensions/xai/xai.live.test.ts
OPENCLAW_LIVE_TEST=1 OPENCLAW_LIVE_TEST_QUIET=1 OPENCLAW_LIVE_IMAGE_GENERATION_PROVIDERS=xai pnpm test:live -- test/image-generation.runtime.live.test.ts
```

The provider-specific live file synthesizes normal TTS, telephony-friendly PCM
TTS, transcribes audio through xAI batch STT, streams the same PCM through xAI
realtime STT, generates text-to-image output, and edits a reference image. The
shared image live file verifies the same xAI provider through OpenClaw’s
runtime selection, fallback, normalization, and media attachment path.

## [​](https://docs.openclaw.ai/providers/xai\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Video generation** \\
\\
Shared video tool parameters and provider selection.](https://docs.openclaw.ai/tools/video-generation)

[**All providers** \\
\\
The broader provider overview.](https://docs.openclaw.ai/providers/index)

[**Troubleshooting** \\
\\
Common issues and fixes.](https://docs.openclaw.ai/help/troubleshooting)

[Vydra](https://docs.openclaw.ai/providers/vydra) [Xiaomi MiMo](https://docs.openclaw.ai/providers/xiaomi)

Ctrl+I