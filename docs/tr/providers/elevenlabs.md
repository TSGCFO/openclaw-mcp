---
source_url: https://docs.openclaw.ai/tr/providers/elevenlabs
title: "ElevenLabs - OpenClaw"
---

[Ana içeriğe atla](https://docs.openclaw.ai/tr/providers/elevenlabs#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/tr)

![TR](https://d3gk2c5xim1je2.cloudfront.net/flags/TR.svg)

Türkçe

Ara...

Ctrl K

Ara...

Navigation

Providers

ElevenLabs

[Get started](https://docs.openclaw.ai/tr) [Install](https://docs.openclaw.ai/tr/install) [Channels](https://docs.openclaw.ai/tr/channels) [Agents](https://docs.openclaw.ai/tr/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tr/tools) [Models](https://docs.openclaw.ai/tr/providers) [Platforms](https://docs.openclaw.ai/tr/platforms) [Gateway & Ops](https://docs.openclaw.ai/tr/gateway) [Reference](https://docs.openclaw.ai/tr/cli) [Help](https://docs.openclaw.ai/tr/help)

Bu sayfada

- [Kimlik doğrulama](https://docs.openclaw.ai/tr/providers/elevenlabs#kimlik-do%C4%9Frulama)
- [Metinden konuşmaya](https://docs.openclaw.ai/tr/providers/elevenlabs#metinden-konu%C5%9Fmaya)
- [Speech-to-text](https://docs.openclaw.ai/tr/providers/elevenlabs#speech-to-text)
- [Voice Call akışlı STT](https://docs.openclaw.ai/tr/providers/elevenlabs#voice-call-ak%C4%B1%C5%9Fl%C4%B1-stt)
- [İlgili](https://docs.openclaw.ai/tr/providers/elevenlabs#i%CC%87lgili)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw, metinden konuşmaya, Scribe
v2 ile toplu speech-to-text ve Voice Call akışlı STT için Scribe v2 Realtime amacıyla ElevenLabs kullanır.

| Yetenek | OpenClaw yüzeyi | Varsayılan |
| --- | --- | --- |
| Metinden konuşmaya | `messages.tts` / `talk` | `eleven_multilingual_v2` |
| Toplu speech-to-text | `tools.media.audio` | `scribe_v2` |
| Akışlı speech-to-text | Voice Call `streaming.provider: "elevenlabs"` | `scribe_v2_realtime` |

## [​](https://docs.openclaw.ai/tr/providers/elevenlabs\#kimlik-do%C4%9Frulama)  Kimlik doğrulama

Ortamda `ELEVENLABS_API_KEY` ayarlayın. Mevcut
ElevenLabs araçlarıyla uyumluluk için `XI_API_KEY` de kabul edilir.

```
export ELEVENLABS_API_KEY="..."
```

## [​](https://docs.openclaw.ai/tr/providers/elevenlabs\#metinden-konu%C5%9Fmaya)  Metinden konuşmaya

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

ElevenLabs v3 TTS kullanmak için `modelId` değerini `eleven_v3` olarak ayarlayın. OpenClaw
mevcut kurulumlar için varsayılan olarak `eleven_multilingual_v2` değerini korur.

## [​](https://docs.openclaw.ai/tr/providers/elevenlabs\#speech-to-text)  Speech-to-text

Gelen ses ekleri ve kısa kaydedilmiş ses segmentleri için Scribe v2 kullanın:

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

OpenClaw, çok parçalı sesi ElevenLabs `/v1/speech-to-text` uç noktasına
`model_id: "scribe_v2"` ile gönderir. Dil ipuçları mevcut olduğunda `language_code` alanına eşlenir.

## [​](https://docs.openclaw.ai/tr/providers/elevenlabs\#voice-call-ak%C4%B1%C5%9Fl%C4%B1-stt)  Voice Call akışlı STT

Paketlenmiş `elevenlabs` Plugin’i, Voice Call
akışlı transcription için Scribe v2 Realtime’ı kaydeder.

| Ayar | Yapılandırma yolu | Varsayılan |
| --- | --- | --- |
| API anahtarı | `plugins.entries.voice-call.config.streaming.providers.elevenlabs.apiKey` | `ELEVENLABS_API_KEY` / `XI_API_KEY` değerine geri düşer |
| Model | `...elevenlabs.modelId` | `scribe_v2_realtime` |
| Ses biçimi | `...elevenlabs.audioFormat` | `ulaw_8000` |
| Örnekleme oranı | `...elevenlabs.sampleRate` | `8000` |
| Commit stratejisi | `...elevenlabs.commitStrategy` | `vad` |
| Dil | `...elevenlabs.languageCode` | (ayarlanmamış) |

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

Voice Call, Twilio medyasını 8 kHz G.711 u-law olarak alır. ElevenLabs realtime
sağlayıcısı varsayılan olarak `ulaw_8000` kullandığı için telefon çerçeveleri
yeniden kodlama olmadan iletilebilir.

## [​](https://docs.openclaw.ai/tr/providers/elevenlabs\#i%CC%87lgili)  İlgili

- [Metinden konuşmaya](https://docs.openclaw.ai/tr/tools/tts)
- [Model seçimi](https://docs.openclaw.ai/tr/concepts/model-providers)

[DeepSeek](https://docs.openclaw.ai/tr/providers/deepseek) [Fal](https://docs.openclaw.ai/tr/providers/fal)

Ctrl+I