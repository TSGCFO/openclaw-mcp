---
source_url: https://docs.openclaw.ai/uk/providers/elevenlabs
title: "ElevenLabs - OpenClaw"
---

[Перейти до основного вмісту](https://docs.openclaw.ai/uk/providers/elevenlabs#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/uk)

![UA](https://d3gk2c5xim1je2.cloudfront.net/flags/UA.svg)

Українська

Пошук...

Ctrl K

Пошук...

Navigation

Providers

ElevenLabs

[Get started](https://docs.openclaw.ai/uk) [Install](https://docs.openclaw.ai/uk/install) [Channels](https://docs.openclaw.ai/uk/channels) [Agents](https://docs.openclaw.ai/uk/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/uk/tools) [Models](https://docs.openclaw.ai/uk/providers) [Platforms](https://docs.openclaw.ai/uk/platforms) [Gateway & Ops](https://docs.openclaw.ai/uk/gateway) [Reference](https://docs.openclaw.ai/uk/cli) [Help](https://docs.openclaw.ai/uk/help)

На цій сторінці

- [Автентифікація](https://docs.openclaw.ai/uk/providers/elevenlabs#%D0%B0%D0%B2%D1%82%D0%B5%D0%BD%D1%82%D0%B8%D1%84%D1%96%D0%BA%D0%B0%D1%86%D1%96%D1%8F)
- [Перетворення тексту на мовлення](https://docs.openclaw.ai/uk/providers/elevenlabs#%D0%BF%D0%B5%D1%80%D0%B5%D1%82%D0%B2%D0%BE%D1%80%D0%B5%D0%BD%D0%BD%D1%8F-%D1%82%D0%B5%D0%BA%D1%81%D1%82%D1%83-%D0%BD%D0%B0-%D0%BC%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D1%8F)
- [Перетворення мовлення на текст](https://docs.openclaw.ai/uk/providers/elevenlabs#%D0%BF%D0%B5%D1%80%D0%B5%D1%82%D0%B2%D0%BE%D1%80%D0%B5%D0%BD%D0%BD%D1%8F-%D0%BC%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D1%8F-%D0%BD%D0%B0-%D1%82%D0%B5%D0%BA%D1%81%D1%82)
- [Потокове STT Voice Call](https://docs.openclaw.ai/uk/providers/elevenlabs#%D0%BF%D0%BE%D1%82%D0%BE%D0%BA%D0%BE%D0%B2%D0%B5-stt-voice-call)
- [Пов’язане](https://docs.openclaw.ai/uk/providers/elevenlabs#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw використовує ElevenLabs для перетворення тексту на мовлення, пакетного перетворення мовлення на текст із Scribe
v2 і потокового STT Voice Call із Scribe v2 Realtime.

| Можливість | Поверхня OpenClaw | За замовчуванням |
| --- | --- | --- |
| Перетворення тексту на мовлення | `messages.tts` / `talk` | `eleven_multilingual_v2` |
| Пакетне перетворення мовлення на текст | `tools.media.audio` | `scribe_v2` |
| Потокове перетворення мовлення на текст | Voice Call `streaming.provider: "elevenlabs"` | `scribe_v2_realtime` |

## [​](https://docs.openclaw.ai/uk/providers/elevenlabs\#%D0%B0%D0%B2%D1%82%D0%B5%D0%BD%D1%82%D0%B8%D1%84%D1%96%D0%BA%D0%B0%D1%86%D1%96%D1%8F)  Автентифікація

Установіть `ELEVENLABS_API_KEY` у середовищі. `XI_API_KEY` також підтримується для
сумісності з наявними інструментами ElevenLabs.

```
export ELEVENLABS_API_KEY="..."
```

## [​](https://docs.openclaw.ai/uk/providers/elevenlabs\#%D0%BF%D0%B5%D1%80%D0%B5%D1%82%D0%B2%D0%BE%D1%80%D0%B5%D0%BD%D0%BD%D1%8F-%D1%82%D0%B5%D0%BA%D1%81%D1%82%D1%83-%D0%BD%D0%B0-%D0%BC%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D1%8F)  Перетворення тексту на мовлення

```
{
  messages: {
    tts: {
      providers: {
        elevenlabs: {
          apiKey: "${ELEVENLABS_API_KEY}",
          voiceId: "pMsXgVXv3BLzUgSXRplE",
          modelId: "eleven_multilingual_v2",
        },
      },
    },
  },
}
```

Установіть `modelId` на `eleven_v3`, щоб використовувати ElevenLabs v3 TTS. OpenClaw зберігає
`eleven_multilingual_v2` як значення за замовчуванням для наявних інсталяцій.

## [​](https://docs.openclaw.ai/uk/providers/elevenlabs\#%D0%BF%D0%B5%D1%80%D0%B5%D1%82%D0%B2%D0%BE%D1%80%D0%B5%D0%BD%D0%BD%D1%8F-%D0%BC%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%BD%D1%8F-%D0%BD%D0%B0-%D1%82%D0%B5%D0%BA%D1%81%D1%82)  Перетворення мовлення на текст

Використовуйте Scribe v2 для вхідних аудіовкладень і коротких записаних голосових сегментів:

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "elevenlabs", model: "scribe_v2" }],
      },
    },
  },
}
```

OpenClaw надсилає multipart-аудіо до ElevenLabs `/v1/speech-to-text` з
`model_id: "scribe_v2"`. Підказки мови зіставляються з `language_code`, якщо вони задані.

## [​](https://docs.openclaw.ai/uk/providers/elevenlabs\#%D0%BF%D0%BE%D1%82%D0%BE%D0%BA%D0%BE%D0%B2%D0%B5-stt-voice-call)  Потокове STT Voice Call

Вбудований Plugin `elevenlabs` реєструє Scribe v2 Realtime для потокової
транскрипції Voice Call.

| Налаштування | Шлях конфігурації | За замовчуванням |
| --- | --- | --- |
| API-ключ | `plugins.entries.voice-call.config.streaming.providers.elevenlabs.apiKey` | Резервно використовує `ELEVENLABS_API_KEY` / `XI_API_KEY` |
| Модель | `...elevenlabs.modelId` | `scribe_v2_realtime` |
| Формат аудіо | `...elevenlabs.audioFormat` | `ulaw_8000` |
| Частота дискретизації | `...elevenlabs.sampleRate` | `8000` |
| Стратегія коміту | `...elevenlabs.commitStrategy` | `vad` |
| Мова | `...elevenlabs.languageCode` | (не задано) |

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          streaming: {
            enabled: true,
            provider: "elevenlabs",
            providers: {
              elevenlabs: {
                apiKey: "${ELEVENLABS_API_KEY}",
                audioFormat: "ulaw_8000",
                commitStrategy: "vad",
                languageCode: "en",
              },
            },
          },
        },
      },
    },
  },
}
```

Voice Call отримує медіапотік Twilio як 8 кГц G.711 u-law. Провайдер ElevenLabs realtime
за замовчуванням використовує `ulaw_8000`, тому телекомунікаційні фрейми можна пересилати без
транскодування.

## [​](https://docs.openclaw.ai/uk/providers/elevenlabs\#%D0%BF%D0%BE%D0%B2%E2%80%99%D1%8F%D0%B7%D0%B0%D0%BD%D0%B5)  Пов’язане

- [Перетворення тексту на мовлення](https://docs.openclaw.ai/uk/tools/tts)
- [Вибір моделі](https://docs.openclaw.ai/uk/concepts/model-providers)

[DeepSeek](https://docs.openclaw.ai/uk/providers/deepseek) [Fal](https://docs.openclaw.ai/uk/providers/fal)

Ctrl+I