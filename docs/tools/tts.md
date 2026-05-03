---
source_url: https://docs.openclaw.ai/tools/tts
title: "Text-to-speech - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/tts#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Tools

Text-to-speech

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Quick start](https://docs.openclaw.ai/tools/tts#quick-start)
- [Supported providers](https://docs.openclaw.ai/tools/tts#supported-providers)
- [Configuration](https://docs.openclaw.ai/tools/tts#configuration)
- [Per-agent voice overrides](https://docs.openclaw.ai/tools/tts#per-agent-voice-overrides)
- [Personas](https://docs.openclaw.ai/tools/tts#personas)
- [Minimal persona](https://docs.openclaw.ai/tools/tts#minimal-persona)
- [Full persona (provider-neutral prompt)](https://docs.openclaw.ai/tools/tts#full-persona-provider-neutral-prompt)
- [Persona resolution](https://docs.openclaw.ai/tools/tts#persona-resolution)
- [How providers use persona prompts](https://docs.openclaw.ai/tools/tts#how-providers-use-persona-prompts)
- [Fallback policy](https://docs.openclaw.ai/tools/tts#fallback-policy)
- [Model-driven directives](https://docs.openclaw.ai/tools/tts#model-driven-directives)
- [Slash commands](https://docs.openclaw.ai/tools/tts#slash-commands)
- [Per-user preferences](https://docs.openclaw.ai/tools/tts#per-user-preferences)
- [Output formats (fixed)](https://docs.openclaw.ai/tools/tts#output-formats-fixed)
- [Auto-TTS behavior](https://docs.openclaw.ai/tools/tts#auto-tts-behavior)
- [Output formats by channel](https://docs.openclaw.ai/tools/tts#output-formats-by-channel)
- [Field reference](https://docs.openclaw.ai/tools/tts#field-reference)
- [Agent tool](https://docs.openclaw.ai/tools/tts#agent-tool)
- [Gateway RPC](https://docs.openclaw.ai/tools/tts#gateway-rpc)
- [Service links](https://docs.openclaw.ai/tools/tts#service-links)
- [Related](https://docs.openclaw.ai/tools/tts#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw can convert outbound replies into audio across **14 speech providers**
and deliver native voice messages on Feishu, Matrix, Telegram, and WhatsApp,
audio attachments everywhere else, and PCM/Ulaw streams for telephony and Talk.

## [​](https://docs.openclaw.ai/tools/tts\#quick-start)  Quick start

1

[Navigate to header](https://docs.openclaw.ai/tools/tts#)

Pick a provider

OpenAI and ElevenLabs are the most reliable hosted options. Microsoft and
Local CLI work without an API key. See the [provider matrix](https://docs.openclaw.ai/tools/tts#supported-providers)
for the full list.

2

[Navigate to header](https://docs.openclaw.ai/tools/tts#)

Set the API key

Export the env var for your provider (for example `OPENAI_API_KEY`,
`ELEVENLABS_API_KEY`). Microsoft and Local CLI need no key.

3

[Navigate to header](https://docs.openclaw.ai/tools/tts#)

Enable in config

Set `messages.tts.auto: "always"` and `messages.tts.provider`:

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "elevenlabs",
    },
  },
}
```

4

[Navigate to header](https://docs.openclaw.ai/tools/tts#)

Try it in chat

`/tts status` shows the current state. `/tts audio Hello from OpenClaw`
sends a one-off audio reply.

Auto-TTS is **off** by default. When `messages.tts.provider` is unset,
OpenClaw picks the first configured provider in registry auto-select order.
The built-in `tts` agent tool is explicit-intent only: ordinary chat stays
text unless the user asks for audio, uses `/tts`, or enables Auto-TTS/directive
speech.

## [​](https://docs.openclaw.ai/tools/tts\#supported-providers)  Supported providers

| Provider | Auth | Notes |
| --- | --- | --- |
| **Azure Speech** | `AZURE_SPEECH_KEY` \+ `AZURE_SPEECH_REGION` (also `AZURE_SPEECH_API_KEY`, `SPEECH_KEY`, `SPEECH_REGION`) | Native Ogg/Opus voice-note output and telephony. |
| **DeepInfra** | `DEEPINFRA_API_KEY` | OpenAI-compatible TTS. Defaults to `hexgrad/Kokoro-82M`. |
| **ElevenLabs** | `ELEVENLABS_API_KEY` or `XI_API_KEY` | Voice cloning, multilingual, deterministic via `seed`. |
| **Google Gemini** | `GEMINI_API_KEY` or `GOOGLE_API_KEY` | Gemini API TTS; persona-aware via `promptTemplate: "audio-profile-v1"`. |
| **Gradium** | `GRADIUM_API_KEY` | Voice-note and telephony output. |
| **Inworld** | `INWORLD_API_KEY` | Streaming TTS API. Native Opus voice-note and PCM telephony. |
| **Local CLI** | none | Runs a configured local TTS command. |
| **Microsoft** | none | Public Edge neural TTS via `node-edge-tts`. Best-effort, no SLA. |
| **MiniMax** | `MINIMAX_API_KEY` (or Token Plan: `MINIMAX_OAUTH_TOKEN`, `MINIMAX_CODE_PLAN_KEY`, `MINIMAX_CODING_API_KEY`) | T2A v2 API. Defaults to `speech-2.8-hd`. |
| **OpenAI** | `OPENAI_API_KEY` | Also used for auto-summary; supports persona `instructions`. |
| **OpenRouter** | `OPENROUTER_API_KEY` (can reuse `models.providers.openrouter.apiKey`) | Default model `hexgrad/kokoro-82m`. |
| **Volcengine** | `VOLCENGINE_TTS_API_KEY` or `BYTEPLUS_SEED_SPEECH_API_KEY` (legacy AppID/token: `VOLCENGINE_TTS_APPID`/`_TOKEN`) | BytePlus Seed Speech HTTP API. |
| **Vydra** | `VYDRA_API_KEY` | Shared image, video, and speech provider. |
| **xAI** | `XAI_API_KEY` | xAI batch TTS. Native Opus voice-note is **not** supported. |
| **Xiaomi MiMo** | `XIAOMI_API_KEY` | MiMo TTS through Xiaomi chat completions. |

If multiple providers are configured, the selected one is used first and the
others are fallback options. Auto-summary uses `summaryModel` (or
`agents.defaults.model.primary`), so that provider must also be authenticated
if you keep summaries enabled.

The bundled **Microsoft** provider uses Microsoft Edge’s online neural TTS
service via `node-edge-tts`. It is a public web service without a published
SLA or quota — treat it as best-effort. The legacy provider id `edge` is
normalized to `microsoft` and `openclaw doctor --fix` rewrites persisted
config; new configs should always use `microsoft`.

## [​](https://docs.openclaw.ai/tools/tts\#configuration)  Configuration

TTS config lives under `messages.tts` in `~/.openclaw/openclaw.json`. Pick a
preset and adapt the provider block:

- Azure Speech

- ElevenLabs

- Google Gemini

- Gradium

- Inworld

- Local CLI

- Microsoft (no key)

- MiniMax

- OpenAI + ElevenLabs

- OpenRouter

- Volcengine

- xAI

- Xiaomi MiMo


```
{
  messages: {
    tts: {
      auto: "always",
      provider: "azure-speech",
      providers: {
        "azure-speech": {
          apiKey: "${AZURE_SPEECH_KEY}",
          region: "eastus",
          voice: "en-US-JennyNeural",
          lang: "en-US",
          outputFormat: "audio-24khz-48kbitrate-mono-mp3",
          voiceNoteOutputFormat: "ogg-24khz-16bit-mono-opus",
        },
      },
    },
  },
}
```

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "elevenlabs",
      providers: {
        elevenlabs: {
          apiKey: "${ELEVENLABS_API_KEY}",
          model: "eleven_multilingual_v2",
          voiceId: "EXAVITQu4vr4xnSDxMaL",
        },
      },
    },
  },
}
```

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "google",
      providers: {
        google: {
          apiKey: "${GEMINI_API_KEY}",
          model: "gemini-3.1-flash-tts-preview",
          voiceName: "Kore",
          // Optional natural-language style prompts:
          // audioProfile: "Speak in a calm, podcast-host tone.",
          // speakerName: "Alex",
        },
      },
    },
  },
}
```

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "gradium",
      providers: {
        gradium: {
          apiKey: "${GRADIUM_API_KEY}",
          voiceId: "YTpq7expH9539ERJ",
        },
      },
    },
  },
}
```

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "inworld",
      providers: {
        inworld: {
          apiKey: "${INWORLD_API_KEY}",
          modelId: "inworld-tts-1.5-max",
          voiceId: "Sarah",
          temperature: 0.7,
        },
      },
    },
  },
}
```

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "tts-local-cli",
      providers: {
        "tts-local-cli": {
          command: "say",
          args: ["-o", "{{OutputPath}}", "{{Text}}"],
          outputFormat: "wav",
          timeoutMs: 120000,
        },
      },
    },
  },
}
```

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "microsoft",
      providers: {
        microsoft: {
          enabled: true,
          voice: "en-US-MichelleNeural",
          lang: "en-US",
          outputFormat: "audio-24khz-48kbitrate-mono-mp3",
          rate: "+0%",
          pitch: "+0%",
        },
      },
    },
  },
}
```

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "minimax",
      providers: {
        minimax: {
          apiKey: "${MINIMAX_API_KEY}",
          model: "speech-2.8-hd",
          voiceId: "English_expressive_narrator",
          speed: 1.0,
          vol: 1.0,
          pitch: 0,
        },
      },
    },
  },
}
```

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "openai",
      summaryModel: "openai/gpt-4.1-mini",
      modelOverrides: { enabled: true },
      providers: {
        openai: {
          apiKey: "${OPENAI_API_KEY}",
          model: "gpt-4o-mini-tts",
          voice: "alloy",
        },
        elevenlabs: {
          apiKey: "${ELEVENLABS_API_KEY}",
          model: "eleven_multilingual_v2",
          voiceId: "EXAVITQu4vr4xnSDxMaL",
          voiceSettings: { stability: 0.5, similarityBoost: 0.75, style: 0.0, useSpeakerBoost: true, speed: 1.0 },
          applyTextNormalization: "auto",
          languageCode: "en",
        },
      },
    },
  },
}
```

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "openrouter",
      providers: {
        openrouter: {
          apiKey: "${OPENROUTER_API_KEY}",
          model: "hexgrad/kokoro-82m",
          voice: "af_alloy",
          responseFormat: "mp3",
        },
      },
    },
  },
}
```

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "volcengine",
      providers: {
        volcengine: {
          apiKey: "${VOLCENGINE_TTS_API_KEY}",
          resourceId: "seed-tts-1.0",
          voice: "en_female_anna_mars_bigtts",
        },
      },
    },
  },
}
```

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "xai",
      providers: {
        xai: {
          apiKey: "${XAI_API_KEY}",
          voiceId: "eve",
          language: "en",
          responseFormat: "mp3",
        },
      },
    },
  },
}
```

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "xiaomi",
      providers: {
        xiaomi: {
          apiKey: "${XIAOMI_API_KEY}",
          model: "mimo-v2.5-tts",
          voice: "mimo_default",
          format: "mp3",
        },
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/tools/tts\#per-agent-voice-overrides)  Per-agent voice overrides

Use `agents.list[].tts` when one agent should speak with a different provider,
voice, model, persona, or auto-TTS mode. The agent block deep-merges over
`messages.tts`, so provider credentials can stay in the global provider config:

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "elevenlabs",
      providers: {
        elevenlabs: { apiKey: "${ELEVENLABS_API_KEY}", model: "eleven_multilingual_v2" },
      },
    },
  },
  agents: {
    list: [\
      {\
        id: "reader",\
        tts: {\
          providers: {\
            elevenlabs: { voiceId: "EXAVITQu4vr4xnSDxMaL" },\
          },\
        },\
      },\
    ],
  },
}
```

To pin a per-agent persona, set `agents.list[].tts.persona` alongside provider
config — it overrides the global `messages.tts.persona` for that agent only.Precedence order for automatic replies, `/tts audio`, `/tts status`, and the
`tts` agent tool:

1. `messages.tts`
2. active `agents.list[].tts`
3. channel override, when the channel supports `channels.<channel>.tts`
4. account override, when the channel passes `channels.<channel>.accounts.<id>.tts`
5. local `/tts` preferences for this host
6. inline `[[tts:...]]` directives when [model overrides](https://docs.openclaw.ai/tools/tts#model-driven-directives) are enabled

Channel and account overrides use the same shape as `messages.tts` and
deep-merge over the earlier layers, so shared provider credentials can stay in
`messages.tts` while a channel or bot account changes only voice, model, persona,
or auto mode:

```
{
  messages: {
    tts: {
      provider: "openai",
      providers: {
        openai: { apiKey: "${OPENAI_API_KEY}", model: "gpt-4o-mini-tts" },
      },
    },
  },
  channels: {
    feishu: {
      accounts: {
        english: {
          tts: {
            providers: {
              openai: { voice: "shimmer" },
            },
          },
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/tools/tts\#personas)  Personas

A **persona** is a stable spoken identity that can be applied deterministically
across providers. It can prefer one provider, define provider-neutral prompt
intent, and carry provider-specific bindings for voices, models, prompt
templates, seeds, and voice settings.

### [​](https://docs.openclaw.ai/tools/tts\#minimal-persona)  Minimal persona

```
{
  messages: {
    tts: {
      auto: "always",
      persona: "narrator",
      personas: {
        narrator: {
          label: "Narrator",
          provider: "elevenlabs",
          providers: {
            elevenlabs: { voiceId: "EXAVITQu4vr4xnSDxMaL", modelId: "eleven_multilingual_v2" },
          },
        },
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/tools/tts\#full-persona-provider-neutral-prompt)  Full persona (provider-neutral prompt)

```
{
  messages: {
    tts: {
      auto: "always",
      persona: "alfred",
      personas: {
        alfred: {
          label: "Alfred",
          description: "Dry, warm British butler narrator.",
          provider: "google",
          fallbackPolicy: "preserve-persona",
          prompt: {
            profile: "A brilliant British butler. Dry, witty, warm, charming, emotionally expressive, never generic.",
            scene: "A quiet late-night study. Close-mic narration for a trusted operator.",
            sampleContext: "The speaker is answering a private technical request with concise confidence and dry warmth.",
            style: "Refined, understated, lightly amused.",
            accent: "British English.",
            pacing: "Measured, with short dramatic pauses.",
            constraints: ["Do not read configuration values aloud.", "Do not explain the persona."],
          },
          providers: {
            google: {
              model: "gemini-3.1-flash-tts-preview",
              voiceName: "Algieba",
              promptTemplate: "audio-profile-v1",
            },
            openai: { model: "gpt-4o-mini-tts", voice: "cedar" },
            elevenlabs: {
              voiceId: "voice_id",
              modelId: "eleven_multilingual_v2",
              seed: 42,
              voiceSettings: {
                stability: 0.65,
                similarityBoost: 0.8,
                style: 0.25,
                useSpeakerBoost: true,
                speed: 0.95,
              },
            },
          },
        },
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/tools/tts\#persona-resolution)  Persona resolution

The active persona is selected deterministically:

1. `/tts persona <id>` local preference, if set.
2. `messages.tts.persona`, if set.
3. No persona.

Provider selection runs explicit-first:

1. Direct overrides (CLI, gateway, Talk, allowed TTS directives).
2. `/tts provider <id>` local preference.
3. Active persona’s `provider`.
4. `messages.tts.provider`.
5. Registry auto-select.

For each provider attempt, OpenClaw merges configs in this order:

1. `messages.tts.providers.<id>`
2. `messages.tts.personas.<persona>.providers.<id>`
3. Trusted request overrides
4. Allowed model-emitted TTS directive overrides

### [​](https://docs.openclaw.ai/tools/tts\#how-providers-use-persona-prompts)  How providers use persona prompts

Persona prompt fields (`profile`, `scene`, `sampleContext`, `style`, `accent`,
`pacing`, `constraints`) are **provider-neutral**. Each provider decides how
to use them:

Google Gemini

Wraps persona prompt fields in a Gemini TTS prompt structure **only when**
the effective Google provider config sets `promptTemplate: "audio-profile-v1"`
or `personaPrompt`. The older `audioProfile` and `speakerName` fields are
still prepended as Google-specific prompt text. Inline audio tags such as
`[whispers]` or `[laughs]` inside a `[[tts:text]]` block are preserved
inside the Gemini transcript; OpenClaw does not generate these tags.

OpenAI

Maps persona prompt fields to the request `instructions` field **only when**
no explicit OpenAI `instructions` is configured. Explicit `instructions`
always wins.

Other providers

Use only the provider-specific persona bindings under
`personas.<id>.providers.<provider>`. Persona prompt fields are ignored
unless the provider implements its own persona-prompt mapping.

### [​](https://docs.openclaw.ai/tools/tts\#fallback-policy)  Fallback policy

`fallbackPolicy` controls behavior when a persona has **no binding** for the
attempted provider:

| Policy | Behavior |
| --- | --- |
| `preserve-persona` | **Default.** Provider-neutral prompt fields stay available; the provider may use them or ignore them. |
| `provider-defaults` | Persona is omitted from prompt preparation for that attempt; the provider uses its neutral defaults while fallback to other providers continues. |
| `fail` | Skip that provider attempt with `reasonCode: "not_configured"` and `personaBinding: "missing"`. Fallback providers are still tried. |

The whole TTS request only fails when **every** attempted provider is skipped
or fails.

## [​](https://docs.openclaw.ai/tools/tts\#model-driven-directives)  Model-driven directives

By default, the assistant **can** emit `[[tts:...]]` directives to override
voice, model, or speed for a single reply, plus an optional
`[[tts:text]]...[[/tts:text]]` block for expressive cues that should appear in
audio only:

```
Here you go.

[[tts:voiceId=pMsXgVXv3BLzUgSXRplE model=eleven_v3 speed=1.1]]
[[tts:text]](laughs) Read the song once more.[[/tts:text]]
```

When `messages.tts.auto` is `"tagged"`, **directives are required** to trigger
audio. Streaming block delivery strips directives from visible text before the
channel sees them, even when split across adjacent blocks.`provider=...` is ignored unless `modelOverrides.allowProvider: true`. When a
reply declares `provider=...`, the other keys in that directive are parsed
only by that provider; unsupported keys are stripped and reported as TTS
directive warnings.**Available directive keys:**

- `provider` (registered provider id; requires `allowProvider: true`)
- `voice` / `voiceName` / `voice_name` / `google_voice` / `voiceId`
- `model` / `google_model`
- `stability`, `similarityBoost`, `style`, `speed`, `useSpeakerBoost`
- `vol` / `volume` (MiniMax volume, 0–10)
- `pitch` (MiniMax integer pitch, −12 to 12; fractional values are truncated)
- `emotion` (Volcengine emotion tag)
- `applyTextNormalization` (`auto|on|off`)
- `languageCode` (ISO 639-1)
- `seed`

**Disable model overrides entirely:**

```
{ messages: { tts: { modelOverrides: { enabled: false } } } }
```

**Allow provider switching while keeping other knobs configurable:**

```
{ messages: { tts: { modelOverrides: { enabled: true, allowProvider: true, allowSeed: false } } } }
```

## [​](https://docs.openclaw.ai/tools/tts\#slash-commands)  Slash commands

Single command `/tts`. On Discord, OpenClaw also registers `/voice` because
`/tts` is a built-in Discord command — text `/tts ...` still works.

```
/tts off | on | status
/tts chat on | off | default
/tts latest
/tts provider <id>
/tts persona <id> | off
/tts limit <chars>
/tts summary off
/tts audio <text>
```

Commands require an authorized sender (allowlist/owner rules apply) and either
`commands.text` or native command registration must be enabled.

Behavior notes:

- `/tts on` writes the local TTS preference to `always`; `/tts off` writes it to `off`.
- `/tts chat on|off|default` writes a session-scoped auto-TTS override for the current chat.
- `/tts persona <id>` writes the local persona preference; `/tts persona off` clears it.
- `/tts latest` reads the latest assistant reply from the current session transcript and sends it as audio once. It stores only a hash of that reply on the session entry to suppress duplicate voice sends.
- `/tts audio` generates a one-off audio reply (does **not** toggle TTS on).
- `limit` and `summary` are stored in **local prefs**, not the main config.
- `/tts status` includes fallback diagnostics for the latest attempt — `Fallback: <primary> -> <used>`, `Attempts: ...`, and per-attempt detail (`provider:outcome(reasonCode) latency`).
- `/status` shows the active TTS mode plus configured provider, model, voice, and sanitized custom endpoint metadata when TTS is enabled.

## [​](https://docs.openclaw.ai/tools/tts\#per-user-preferences)  Per-user preferences

Slash commands write local overrides to `prefsPath`. The default is
`~/.openclaw/settings/tts.json`; override with the `OPENCLAW_TTS_PREFS` env var
or `messages.tts.prefsPath`.

| Stored field | Effect |
| --- | --- |
| `auto` | Local auto-TTS override (`always`, `off`, …) |
| `provider` | Local primary provider override |
| `persona` | Local persona override |
| `maxLength` | Summary threshold (default `1500` chars) |
| `summarize` | Summary toggle (default `true`) |

These override the effective config from `messages.tts` plus the active
`agents.list[].tts` block for that host.

## [​](https://docs.openclaw.ai/tools/tts\#output-formats-fixed)  Output formats (fixed)

TTS voice delivery is channel-capability driven. Channel plugins advertise
whether voice-style TTS should ask providers for a native `voice-note` target or
keep normal `audio-file` synthesis and only mark compatible output for voice
delivery.

- **Voice-note capable channels**: voice-note replies prefer Opus (`opus_48000_64` from ElevenLabs, `opus` from OpenAI).

  - 48kHz / 64kbps is a good voice message tradeoff.
- **Feishu / WhatsApp**: when a voice-note reply is produced as MP3/WebM/WAV/M4A
or another likely audio file, the channel plugin transcodes it to 48kHz
Ogg/Opus with `ffmpeg` before sending the native voice message. WhatsApp sends
the result through the Baileys `audio` payload with `ptt: true` and
`audio/ogg; codecs=opus`. If conversion fails, Feishu receives the original
file as an attachment; WhatsApp send fails rather than posting an incompatible
PTT payload.
- **BlueBubbles**: keeps provider synthesis on the normal audio-file path; MP3
and CAF outputs are marked for iMessage voice memo delivery.
- **Other channels**: MP3 (`mp3_44100_128` from ElevenLabs, `mp3` from OpenAI).

  - 44.1kHz / 128kbps is the default balance for speech clarity.
- **MiniMax**: MP3 (`speech-2.8-hd` model, 32kHz sample rate) for normal audio attachments. For channel-advertised voice-note targets, OpenClaw transcodes the MiniMax MP3 to 48kHz Opus with `ffmpeg` before delivery when the channel advertises transcoding.
- **Xiaomi MiMo**: MP3 by default, or WAV when configured. For channel-advertised voice-note targets, OpenClaw transcodes Xiaomi output to 48kHz Opus with `ffmpeg` before delivery when the channel advertises transcoding.
- **Local CLI**: uses the configured `outputFormat`. Voice-note targets are
converted to Ogg/Opus and telephony output is converted to raw 16 kHz mono PCM
with `ffmpeg`.
- **Google Gemini**: Gemini API TTS returns raw 24kHz PCM. OpenClaw wraps it as WAV for audio attachments, transcodes it to 48kHz Opus for voice-note targets, and returns PCM directly for Talk/telephony.
- **Gradium**: WAV for audio attachments, Opus for voice-note targets, and `ulaw_8000` at 8 kHz for telephony.
- **Inworld**: MP3 for normal audio attachments, native `OGG_OPUS` for voice-note targets, and raw `PCM` at 22050 Hz for Talk/telephony.
- **xAI**: MP3 by default; `responseFormat` may be `mp3`, `wav`, `pcm`, `mulaw`, or `alaw`. OpenClaw uses xAI’s batch REST TTS endpoint and returns a complete audio attachment; xAI’s streaming TTS WebSocket is not used by this provider path. Native Opus voice-note format is not supported by this path.
- **Microsoft**: uses `microsoft.outputFormat` (default `audio-24khz-48kbitrate-mono-mp3`).

  - The bundled transport accepts an `outputFormat`, but not all formats are available from the service.
  - Output format values follow Microsoft Speech output formats (including Ogg/WebM Opus).
  - Telegram `sendVoice` accepts OGG/MP3/M4A; use OpenAI/ElevenLabs if you need
    guaranteed Opus voice messages.
  - If the configured Microsoft output format fails, OpenClaw retries with MP3.

OpenAI/ElevenLabs output formats are fixed per channel (see above).

## [​](https://docs.openclaw.ai/tools/tts\#auto-tts-behavior)  Auto-TTS behavior

When `messages.tts.auto` is enabled, OpenClaw:

- Skips TTS if the reply already contains media or a `MEDIA:` directive.
- Skips very short replies (under 10 chars).
- Summarizes long replies when summaries are enabled, using
`summaryModel` (or `agents.defaults.model.primary`).
- Attaches the generated audio to the reply.
- In `mode: "final"`, still sends audio-only TTS for streamed final replies
after the text stream completes; the generated media goes through the same
channel media normalization as normal reply attachments.

If the reply exceeds `maxLength` and summary is off (or no API key for the
summary model), audio is skipped and the normal text reply is sent.

```
Reply -> TTS enabled?
  no  -> send text
  yes -> has media / MEDIA: / short?
          yes -> send text
          no  -> length > limit?
                   no  -> TTS -> attach audio
                   yes -> summary enabled?
                            no  -> send text
                            yes -> summarize -> TTS -> attach audio
```

## [​](https://docs.openclaw.ai/tools/tts\#output-formats-by-channel)  Output formats by channel

| Target | Format |
| --- | --- |
| Feishu / Matrix / Telegram / WhatsApp | Voice-note replies prefer **Opus** (`opus_48000_64` from ElevenLabs, `opus` from OpenAI). 48 kHz / 64 kbps balances clarity and size. |
| Other channels | **MP3** (`mp3_44100_128` from ElevenLabs, `mp3` from OpenAI). 44.1 kHz / 128 kbps default for speech. |
| Talk / telephony | Provider-native **PCM** (Inworld 22050 Hz, Google 24 kHz), or `ulaw_8000` from Gradium for telephony. |

Per-provider notes:

- **Feishu / WhatsApp transcoding:** When a voice-note reply lands as MP3/WebM/WAV/M4A, the channel plugin transcodes to 48 kHz Ogg/Opus with `ffmpeg`. WhatsApp sends through Baileys with `ptt: true` and `audio/ogg; codecs=opus`. If conversion fails: Feishu falls back to attaching the original file; WhatsApp send fails rather than posting an incompatible PTT payload.
- **MiniMax / Xiaomi MiMo:** Default MP3 (32 kHz for MiniMax `speech-2.8-hd`); transcoded to 48 kHz Opus for voice-note targets via `ffmpeg`.
- **Local CLI:** Uses configured `outputFormat`. Voice-note targets are converted to Ogg/Opus and telephony output to raw 16 kHz mono PCM.
- **Google Gemini:** Returns raw 24 kHz PCM. OpenClaw wraps as WAV for attachments, transcodes to 48 kHz Opus for voice-note targets, returns PCM directly for Talk/telephony.
- **Inworld:** MP3 attachments, native `OGG_OPUS` voice-note, raw `PCM` 22050 Hz for Talk/telephony.
- **xAI:** MP3 by default; `responseFormat` may be `mp3|wav|pcm|mulaw|alaw`. Uses xAI’s batch REST endpoint — streaming WebSocket TTS is **not** used. Native Opus voice-note format is **not** supported.
- **Microsoft:** Uses `microsoft.outputFormat` (default `audio-24khz-48kbitrate-mono-mp3`). Telegram `sendVoice` accepts OGG/MP3/M4A; use OpenAI/ElevenLabs if you need guaranteed Opus voice messages. If the configured Microsoft format fails, OpenClaw retries with MP3.

OpenAI and ElevenLabs output formats are fixed per channel as listed above.

## [​](https://docs.openclaw.ai/tools/tts\#field-reference)  Field reference

Top-level messages.tts.\*

[​](https://docs.openclaw.ai/tools/tts#param-auto)

auto

"off" \| "always" \| "inbound" \| "tagged"

Auto-TTS mode. `inbound` only sends audio after an inbound voice message; `tagged` only sends audio when the reply includes `[[tts:...]]` directives or a `[[tts:text]]` block.

[​](https://docs.openclaw.ai/tools/tts#param-enabled)

enabled

boolean

deprecated

Legacy toggle. `openclaw doctor --fix` migrates this to `auto`.

[​](https://docs.openclaw.ai/tools/tts#param-mode)

mode

"final" \| "all"

default:"final"

`"all"` includes tool/block replies in addition to final replies.

[​](https://docs.openclaw.ai/tools/tts#param-provider)

provider

string

Speech provider id. When unset, OpenClaw uses the first configured provider in registry auto-select order. Legacy `provider: "edge"` is rewritten to `"microsoft"` by `openclaw doctor --fix`.

[​](https://docs.openclaw.ai/tools/tts#param-persona)

persona

string

Active persona id from `personas`. Normalized to lowercase.

[​](https://docs.openclaw.ai/tools/tts#param-personas-id)

personas.<id>

object

Stable spoken identity. Fields: `label`, `description`, `provider`, `fallbackPolicy`, `prompt`, `providers.<provider>`. See [Personas](https://docs.openclaw.ai/tools/tts#personas).

[​](https://docs.openclaw.ai/tools/tts#param-summary-model)

summaryModel

string

Cheap model for auto-summary; defaults to `agents.defaults.model.primary`. Accepts `provider/model` or a configured model alias.

[​](https://docs.openclaw.ai/tools/tts#param-model-overrides)

modelOverrides

object

Allow the model to emit TTS directives. `enabled` defaults to `true`; `allowProvider` defaults to `false`.

[​](https://docs.openclaw.ai/tools/tts#param-providers-id)

providers.<id>

object

Provider-owned settings keyed by speech provider id. Legacy direct blocks (`messages.tts.openai`, `.elevenlabs`, `.microsoft`, `.edge`) are rewritten by `openclaw doctor --fix`; commit only `messages.tts.providers.<id>`.

[​](https://docs.openclaw.ai/tools/tts#param-max-text-length)

maxTextLength

number

Hard cap for TTS input characters. `/tts audio` fails if exceeded.

[​](https://docs.openclaw.ai/tools/tts#param-timeout-ms)

timeoutMs

number

Request timeout in milliseconds.

[​](https://docs.openclaw.ai/tools/tts#param-prefs-path)

prefsPath

string

Override the local prefs JSON path (provider/limit/summary). Default `~/.openclaw/settings/tts.json`.

Azure Speech

[​](https://docs.openclaw.ai/tools/tts#param-api-key)

apiKey

string

Env: `AZURE_SPEECH_KEY`, `AZURE_SPEECH_API_KEY`, or `SPEECH_KEY`.

[​](https://docs.openclaw.ai/tools/tts#param-region)

region

string

Azure Speech region (e.g. `eastus`). Env: `AZURE_SPEECH_REGION` or `SPEECH_REGION`.

[​](https://docs.openclaw.ai/tools/tts#param-endpoint)

endpoint

string

Optional Azure Speech endpoint override (alias `baseUrl`).

[​](https://docs.openclaw.ai/tools/tts#param-voice)

voice

string

Azure voice ShortName. Default `en-US-JennyNeural`.

[​](https://docs.openclaw.ai/tools/tts#param-lang)

lang

string

SSML language code. Default `en-US`.

[​](https://docs.openclaw.ai/tools/tts#param-output-format)

outputFormat

string

Azure `X-Microsoft-OutputFormat` for standard audio. Default `audio-24khz-48kbitrate-mono-mp3`.

[​](https://docs.openclaw.ai/tools/tts#param-voice-note-output-format)

voiceNoteOutputFormat

string

Azure `X-Microsoft-OutputFormat` for voice-note output. Default `ogg-24khz-16bit-mono-opus`.

ElevenLabs

[​](https://docs.openclaw.ai/tools/tts#param-api-key-1)

apiKey

string

Falls back to `ELEVENLABS_API_KEY` or `XI_API_KEY`.

[​](https://docs.openclaw.ai/tools/tts#param-model)

model

string

Model id (e.g. `eleven_multilingual_v2`, `eleven_v3`).

[​](https://docs.openclaw.ai/tools/tts#param-voice-id)

voiceId

string

ElevenLabs voice id.

[​](https://docs.openclaw.ai/tools/tts#param-voice-settings)

voiceSettings

object

`stability`, `similarityBoost`, `style` (each `0..1`), `useSpeakerBoost` (`true|false`), `speed` (`0.5..2.0`, `1.0` = normal).

[​](https://docs.openclaw.ai/tools/tts#param-apply-text-normalization)

applyTextNormalization

"auto" \| "on" \| "off"

Text normalization mode.

[​](https://docs.openclaw.ai/tools/tts#param-language-code)

languageCode

string

2-letter ISO 639-1 (e.g. `en`, `de`).

[​](https://docs.openclaw.ai/tools/tts#param-seed)

seed

number

Integer `0..4294967295` for best-effort determinism.

[​](https://docs.openclaw.ai/tools/tts#param-base-url)

baseUrl

string

Override ElevenLabs API base URL.

Google Gemini

[​](https://docs.openclaw.ai/tools/tts#param-api-key-2)

apiKey

string

Falls back to `GEMINI_API_KEY` / `GOOGLE_API_KEY`. If omitted, TTS can reuse `models.providers.google.apiKey` before env fallback.

[​](https://docs.openclaw.ai/tools/tts#param-model-1)

model

string

Gemini TTS model. Default `gemini-3.1-flash-tts-preview`.

[​](https://docs.openclaw.ai/tools/tts#param-voice-name)

voiceName

string

Gemini prebuilt voice name. Default `Kore`. Alias: `voice`.

[​](https://docs.openclaw.ai/tools/tts#param-audio-profile)

audioProfile

string

Natural-language style prompt prepended before spoken text.

[​](https://docs.openclaw.ai/tools/tts#param-speaker-name)

speakerName

string

Optional speaker label prepended before spoken text when your prompt uses a named speaker.

[​](https://docs.openclaw.ai/tools/tts#param-prompt-template)

promptTemplate

"audio-profile-v1"

Set to `audio-profile-v1` to wrap active persona prompt fields in a deterministic Gemini TTS prompt structure.

[​](https://docs.openclaw.ai/tools/tts#param-persona-prompt)

personaPrompt

string

Google-specific extra persona prompt text appended to the template’s Director’s Notes.

[​](https://docs.openclaw.ai/tools/tts#param-base-url-1)

baseUrl

string

Only `https://generativelanguage.googleapis.com` is accepted.

Gradium

[​](https://docs.openclaw.ai/tools/tts#param-api-key-3)

apiKey

string

Env: `GRADIUM_API_KEY`.

[​](https://docs.openclaw.ai/tools/tts#param-base-url-2)

baseUrl

string

Default `https://api.gradium.ai`.

[​](https://docs.openclaw.ai/tools/tts#param-voice-id-1)

voiceId

string

Default Emma (`YTpq7expH9539ERJ`).

Inworld

[​](https://docs.openclaw.ai/tools/tts#param-api-key-4)

apiKey

string

Env: `INWORLD_API_KEY`.

[​](https://docs.openclaw.ai/tools/tts#param-base-url-3)

baseUrl

string

Default `https://api.inworld.ai`.

[​](https://docs.openclaw.ai/tools/tts#param-model-id)

modelId

string

Default `inworld-tts-1.5-max`. Also: `inworld-tts-1.5-mini`, `inworld-tts-1-max`, `inworld-tts-1`.

[​](https://docs.openclaw.ai/tools/tts#param-voice-id-2)

voiceId

string

Default `Sarah`.

[​](https://docs.openclaw.ai/tools/tts#param-temperature)

temperature

number

Sampling temperature `0..2`.

Local CLI (tts-local-cli)

[​](https://docs.openclaw.ai/tools/tts#param-command)

command

string

Local executable or command string for CLI TTS.

[​](https://docs.openclaw.ai/tools/tts#param-args)

args

string\[\]

Command arguments. Supports `{{Text}}`, `{{OutputPath}}`, `{{OutputDir}}`, `{{OutputBase}}` placeholders.

[​](https://docs.openclaw.ai/tools/tts#param-output-format-1)

outputFormat

"mp3" \| "opus" \| "wav"

Expected CLI output format. Default `mp3` for audio attachments.

[​](https://docs.openclaw.ai/tools/tts#param-timeout-ms-1)

timeoutMs

number

Command timeout in milliseconds. Default `120000`.

[​](https://docs.openclaw.ai/tools/tts#param-cwd)

cwd

string

Optional command working directory.

[​](https://docs.openclaw.ai/tools/tts#param-env)

env

Record<string, string>

Optional environment overrides for the command.

Microsoft (no API key)

[​](https://docs.openclaw.ai/tools/tts#param-enabled-1)

enabled

boolean

default:"true"

Allow Microsoft speech usage.

[​](https://docs.openclaw.ai/tools/tts#param-voice-1)

voice

string

Microsoft neural voice name (e.g. `en-US-MichelleNeural`).

[​](https://docs.openclaw.ai/tools/tts#param-lang-1)

lang

string

Language code (e.g. `en-US`).

[​](https://docs.openclaw.ai/tools/tts#param-output-format-2)

outputFormat

string

Microsoft output format. Default `audio-24khz-48kbitrate-mono-mp3`. Not all formats are supported by the bundled Edge-backed transport.

[​](https://docs.openclaw.ai/tools/tts#param-rate-pitch-volume)

rate / pitch / volume

string

Percent strings (e.g. `+10%`, `-5%`).

[​](https://docs.openclaw.ai/tools/tts#param-save-subtitles)

saveSubtitles

boolean

Write JSON subtitles alongside the audio file.

[​](https://docs.openclaw.ai/tools/tts#param-proxy)

proxy

string

Proxy URL for Microsoft speech requests.

[​](https://docs.openclaw.ai/tools/tts#param-timeout-ms-2)

timeoutMs

number

Request timeout override (ms).

[​](https://docs.openclaw.ai/tools/tts#param-edge)

edge.\*

object

deprecated

Legacy alias. Run `openclaw doctor --fix` to rewrite persisted config to `providers.microsoft`.

MiniMax

[​](https://docs.openclaw.ai/tools/tts#param-api-key-5)

apiKey

string

Falls back to `MINIMAX_API_KEY`. Token Plan auth via `MINIMAX_OAUTH_TOKEN`, `MINIMAX_CODE_PLAN_KEY`, or `MINIMAX_CODING_API_KEY`.

[​](https://docs.openclaw.ai/tools/tts#param-base-url-4)

baseUrl

string

Default `https://api.minimax.io`. Env: `MINIMAX_API_HOST`.

[​](https://docs.openclaw.ai/tools/tts#param-model-2)

model

string

Default `speech-2.8-hd`. Env: `MINIMAX_TTS_MODEL`.

[​](https://docs.openclaw.ai/tools/tts#param-voice-id-3)

voiceId

string

Default `English_expressive_narrator`. Env: `MINIMAX_TTS_VOICE_ID`.

[​](https://docs.openclaw.ai/tools/tts#param-speed)

speed

number

`0.5..2.0`. Default `1.0`.

[​](https://docs.openclaw.ai/tools/tts#param-vol)

vol

number

`(0, 10]`. Default `1.0`.

[​](https://docs.openclaw.ai/tools/tts#param-pitch)

pitch

number

Integer `-12..12`. Default `0`. Fractional values are truncated before the request.

OpenAI

[​](https://docs.openclaw.ai/tools/tts#param-api-key-6)

apiKey

string

Falls back to `OPENAI_API_KEY`.

[​](https://docs.openclaw.ai/tools/tts#param-model-3)

model

string

OpenAI TTS model id (e.g. `gpt-4o-mini-tts`).

[​](https://docs.openclaw.ai/tools/tts#param-voice-2)

voice

string

Voice name (e.g. `alloy`, `cedar`).

[​](https://docs.openclaw.ai/tools/tts#param-instructions)

instructions

string

Explicit OpenAI `instructions` field. When set, persona prompt fields are **not** auto-mapped.

[​](https://docs.openclaw.ai/tools/tts#param-extra-body-extra-body)

extraBody / extra\_body

Record<string, unknown>

Extra JSON fields merged into `/audio/speech` request bodies after generated OpenAI TTS fields. Use this for OpenAI-compatible endpoints such as Kokoro that require provider-specific keys like `lang`; unsafe prototype keys are ignored.

[​](https://docs.openclaw.ai/tools/tts#param-base-url-5)

baseUrl

string

Override the OpenAI TTS endpoint. Resolution order: config → `OPENAI_TTS_BASE_URL` → `https://api.openai.com/v1`. Non-default values are treated as OpenAI-compatible TTS endpoints, so custom model and voice names are accepted.

OpenRouter

[​](https://docs.openclaw.ai/tools/tts#param-api-key-7)

apiKey

string

Env: `OPENROUTER_API_KEY`. Can reuse `models.providers.openrouter.apiKey`.

[​](https://docs.openclaw.ai/tools/tts#param-base-url-6)

baseUrl

string

Default `https://openrouter.ai/api/v1`. Legacy `https://openrouter.ai/v1` is normalized.

[​](https://docs.openclaw.ai/tools/tts#param-model-4)

model

string

Default `hexgrad/kokoro-82m`. Alias: `modelId`.

[​](https://docs.openclaw.ai/tools/tts#param-voice-3)

voice

string

Default `af_alloy`. Alias: `voiceId`.

[​](https://docs.openclaw.ai/tools/tts#param-response-format)

responseFormat

"mp3" \| "pcm"

Default `mp3`.

[​](https://docs.openclaw.ai/tools/tts#param-speed-1)

speed

number

Provider-native speed override.

Volcengine (BytePlus Seed Speech)

[​](https://docs.openclaw.ai/tools/tts#param-api-key-8)

apiKey

string

Env: `VOLCENGINE_TTS_API_KEY` or `BYTEPLUS_SEED_SPEECH_API_KEY`.

[​](https://docs.openclaw.ai/tools/tts#param-resource-id)

resourceId

string

Default `seed-tts-1.0`. Env: `VOLCENGINE_TTS_RESOURCE_ID`. Use `seed-tts-2.0` when your project has TTS 2.0 entitlement.

[​](https://docs.openclaw.ai/tools/tts#param-app-key)

appKey

string

App key header. Default `aGjiRDfUWi`. Env: `VOLCENGINE_TTS_APP_KEY`.

[​](https://docs.openclaw.ai/tools/tts#param-base-url-7)

baseUrl

string

Override the Seed Speech TTS HTTP endpoint. Env: `VOLCENGINE_TTS_BASE_URL`.

[​](https://docs.openclaw.ai/tools/tts#param-voice-4)

voice

string

Voice type. Default `en_female_anna_mars_bigtts`. Env: `VOLCENGINE_TTS_VOICE`.

[​](https://docs.openclaw.ai/tools/tts#param-speed-ratio)

speedRatio

number

Provider-native speed ratio.

[​](https://docs.openclaw.ai/tools/tts#param-emotion)

emotion

string

Provider-native emotion tag.

[​](https://docs.openclaw.ai/tools/tts#param-app-id-token-cluster)

appId / token / cluster

string

deprecated

Legacy Volcengine Speech Console fields. Env: `VOLCENGINE_TTS_APPID`, `VOLCENGINE_TTS_TOKEN`, `VOLCENGINE_TTS_CLUSTER` (default `volcano_tts`).

xAI

[​](https://docs.openclaw.ai/tools/tts#param-api-key-9)

apiKey

string

Env: `XAI_API_KEY`.

[​](https://docs.openclaw.ai/tools/tts#param-base-url-8)

baseUrl

string

Default `https://api.x.ai/v1`. Env: `XAI_BASE_URL`.

[​](https://docs.openclaw.ai/tools/tts#param-voice-id-4)

voiceId

string

Default `eve`. Live voices: `ara`, `eve`, `leo`, `rex`, `sal`, `una`.

[​](https://docs.openclaw.ai/tools/tts#param-language)

language

string

BCP-47 language code or `auto`. Default `en`.

[​](https://docs.openclaw.ai/tools/tts#param-response-format-1)

responseFormat

"mp3" \| "wav" \| "pcm" \| "mulaw" \| "alaw"

Default `mp3`.

[​](https://docs.openclaw.ai/tools/tts#param-speed-2)

speed

number

Provider-native speed override.

Xiaomi MiMo

[​](https://docs.openclaw.ai/tools/tts#param-api-key-10)

apiKey

string

Env: `XIAOMI_API_KEY`.

[​](https://docs.openclaw.ai/tools/tts#param-base-url-9)

baseUrl

string

Default `https://api.xiaomimimo.com/v1`. Env: `XIAOMI_BASE_URL`.

[​](https://docs.openclaw.ai/tools/tts#param-model-5)

model

string

Default `mimo-v2.5-tts`. Env: `XIAOMI_TTS_MODEL`. Also supports `mimo-v2-tts`.

[​](https://docs.openclaw.ai/tools/tts#param-voice-5)

voice

string

Default `mimo_default`. Env: `XIAOMI_TTS_VOICE`.

[​](https://docs.openclaw.ai/tools/tts#param-format)

format

"mp3" \| "wav"

Default `mp3`. Env: `XIAOMI_TTS_FORMAT`.

[​](https://docs.openclaw.ai/tools/tts#param-style)

style

string

Optional natural-language style instruction sent as the user message; not spoken.

## [​](https://docs.openclaw.ai/tools/tts\#agent-tool)  Agent tool

The `tts` tool converts text to speech and returns an audio attachment for
reply delivery. On Feishu, Matrix, Telegram, and WhatsApp, the audio is
delivered as a voice message rather than a file attachment. Feishu and
WhatsApp can transcode non-Opus TTS output on this path when `ffmpeg` is
available.WhatsApp sends audio through Baileys as a PTT voice note (`audio` with
`ptt: true`) and sends visible text **separately** from PTT audio because
clients do not consistently render captions on voice notes.The tool accepts optional `channel` and `timeoutMs` fields; `timeoutMs` is a
per-call provider request timeout in milliseconds.

## [​](https://docs.openclaw.ai/tools/tts\#gateway-rpc)  Gateway RPC

| Method | Purpose |
| --- | --- |
| `tts.status` | Read current TTS state and last attempt. |
| `tts.enable` | Set local auto preference to `always`. |
| `tts.disable` | Set local auto preference to `off`. |
| `tts.convert` | One-off text → audio. |
| `tts.setProvider` | Set local provider preference. |
| `tts.setPersona` | Set local persona preference. |
| `tts.providers` | List configured providers and status. |

## [​](https://docs.openclaw.ai/tools/tts\#service-links)  Service links

- [OpenAI text-to-speech guide](https://platform.openai.com/docs/guides/text-to-speech)
- [OpenAI Audio API reference](https://platform.openai.com/docs/api-reference/audio)
- [Azure Speech REST text-to-speech](https://learn.microsoft.com/azure/ai-services/speech-service/rest-text-to-speech)
- [Azure Speech provider](https://docs.openclaw.ai/providers/azure-speech)
- [ElevenLabs Text to Speech](https://elevenlabs.io/docs/api-reference/text-to-speech)
- [ElevenLabs Authentication](https://elevenlabs.io/docs/api-reference/authentication)
- [Gradium](https://docs.openclaw.ai/providers/gradium)
- [Inworld TTS API](https://docs.inworld.ai/tts/tts)
- [MiniMax T2A v2 API](https://platform.minimaxi.com/document/T2A%20V2)
- [Volcengine TTS HTTP API](https://docs.openclaw.ai/providers/volcengine#text-to-speech)
- [Xiaomi MiMo speech synthesis](https://docs.openclaw.ai/providers/xiaomi#text-to-speech)
- [node-edge-tts](https://github.com/SchneeHertz/node-edge-tts)
- [Microsoft Speech output formats](https://learn.microsoft.com/azure/ai-services/speech-service/rest-text-to-speech#audio-outputs)
- [xAI text to speech](https://docs.x.ai/developers/rest-api-reference/inference/voice#text-to-speech-rest)

## [​](https://docs.openclaw.ai/tools/tts\#related)  Related

- [Media overview](https://docs.openclaw.ai/tools/media-overview)
- [Music generation](https://docs.openclaw.ai/tools/music-generation)
- [Video generation](https://docs.openclaw.ai/tools/video-generation)
- [Slash commands](https://docs.openclaw.ai/tools/slash-commands)
- [Voice call plugin](https://docs.openclaw.ai/plugins/voice-call)

[Trajectory bundles](https://docs.openclaw.ai/tools/trajectory) [Video generation](https://docs.openclaw.ai/tools/video-generation)

Ctrl+I