---
source_url: https://docs.openclaw.ai/nodes/talk
title: "Talk mode - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/nodes/talk#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Node features

Talk mode

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Behavior (macOS)](https://docs.openclaw.ai/nodes/talk#behavior-macos)
- [Voice directives in replies](https://docs.openclaw.ai/nodes/talk#voice-directives-in-replies)
- [Config (~/.openclaw/openclaw.json)](https://docs.openclaw.ai/nodes/talk#config-%2F-openclaw%2Fopenclaw-json)
- [macOS UI](https://docs.openclaw.ai/nodes/talk#macos-ui)
- [Android UI](https://docs.openclaw.ai/nodes/talk#android-ui)
- [Notes](https://docs.openclaw.ai/nodes/talk#notes)
- [Related](https://docs.openclaw.ai/nodes/talk#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Talk mode is a continuous voice conversation loop:

1. Listen for speech
2. Send transcript to the model (main session, chat.send)
3. Wait for the response
4. Speak it via the configured Talk provider (`talk.speak`)

## [​](https://docs.openclaw.ai/nodes/talk\#behavior-macos)  Behavior (macOS)

- **Always-on overlay** while Talk mode is enabled.
- **Listening → Thinking → Speaking** phase transitions.
- On a **short pause** (silence window), the current transcript is sent.
- Replies are **written to WebChat** (same as typing).
- **Interrupt on speech** (default on): if the user starts talking while the assistant is speaking, we stop playback and note the interruption timestamp for the next prompt.

## [​](https://docs.openclaw.ai/nodes/talk\#voice-directives-in-replies)  Voice directives in replies

The assistant may prefix its reply with a **single JSON line** to control voice:

```
{ "voice": "<voice-id>", "once": true }
```

Rules:

- First non-empty line only.
- Unknown keys are ignored.
- `once: true` applies to the current reply only.
- Without `once`, the voice becomes the new default for Talk mode.
- The JSON line is stripped before TTS playback.

Supported keys:

- `voice` / `voice_id` / `voiceId`
- `model` / `model_id` / `modelId`
- `speed`, `rate` (WPM), `stability`, `similarity`, `style`, `speakerBoost`
- `seed`, `normalize`, `lang`, `output_format`, `latency_tier`
- `once`

## [​](https://docs.openclaw.ai/nodes/talk\#config-/-openclaw/openclaw-json)  Config (`~/.openclaw/openclaw.json`)

```
{
  talk: {
    provider: "elevenlabs",
    providers: {
      elevenlabs: {
        voiceId: "elevenlabs_voice_id",
        modelId: "eleven_v3",
        outputFormat: "mp3_44100_128",
        apiKey: "elevenlabs_api_key",
      },
      mlx: {
        modelId: "mlx-community/Soprano-80M-bf16",
      },
      system: {},
    },
    speechLocale: "ru-RU",
    silenceTimeoutMs: 1500,
    interruptOnSpeech: true,
  },
}
```

Defaults:

- `interruptOnSpeech`: true
- `silenceTimeoutMs`: when unset, Talk keeps the platform default pause window before sending the transcript (`700 ms on macOS and Android, 900 ms on iOS`)
- `provider`: selects the active Talk provider. Use `elevenlabs`, `mlx`, or `system` for the macOS-local playback paths.
- `providers.<provider>.voiceId`: falls back to `ELEVENLABS_VOICE_ID` / `SAG_VOICE_ID` for ElevenLabs (or first ElevenLabs voice when API key is available).
- `providers.elevenlabs.modelId`: defaults to `eleven_v3` when unset.
- `providers.mlx.modelId`: defaults to `mlx-community/Soprano-80M-bf16` when unset.
- `providers.elevenlabs.apiKey`: falls back to `ELEVENLABS_API_KEY` (or gateway shell profile if available).
- `speechLocale`: optional BCP 47 locale id for on-device Talk speech recognition on iOS/macOS. Leave unset to use the device default.
- `outputFormat`: defaults to `pcm_44100` on macOS/iOS and `pcm_24000` on Android (set `mp3_*` to force MP3 streaming)

## [​](https://docs.openclaw.ai/nodes/talk\#macos-ui)  macOS UI

- Menu bar toggle: **Talk**
- Config tab: **Talk Mode** group (voice id + interrupt toggle)
- Overlay:
  - **Listening**: cloud pulses with mic level
  - **Thinking**: sinking animation
  - **Speaking**: radiating rings
  - Click cloud: stop speaking
  - Click X: exit Talk mode

## [​](https://docs.openclaw.ai/nodes/talk\#android-ui)  Android UI

- Voice tab toggle: **Talk**
- Manual **Mic** and **Talk** are mutually exclusive runtime capture modes.
- Manual Mic stops when the app leaves the foreground or the user leaves the Voice tab.
- Talk Mode keeps running until toggled off or the Android node disconnects, and uses Android’s microphone foreground-service type while active.

## [​](https://docs.openclaw.ai/nodes/talk\#notes)  Notes

- Requires Speech + Microphone permissions.
- Uses `chat.send` against session key `main`.
- The gateway resolves Talk playback through `talk.speak` using the active Talk provider. Android falls back to local system TTS only when that RPC is unavailable.
- macOS local MLX playback uses the bundled `openclaw-mlx-tts` helper when present, or an executable on `PATH`. Set `OPENCLAW_MLX_TTS_BIN` to point at a custom helper binary during development.
- `stability` for `eleven_v3` is validated to `0.0`, `0.5`, or `1.0`; other models accept `0..1`.
- `latency_tier` is validated to `0..4` when set.
- Android supports `pcm_16000`, `pcm_22050`, `pcm_24000`, and `pcm_44100` output formats for low-latency AudioTrack streaming.

## [​](https://docs.openclaw.ai/nodes/talk\#related)  Related

- [Voice wake](https://docs.openclaw.ai/nodes/voicewake)
- [Audio and voice notes](https://docs.openclaw.ai/nodes/audio)
- [Media understanding](https://docs.openclaw.ai/nodes/media-understanding)

[Text to speech (TTS)](https://docs.openclaw.ai/tools/tts) [Voice wake](https://docs.openclaw.ai/nodes/voicewake)

Ctrl+I