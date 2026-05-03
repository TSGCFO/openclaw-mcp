---
source_url: https://docs.openclaw.ai/tr/providers/moonshot
title: "Moonshot AI - OpenClaw"
---

[Ana içeriğe atla](https://docs.openclaw.ai/tr/providers/moonshot#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/tr)

![TR](https://d3gk2c5xim1je2.cloudfront.net/flags/TR.svg)

Türkçe

Ara...

Ctrl K

Ara...

Navigation

Providers

Moonshot AI

[Get started](https://docs.openclaw.ai/tr) [Install](https://docs.openclaw.ai/tr/install) [Channels](https://docs.openclaw.ai/tr/channels) [Agents](https://docs.openclaw.ai/tr/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tr/tools) [Models](https://docs.openclaw.ai/tr/providers) [Platforms](https://docs.openclaw.ai/tr/platforms) [Gateway & Ops](https://docs.openclaw.ai/tr/gateway) [Reference](https://docs.openclaw.ai/tr/cli) [Help](https://docs.openclaw.ai/tr/help)

Bu sayfada

- [Yerleşik model kataloğu](https://docs.openclaw.ai/tr/providers/moonshot#yerle%C5%9Fik-model-katalo%C4%9Fu)
- [Başlangıç](https://docs.openclaw.ai/tr/providers/moonshot#ba%C5%9Flang%C4%B1%C3%A7)
- [Yapılandırma örneği](https://docs.openclaw.ai/tr/providers/moonshot#yap%C4%B1land%C4%B1rma-%C3%B6rne%C4%9Fi)
- [Kimi web arama](https://docs.openclaw.ai/tr/providers/moonshot#kimi-web-arama)
- [Gelişmiş yapılandırma](https://docs.openclaw.ai/tr/providers/moonshot#geli%C5%9Fmi%C5%9F-yap%C4%B1land%C4%B1rma)
- [İlgili](https://docs.openclaw.ai/tr/providers/moonshot#i%CC%87lgili)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Moonshot, OpenAI uyumlu uç noktalarla Kimi API’sini sağlar. Sağlayıcıyı
yapılandırın ve varsayılan modeli `moonshot/kimi-k2.6` olarak ayarlayın veya
`kimi/kimi-code` ile Kimi Coding kullanın.

Moonshot ve Kimi Coding **ayrı sağlayıcılardır**. Anahtarlar birbiri yerine kullanılamaz, uç noktalar farklıdır ve model başvuruları farklıdır (`moonshot/...` ile `kimi/...`).

## [​](https://docs.openclaw.ai/tr/providers/moonshot\#yerle%C5%9Fik-model-katalo%C4%9Fu)  Yerleşik model kataloğu

| Model başvurusu | Ad | Muhakeme | Girdi | Bağlam | Maks. çıktı |
| --- | --- | --- | --- | --- | --- |
| `moonshot/kimi-k2.6` | Kimi K2.6 | Hayır | text, image | 262,144 | 262,144 |
| `moonshot/kimi-k2.5` | Kimi K2.5 | Hayır | text, image | 262,144 | 262,144 |
| `moonshot/kimi-k2-thinking` | Kimi K2 Thinking | Evet | text | 262,144 | 262,144 |
| `moonshot/kimi-k2-thinking-turbo` | Kimi K2 Thinking Turbo | Evet | text | 262,144 | 262,144 |
| `moonshot/kimi-k2-turbo` | Kimi K2 Turbo | Hayır | text | 256,000 | 16,384 |

Güncel Moonshot barındırmalı K2 modelleri için paketlenmiş maliyet tahminleri,
Moonshot’un yayımlanmış kullandıkça öde ücretlerini kullanır: Kimi K2.6 için
önbellek isabeti 0.16/MTok,girdi0.16/MTok, girdi 0.16/MTok,girdi0.95/MTok ve çıktı 4.00/MTok;KimiK2.5ic\\cino¨nbellekisabeti4.00/MTok; Kimi K2.5
için önbellek isabeti 4.00/MTok;KimiK2.5ic\\c​ino¨nbellekisabeti0.10/MTok, girdi 0.60/MTokvec\\cıktı0.60/MTok ve çıktı 0.60/MTokvec\\c​ıktı3.00/MTok’tur.
Diğer eski katalog girdileri, bunları yapılandırmada geçersiz kılmadığınız
sürece sıfır maliyet yer tutucularını korur.

## [​](https://docs.openclaw.ai/tr/providers/moonshot\#ba%C5%9Flang%C4%B1%C3%A7)  Başlangıç

Sağlayıcınızı seçin ve kurulum adımlarını izleyin.

- Moonshot API

- Kimi Coding


**Şunun için en iyisi:** Moonshot Open Platform üzerinden Kimi K2 modelleri.

1

[Navigate to header](https://docs.openclaw.ai/tr/providers/moonshot#)

Uç nokta bölgenizi seçin

| Kimlik doğrulama seçeneği | Uç nokta | Bölge |
| --- | --- | --- |
| `moonshot-api-key` | `https://api.moonshot.ai/v1` | Uluslararası |
| `moonshot-api-key-cn` | `https://api.moonshot.cn/v1` | Çin |

2

[Navigate to header](https://docs.openclaw.ai/tr/providers/moonshot#)

Onboarding çalıştırın

```
openclaw onboard --auth-choice moonshot-api-key
```

Veya Çin uç noktası için:

```
openclaw onboard --auth-choice moonshot-api-key-cn
```

3

[Navigate to header](https://docs.openclaw.ai/tr/providers/moonshot#)

Varsayılan bir model ayarlayın

```
{
  agents: {
    defaults: {
      model: { primary: "moonshot/kimi-k2.6" },
    },
  },
}
```

4

[Navigate to header](https://docs.openclaw.ai/tr/providers/moonshot#)

Modellerin kullanılabilir olduğunu doğrulayın

```
openclaw models list --provider moonshot
```

5

[Navigate to header](https://docs.openclaw.ai/tr/providers/moonshot#)

Canlı bir smoke test çalıştırın

Normal oturumlarınıza dokunmadan model erişimini ve maliyet
takibini doğrulamak istediğinizde yalıtılmış bir durum dizini kullanın:

```
OPENCLAW_CONFIG_PATH=/tmp/openclaw-kimi/openclaw.json \
OPENCLAW_STATE_DIR=/tmp/openclaw-kimi \
openclaw agent --local \
  --session-id live-kimi-cost \
  --message 'Reply exactly: KIMI_LIVE_OK' \
  --thinking off \
  --json
```

JSON yanıtı `provider: "moonshot"` ve
`model: "kimi-k2.6"` bildirmelidir. Asistan transkript girdisi,
Moonshot kullanım üst verilerini döndürdüğünde `usage.cost` altında
normalleştirilmiş token kullanımını ve tahmini maliyeti saklar.

### [​](https://docs.openclaw.ai/tr/providers/moonshot\#yap%C4%B1land%C4%B1rma-%C3%B6rne%C4%9Fi)  Yapılandırma örneği

```
{
  env: { MOONSHOT_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "moonshot/kimi-k2.6" },
      models: {
        // moonshot-kimi-k2-aliases:start
        "moonshot/kimi-k2.6": { alias: "Kimi K2.6" },
        "moonshot/kimi-k2.5": { alias: "Kimi K2.5" },
        "moonshot/kimi-k2-thinking": { alias: "Kimi K2 Thinking" },
        "moonshot/kimi-k2-thinking-turbo": { alias: "Kimi K2 Thinking Turbo" },
        "moonshot/kimi-k2-turbo": { alias: "Kimi K2 Turbo" },
        // moonshot-kimi-k2-aliases:end
      },
    },
  },
  models: {
    mode: "merge",
    providers: {
      moonshot: {
        baseUrl: "https://api.moonshot.ai/v1",
        api: "openai-completions",
        apiKey: "${MOONSHOT_API_KEY}",
        models: [\
          // moonshot-kimi-k2-models:start\
          {\
            id: "kimi-k2.6",\
            name: "Kimi K2.6",\
            reasoning: false,\
            input: ["text", "image"],\
            cost: { input: 0.95, output: 4, cacheRead: 0.16, cacheWrite: 0 },\
            contextWindow: 262144,\
            maxTokens: 262144,\
          },\
          {\
            id: "kimi-k2.5",\
            name: "Kimi K2.5",\
            reasoning: false,\
            input: ["text", "image"],\
            cost: { input: 0.6, output: 3, cacheRead: 0.1, cacheWrite: 0 },\
            contextWindow: 262144,\
            maxTokens: 262144,\
          },\
          {\
            id: "kimi-k2-thinking",\
            name: "Kimi K2 Thinking",\
            reasoning: true,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 262144,\
            maxTokens: 262144,\
          },\
          {\
            id: "kimi-k2-thinking-turbo",\
            name: "Kimi K2 Thinking Turbo",\
            reasoning: true,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 262144,\
            maxTokens: 262144,\
          },\
          {\
            id: "kimi-k2-turbo",\
            name: "Kimi K2 Turbo",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 256000,\
            maxTokens: 16384,\
          },\
          // moonshot-kimi-k2-models:end\
        ],
      },
    },
  },
}
```

**Şunun için en iyisi:** Kimi Coding uç noktası üzerinden kod odaklı görevler.

Kimi Coding, Moonshot’tan (`moonshot/...`) farklı bir API anahtarı ve sağlayıcı öneki (`kimi/...`) kullanır. Eski model başvurusu `kimi/k2p5`, uyumluluk kimliği olarak kabul edilmeye devam eder.

1

[Navigate to header](https://docs.openclaw.ai/tr/providers/moonshot#)

Onboarding çalıştırın

```
openclaw onboard --auth-choice kimi-code-api-key
```

2

[Navigate to header](https://docs.openclaw.ai/tr/providers/moonshot#)

Varsayılan bir model ayarlayın

```
{
  agents: {
    defaults: {
      model: { primary: "kimi/kimi-code" },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/tr/providers/moonshot#)

Modelin kullanılabilir olduğunu doğrulayın

```
openclaw models list --provider kimi
```

### [​](https://docs.openclaw.ai/tr/providers/moonshot\#yap%C4%B1land%C4%B1rma-%C3%B6rne%C4%9Fi-2)  Yapılandırma örneği

```
{
  env: { KIMI_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "kimi/kimi-code" },
      models: {
        "kimi/kimi-code": { alias: "Kimi" },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/tr/providers/moonshot\#kimi-web-arama)  Kimi web arama

OpenClaw ayrıca, Moonshot web arama tarafından desteklenen bir `web_search`
sağlayıcısı olarak **Kimi** ile gelir.

1

[Navigate to header](https://docs.openclaw.ai/tr/providers/moonshot#)

Etkileşimli web arama kurulumunu çalıştırın

```
openclaw configure --section web
```

Web arama bölümünde **Kimi** seçeneğini seçerek
`plugins.entries.moonshot.config.webSearch.*` değerlerini depolayın.

2

[Navigate to header](https://docs.openclaw.ai/tr/providers/moonshot#)

Web arama bölgesini ve modeli yapılandırın

Etkileşimli kurulum şunları sorar:

| Ayar | Seçenekler |
| --- | --- |
| API bölgesi | `https://api.moonshot.ai/v1` (uluslararası) veya `https://api.moonshot.cn/v1` (Çin) |
| Web arama modeli | Varsayılan olarak `kimi-k2.6` |

Yapılandırma `plugins.entries.moonshot.config.webSearch` altında bulunur:

```
{
  plugins: {
    entries: {
      moonshot: {
        config: {
          webSearch: {
            apiKey: "sk-...", // veya KIMI_API_KEY / MOONSHOT_API_KEY kullanın
            baseUrl: "https://api.moonshot.ai/v1",
            model: "kimi-k2.6",
          },
        },
      },
    },
  },
  tools: {
    web: {
      search: {
        provider: "kimi",
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/tr/providers/moonshot\#geli%C5%9Fmi%C5%9F-yap%C4%B1land%C4%B1rma)  Gelişmiş yapılandırma

Yerel düşünme modu

Moonshot Kimi, ikili yerel düşünmeyi destekler:

- `thinking: { type: "enabled" }`
- `thinking: { type: "disabled" }`

Bunu model başına `agents.defaults.models.<provider/model>.params` üzerinden yapılandırın:

```
{
  agents: {
    defaults: {
      models: {
        "moonshot/kimi-k2.6": {
          params: {
            thinking: { type: "disabled" },
          },
        },
      },
    },
  },
}
```

OpenClaw, Moonshot için çalışma zamanı `/think` düzeylerini de eşler:

| `/think` düzeyi | Moonshot davranışı |
| --- | --- |
| `/think off` | `thinking.type=disabled` |
| Kapalı olmayan her düzey | `thinking.type=enabled` |

Moonshot düşünmesi etkin olduğunda, `tool_choice` değeri `auto` veya `none` olmalıdır. OpenClaw, uyumluluk için uyumsuz `tool_choice` değerlerini `auto` olarak normalleştirir.

Kimi K2.6 ayrıca `reasoning_content` içeriğinin çok turlu korunmasını
denetleyen isteğe bağlı bir `thinking.keep` alanını kabul eder. Turlar
arasında tam muhakemeyi korumak için bunu `"all"` olarak ayarlayın; sunucu
varsayılan stratejisini kullanmak için bunu atlayın (veya `null` bırakın).
OpenClaw, `thinking.keep` alanını yalnızca `moonshot/kimi-k2.6` için iletir
ve diğer modellerden kaldırır.

```
{
  agents: {
    defaults: {
      models: {
        "moonshot/kimi-k2.6": {
          params: {
            thinking: { type: "enabled", keep: "all" },
          },
        },
      },
    },
  },
}
```

Tool call kimliği temizleme

Moonshot Kimi, `functions.<name>:<index>` biçimindeki `tool_call` kimliklerini sunar. OpenClaw, çok turlu tool call işlemlerinin çalışmaya devam etmesi için bunları değiştirmeden korur.Özel bir OpenAI uyumlu sağlayıcıda katı temizlemeyi zorlamak için `sanitizeToolCallIds: true` ayarlayın:

```
{
  models: {
    providers: {
      "my-kimi-proxy": {
        api: "openai-completions",
        sanitizeToolCallIds: true,
      },
    },
  },
}
```

Akış kullanım uyumluluğu

Yerel Moonshot uç noktaları (`https://api.moonshot.ai/v1` ve
`https://api.moonshot.cn/v1`), paylaşılan `openai-completions` taşıması
üzerinde akış kullanım uyumluluğunu bildirir. OpenClaw bunu uç nokta
yeteneklerine göre belirler; bu nedenle aynı yerel Moonshot ana bilgisayarlarını
hedefleyen uyumlu özel sağlayıcı kimlikleri de aynı akış-kullanım
davranışını devralır.Paketlenmiş K2.6 fiyatlandırmasıyla, girdi, çıktı ve önbellek-okuma
token’larını içeren akış kullanımı; `/status`, `/usage full`, `/usage cost`
ve transkript destekli oturum muhasebesi için yerel tahmini USD maliyetine
de dönüştürülür.

Uç nokta ve model başvurusu başvurusu

| Sağlayıcı | Model başvurusu öneki | Uç nokta | Kimlik doğrulama ortam değişkeni |
| --- | --- | --- | --- |
| Moonshot | `moonshot/` | `https://api.moonshot.ai/v1` | `MOONSHOT_API_KEY` |
| Moonshot CN | `moonshot/` | `https://api.moonshot.cn/v1` | `MOONSHOT_API_KEY` |
| Kimi Coding | `kimi/` | Kimi Coding uç noktası | `KIMI_API_KEY` |
| Web arama | N/A | Moonshot API bölgesiyle aynı | `KIMI_API_KEY` veya `MOONSHOT_API_KEY` |

- Kimi web arama, `KIMI_API_KEY` veya `MOONSHOT_API_KEY` kullanır ve varsayılan olarak `https://api.moonshot.ai/v1` ile `kimi-k2.6` modelini kullanır.
- Gerekirse fiyatlandırma ve bağlam üst verilerini `models.providers` içinde geçersiz kılın.
- Moonshot bir model için farklı bağlam sınırları yayımlarsa, `contextWindow` değerini buna göre ayarlayın.

## [​](https://docs.openclaw.ai/tr/providers/moonshot\#i%CC%87lgili)  İlgili

[**Model seçimi** \\
\\
Sağlayıcıları, model başvurularını ve yük devretme davranışını seçme.](https://docs.openclaw.ai/tr/concepts/model-providers)

[**Web arama** \\
\\
Kimi dahil web arama sağlayıcılarını yapılandırma.](https://docs.openclaw.ai/tr/tools/web)

[**Yapılandırma başvurusu** \\
\\
Sağlayıcılar, modeller ve Plugin’ler için tam yapılandırma şeması.](https://docs.openclaw.ai/tr/gateway/configuration-reference)

[**Moonshot Open Platform** \\
\\
Moonshot API anahtarı yönetimi ve belgeler.](https://platform.moonshot.ai/)

[Mistral](https://docs.openclaw.ai/tr/providers/mistral) [NVIDIA](https://docs.openclaw.ai/tr/providers/nvidia)

Ctrl+I