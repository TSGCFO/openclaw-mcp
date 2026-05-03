---
source_url: https://docs.openclaw.ai/providers/senseaudio
title: "SenseAudio - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/senseaudio#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

SenseAudio

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [SenseAudio](https://docs.openclaw.ai/providers/senseaudio#senseaudio)
- [Getting Started](https://docs.openclaw.ai/providers/senseaudio#getting-started)
- [Options](https://docs.openclaw.ai/providers/senseaudio#options)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/providers/senseaudio\#senseaudio)  SenseAudio

SenseAudio can transcribe inbound audio/voice-note attachments through
OpenClaw’s shared `tools.media.audio` pipeline. OpenClaw posts multipart audio
to the OpenAI-compatible transcription endpoint and injects the returned text
as `{{Transcript}}` plus an `[Audio]` block.

| Detail | Value |
| --- | --- |
| Website | [senseaudio.cn](https://senseaudio.cn/) |
| Docs | [senseaudio.cn/docs](https://senseaudio.cn/docs) |
| Auth | `SENSEAUDIO_API_KEY` |
| Default model | `senseaudio-asr-pro-1.5-260319` |
| Default URL | `https://api.senseaudio.cn/v1` |

## [​](https://docs.openclaw.ai/providers/senseaudio\#getting-started)  Getting Started

1

[Navigate to header](https://docs.openclaw.ai/providers/senseaudio#)

Set your API key

```
export SENSEAUDIO_API_KEY="..."
```

2

[Navigate to header](https://docs.openclaw.ai/providers/senseaudio#)

Enable the audio provider

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "senseaudio", model: "senseaudio-asr-pro-1.5-260319" }],
      },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/senseaudio#)

Send a voice note

Send an audio message through any connected channel. OpenClaw uploads the
audio to SenseAudio and uses the transcript in the reply pipeline.

## [​](https://docs.openclaw.ai/providers/senseaudio\#options)  Options

| Option | Path | Description |
| --- | --- | --- |
| `model` | `tools.media.audio.models[].model` | SenseAudio ASR model id |
| `language` | `tools.media.audio.models[].language` | Optional language hint |
| `prompt` | `tools.media.audio.prompt` | Optional transcription prompt |
| `baseUrl` | `tools.media.audio.baseUrl` or model | Override the OpenAI-compatible base |
| `headers` | `tools.media.audio.request.headers` | Extra request headers |

SenseAudio is batch STT only in OpenClaw. Voice Call realtime transcription
continues to use providers with streaming STT support.

[Runway](https://docs.openclaw.ai/providers/runway) [SGLang](https://docs.openclaw.ai/providers/sglang)

Ctrl+I