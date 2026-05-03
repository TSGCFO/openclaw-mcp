---
source_url: https://docs.openclaw.ai/pl/providers/mistral
title: "Mistral - OpenClaw"
---

[Przejdź do głównej treści](https://docs.openclaw.ai/pl/providers/mistral#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/pl)

![PL](https://d3gk2c5xim1je2.cloudfront.net/flags/PL.svg)

Polski

Szukaj...

Ctrl K

Szukaj...

Navigation

Providers

Mistral

[Get started](https://docs.openclaw.ai/pl) [Install](https://docs.openclaw.ai/pl/install) [Channels](https://docs.openclaw.ai/pl/channels) [Agents](https://docs.openclaw.ai/pl/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/pl/tools) [Models](https://docs.openclaw.ai/pl/providers) [Platforms](https://docs.openclaw.ai/pl/platforms) [Gateway & Ops](https://docs.openclaw.ai/pl/gateway) [Reference](https://docs.openclaw.ai/pl/cli) [Help](https://docs.openclaw.ai/pl/help)

Na tej stronie

- [Pierwsze kroki](https://docs.openclaw.ai/pl/providers/mistral#pierwsze-kroki)
- [Wbudowany katalog LLM](https://docs.openclaw.ai/pl/providers/mistral#wbudowany-katalog-llm)
- [Transkrypcja audio (Voxtral)](https://docs.openclaw.ai/pl/providers/mistral#transkrypcja-audio-voxtral)
- [Strumieniowy STT dla Voice Call](https://docs.openclaw.ai/pl/providers/mistral#strumieniowy-stt-dla-voice-call)
- [Konfiguracja zaawansowana](https://docs.openclaw.ai/pl/providers/mistral#konfiguracja-zaawansowana)
- [Powiązane](https://docs.openclaw.ai/pl/providers/mistral#powi%C4%85zane)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw obsługuje Mistral zarówno do routingu modeli tekstu/obrazu (`mistral/...`), jak i
transkrypcji audio przez Voxtral w rozumieniu multimediów.
Mistral może być także używany do osadzeń pamięci (`memorySearch.provider = "mistral"`).

- Dostawca: `mistral`
- Uwierzytelnianie: `MISTRAL_API_KEY`
- API: Mistral Chat Completions (`https://api.mistral.ai/v1`)

## [​](https://docs.openclaw.ai/pl/providers/mistral\#pierwsze-kroki)  Pierwsze kroki

1

[Navigate to header](https://docs.openclaw.ai/pl/providers/mistral#)

Uzyskaj klucz API

Utwórz klucz API w [konsoli Mistral](https://console.mistral.ai/).

2

[Navigate to header](https://docs.openclaw.ai/pl/providers/mistral#)

Uruchom onboarding

```
openclaw onboard --auth-choice mistral-api-key
```

Albo przekaż klucz bezpośrednio:

```
openclaw onboard --mistral-api-key "$MISTRAL_API_KEY"
```

3

[Navigate to header](https://docs.openclaw.ai/pl/providers/mistral#)

Ustaw model domyślny

```
{
  env: { MISTRAL_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "mistral/mistral-large-latest" } } },
}
```

4

[Navigate to header](https://docs.openclaw.ai/pl/providers/mistral#)

Sprawdź, czy model jest dostępny

```
openclaw models list --provider mistral
```

## [​](https://docs.openclaw.ai/pl/providers/mistral\#wbudowany-katalog-llm)  Wbudowany katalog LLM

OpenClaw obecnie dostarcza ten dołączony katalog Mistral:

| Ref modelu | Wejście | Kontekst | Maks. wyjście | Uwagi |
| --- | --- | --- | --- | --- |
| `mistral/mistral-large-latest` | tekst, obraz | 262,144 | 16,384 | Model domyślny |
| `mistral/mistral-medium-2508` | tekst, obraz | 262,144 | 8,192 | Mistral Medium 3.1 |
| `mistral/mistral-small-latest` | tekst, obraz | 128,000 | 16,384 | Mistral Small 4; regulowane rozumowanie przez API `reasoning_effort` |
| `mistral/pixtral-large-latest` | tekst, obraz | 128,000 | 32,768 | Pixtral |
| `mistral/codestral-latest` | tekst | 256,000 | 4,096 | Programowanie |
| `mistral/devstral-medium-latest` | tekst | 262,144 | 32,768 | Devstral 2 |
| `mistral/magistral-small` | tekst | 128,000 | 40,000 | Z włączonym rozumowaniem |

## [​](https://docs.openclaw.ai/pl/providers/mistral\#transkrypcja-audio-voxtral)  Transkrypcja audio (Voxtral)

Użyj Voxtral do wsadowej transkrypcji audio przez potok rozumienia
multimediów.

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "mistral", model: "voxtral-mini-latest" }],
      },
    },
  },
}
```

Ścieżka transkrypcji multimediów używa `/v1/audio/transcriptions`. Domyślny model audio dla Mistral to `voxtral-mini-latest`.

## [​](https://docs.openclaw.ai/pl/providers/mistral\#strumieniowy-stt-dla-voice-call)  Strumieniowy STT dla Voice Call

Dołączony Plugin `mistral` rejestruje Voxtral Realtime jako dostawcę
strumieniowego STT dla Voice Call.

| Ustawienie | Ścieżka konfiguracji | Domyślnie |
| --- | --- | --- |
| Klucz API | `plugins.entries.voice-call.config.streaming.providers.mistral.apiKey` | Wraca do `MISTRAL_API_KEY` |
| Model | `...mistral.model` | `voxtral-mini-transcribe-realtime-2602` |
| Kodowanie | `...mistral.encoding` | `pcm_mulaw` |
| Częstotliwość próbkowania | `...mistral.sampleRate` | `8000` |
| Opóźnienie docelowe | `...mistral.targetStreamingDelayMs` | `800` |

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          streaming: {
            enabled: true,
            provider: "mistral",
            providers: {
              mistral: {
                apiKey: "${MISTRAL_API_KEY}",
                targetStreamingDelayMs: 800,
              },
            },
          },
        },
      },
    },
  },
}
```

OpenClaw domyślnie ustawia Mistral realtime STT na `pcm_mulaw` przy 8 kHz, aby Voice Call
mógł przekazywać ramki multimediów Twilio bezpośrednio. Użyj `encoding: "pcm_s16le"` i
pasującego `sampleRate` tylko wtedy, gdy strumień nadrzędny jest już surowym PCM.

## [​](https://docs.openclaw.ai/pl/providers/mistral\#konfiguracja-zaawansowana)  Konfiguracja zaawansowana

Regulowane rozumowanie (mistral-small-latest)

`mistral/mistral-small-latest` mapuje się na Mistral Small 4 i obsługuje [regulowane rozumowanie](https://docs.mistral.ai/capabilities/reasoning/adjustable) w API Chat Completions przez `reasoning_effort` (`none` minimalizuje dodatkowe myślenie w wyjściu; `high` pokazuje pełne ślady myślenia przed końcową odpowiedzią).OpenClaw mapuje poziom **thinking** sesji na API Mistral:

| Poziom thinking w OpenClaw | Mistral `reasoning_effort` |
| --- | --- |
| **off** / **minimal** | `none` |
| **low** / **medium** / **high** / **xhigh** / **adaptive** / **max** | `high` |

Inne dołączone modele katalogu Mistral nie używają tego parametru. Nadal używaj modeli `magistral-*`, gdy chcesz natywnego zachowania Mistral nastawionego najpierw na rozumowanie.

Osadzenia pamięci

Mistral może udostępniać osadzenia pamięci przez `/v1/embeddings` (model domyślny: `mistral-embed`).

```
{
  memorySearch: { provider: "mistral" },
}
```

Uwierzytelnianie i bazowy URL

- Uwierzytelnianie Mistral używa `MISTRAL_API_KEY`.
- Bazowy URL dostawcy domyślnie to `https://api.mistral.ai/v1`.
- Domyślny model onboardingu to `mistral/mistral-large-latest`.
- Z.AI używa uwierzytelniania Bearer z Twoim kluczem API.

## [​](https://docs.openclaw.ai/pl/providers/mistral\#powi%C4%85zane)  Powiązane

[**Wybór modelu** \\
\\
Wybieranie dostawców, refów modeli i zachowania przełączania awaryjnego.](https://docs.openclaw.ai/pl/concepts/model-providers)

[**Rozumienie multimediów** \\
\\
Konfiguracja transkrypcji audio i wybór dostawcy.](https://docs.openclaw.ai/pl/nodes/media-understanding)

[MiniMax](https://docs.openclaw.ai/pl/providers/minimax) [Moonshot AI](https://docs.openclaw.ai/pl/providers/moonshot)

Ctrl+I