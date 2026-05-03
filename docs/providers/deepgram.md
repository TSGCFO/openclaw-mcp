---
source_url: https://docs.openclaw.ai/providers/deepgram
title: "Deepgram - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/deepgram#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Deepgram

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/deepgram#getting-started)
- [Configuration options](https://docs.openclaw.ai/providers/deepgram#configuration-options)
- [Voice Call streaming STT](https://docs.openclaw.ai/providers/deepgram#voice-call-streaming-stt)
- [Notes](https://docs.openclaw.ai/providers/deepgram#notes)
- [Related](https://docs.openclaw.ai/providers/deepgram#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Deepgram is a speech-to-text API. In OpenClaw it is used for inbound
audio/voice-note transcription through `tools.media.audio` and for Voice Call
streaming STT through `plugins.entries.voice-call.config.streaming`.For batch transcription, OpenClaw uploads the complete audio file to Deepgram
and injects the transcript into the reply pipeline (`{{Transcript}}` +
`[Audio]` block). For Voice Call streaming, OpenClaw forwards live G.711
u-law frames over Deepgram’s WebSocket `listen` endpoint and emits partial or
final transcripts as Deepgram returns them.

| Detail | Value |
| --- | --- |
| Website | [deepgram.com](https://deepgram.com/) |
| Docs | [developers.deepgram.com](https://developers.deepgram.com/) |
| Auth | `DEEPGRAM_API_KEY` |
| Default model | `nova-3` |

## [​](https://docs.openclaw.ai/providers/deepgram\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/deepgram#)

Set your API key

Add your Deepgram API key to the environment:

```
DEEPGRAM_API_KEY=dg_...
```

2

[Navigate to header](https://docs.openclaw.ai/providers/deepgram#)

Enable the audio provider

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "deepgram", model: "nova-3" }],
      },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/deepgram#)

Send a voice note

Send an audio message through any connected channel. OpenClaw transcribes it
via Deepgram and injects the transcript into the reply pipeline.

## [​](https://docs.openclaw.ai/providers/deepgram\#configuration-options)  Configuration options

| Option | Path | Description |
| --- | --- | --- |
| `model` | `tools.media.audio.models[].model` | Deepgram model id (default: `nova-3`) |
| `language` | `tools.media.audio.models[].language` | Language hint (optional) |
| `detect_language` | `tools.media.audio.providerOptions.deepgram.detect_language` | Enable language detection (optional) |
| `punctuate` | `tools.media.audio.providerOptions.deepgram.punctuate` | Enable punctuation (optional) |
| `smart_format` | `tools.media.audio.providerOptions.deepgram.smart_format` | Enable smart formatting (optional) |

- With language hint

- With Deepgram options


```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "deepgram", model: "nova-3", language: "en" }],
      },
    },
  },
}
```

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        providerOptions: {
          deepgram: {
            detect_language: true,
            punctuate: true,
            smart_format: true,
          },
        },
        models: [{ provider: "deepgram", model: "nova-3" }],
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/providers/deepgram\#voice-call-streaming-stt)  Voice Call streaming STT

The bundled `deepgram` plugin also registers a realtime transcription provider
for the Voice Call plugin.

| Setting | Config path | Default |
| --- | --- | --- |
| API key | `plugins.entries.voice-call.config.streaming.providers.deepgram.apiKey` | Falls back to `DEEPGRAM_API_KEY` |
| Model | `...deepgram.model` | `nova-3` |
| Language | `...deepgram.language` | (unset) |
| Encoding | `...deepgram.encoding` | `mulaw` |
| Sample rate | `...deepgram.sampleRate` | `8000` |
| Endpointing | `...deepgram.endpointingMs` | `800` |
| Interim results | `...deepgram.interimResults` | `true` |

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          streaming: {
            enabled: true,
            provider: "deepgram",
            providers: {
              deepgram: {
                apiKey: "${DEEPGRAM_API_KEY}",
                model: "nova-3",
                endpointingMs: 800,
                language: "en-US",
              },
            },
          },
        },
      },
    },
  },
}
```

Voice Call receives telephony audio as 8 kHz G.711 u-law. The Deepgram
streaming provider defaults to `encoding: "mulaw"` and `sampleRate: 8000`, so
Twilio media frames can be forwarded directly.

## [​](https://docs.openclaw.ai/providers/deepgram\#notes)  Notes

Authentication

Authentication follows the standard provider auth order. `DEEPGRAM_API_KEY` is
the simplest path.

Proxy and custom endpoints

Override endpoints or headers with `tools.media.audio.baseUrl` and
`tools.media.audio.headers` when using a proxy.

Output behavior

Output follows the same audio rules as other providers (size caps, timeouts,
transcript injection).

## [​](https://docs.openclaw.ai/providers/deepgram\#related)  Related

[**Media tools** \\
\\
Audio, image, and video processing pipeline overview.](https://docs.openclaw.ai/tools/media-overview)

[**Configuration** \\
\\
Full config reference including media tool settings.](https://docs.openclaw.ai/gateway/configuration)

[**Troubleshooting** \\
\\
Common issues and debugging steps.](https://docs.openclaw.ai/help/troubleshooting)

[**FAQ** \\
\\
Frequently asked questions about OpenClaw setup.](https://docs.openclaw.ai/help/faq)

[ComfyUI](https://docs.openclaw.ai/providers/comfy) [Deepinfra](https://docs.openclaw.ai/providers/deepinfra)

Ctrl+I