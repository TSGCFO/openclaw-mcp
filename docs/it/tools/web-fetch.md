---
source_url: https://docs.openclaw.ai/it/tools/web-fetch
title: "Recupero web - OpenClaw"
---

[Vai al contenuto principale](https://docs.openclaw.ai/it/tools/web-fetch#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/it)

![IT](https://d3gk2c5xim1je2.cloudfront.net/flags/IT.svg)

Italiano

Cerca...

Ctrl K

Cerca...

Navigation

Web tools

Recupero web

[Get started](https://docs.openclaw.ai/it) [Install](https://docs.openclaw.ai/it/install) [Channels](https://docs.openclaw.ai/it/channels) [Agents](https://docs.openclaw.ai/it/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/it/tools) [Models](https://docs.openclaw.ai/it/providers) [Platforms](https://docs.openclaw.ai/it/platforms) [Gateway & Ops](https://docs.openclaw.ai/it/gateway) [Reference](https://docs.openclaw.ai/it/cli) [Help](https://docs.openclaw.ai/it/help)

In questa pagina

- [Avvio rapido](https://docs.openclaw.ai/it/tools/web-fetch#avvio-rapido)
- [Parametri dello strumento](https://docs.openclaw.ai/it/tools/web-fetch#parametri-dello-strumento)
- [Come funziona](https://docs.openclaw.ai/it/tools/web-fetch#come-funziona)
- [Configurazione](https://docs.openclaw.ai/it/tools/web-fetch#configurazione)
- [Fallback Firecrawl](https://docs.openclaw.ai/it/tools/web-fetch#fallback-firecrawl)
- [Limiti e sicurezza](https://docs.openclaw.ai/it/tools/web-fetch#limiti-e-sicurezza)
- [Profili degli strumenti](https://docs.openclaw.ai/it/tools/web-fetch#profili-degli-strumenti)
- [Correlati](https://docs.openclaw.ai/it/tools/web-fetch#correlati)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Lo strumento `web_fetch` esegue una semplice HTTP GET ed estrae contenuto leggibile
(da HTML a markdown o testo). **Non** esegue JavaScript.Per siti molto dipendenti da JS o pagine protette da login, usa invece il
[Browser Web](https://docs.openclaw.ai/it/tools/browser).

## [​](https://docs.openclaw.ai/it/tools/web-fetch\#avvio-rapido)  Avvio rapido

`web_fetch` è **abilitato per impostazione predefinita** — non serve alcuna configurazione. L’agente può
chiamarlo immediatamente:

```
await web_fetch({ url: "https://example.com/article" });
```

## [​](https://docs.openclaw.ai/it/tools/web-fetch\#parametri-dello-strumento)  Parametri dello strumento

[​](https://docs.openclaw.ai/it/tools/web-fetch#param-url)

url

string

obbligatorio

URL da recuperare. Solo `http(s)`.

[​](https://docs.openclaw.ai/it/tools/web-fetch#param-extract-mode)

extractMode

'markdown' \| 'text'

predefinito:"markdown"

Formato di output dopo l’estrazione del contenuto principale.

[​](https://docs.openclaw.ai/it/tools/web-fetch#param-max-chars)

maxChars

number

Tronca l’output a questo numero di caratteri.

## [​](https://docs.openclaw.ai/it/tools/web-fetch\#come-funziona)  Come funziona

1

[Navigate to header](https://docs.openclaw.ai/it/tools/web-fetch#)

Recupero

Invia una HTTP GET con uno User-Agent simile a Chrome e l’header
`Accept-Language`. Blocca nomi host privati/interni e ricontrolla i redirect.

2

[Navigate to header](https://docs.openclaw.ai/it/tools/web-fetch#)

Estrazione

Esegue Readability (estrazione del contenuto principale) sulla risposta HTML.

3

[Navigate to header](https://docs.openclaw.ai/it/tools/web-fetch#)

Fallback (opzionale)

Se Readability fallisce e Firecrawl è configurato, ritenta tramite la
Firecrawl API con modalità di elusione dei bot.

4

[Navigate to header](https://docs.openclaw.ai/it/tools/web-fetch#)

Cache

I risultati vengono memorizzati nella cache per 15 minuti (configurabile) per ridurre i
recuperi ripetuti dello stesso URL.

## [​](https://docs.openclaw.ai/it/tools/web-fetch\#configurazione)  Configurazione

```
{
  tools: {
    web: {
      fetch: {
        enabled: true, // default: true
        provider: "firecrawl", // optional; omit for auto-detect
        maxChars: 50000, // max output chars
        maxCharsCap: 50000, // hard cap for maxChars param
        maxResponseBytes: 2000000, // max download size before truncation
        timeoutSeconds: 30,
        cacheTtlMinutes: 15,
        maxRedirects: 3,
        readability: true, // use Readability extraction
        userAgent: "Mozilla/5.0 ...", // override User-Agent
        ssrfPolicy: {
          allowRfc2544BenchmarkRange: true, // opt-in for trusted fake-IP proxies using 198.18.0.0/15
          allowIpv6UniqueLocalRange: true, // opt-in for trusted fake-IP proxies using fc00::/7
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/it/tools/web-fetch\#fallback-firecrawl)  Fallback Firecrawl

Se l’estrazione Readability fallisce, `web_fetch` può ricorrere a
[Firecrawl](https://docs.openclaw.ai/it/tools/firecrawl) per l’elusione dei bot e un’estrazione migliore:

```
{
  tools: {
    web: {
      fetch: {
        provider: "firecrawl", // optional; omit for auto-detect from available credentials
      },
    },
  },
  plugins: {
    entries: {
      firecrawl: {
        enabled: true,
        config: {
          webFetch: {
            apiKey: "fc-...", // optional if FIRECRAWL_API_KEY is set
            baseUrl: "https://api.firecrawl.dev",
            onlyMainContent: true,
            maxAgeMs: 86400000, // cache duration (1 day)
            timeoutSeconds: 60,
          },
        },
      },
    },
  },
}
```

`plugins.entries.firecrawl.config.webFetch.apiKey` supporta oggetti SecretRef.
La configurazione legacy `tools.web.fetch.firecrawl.*` viene migrata automaticamente da `openclaw doctor --fix`.

Se Firecrawl è abilitato e il suo SecretRef non è risolto senza fallback dell’env
`FIRECRAWL_API_KEY`, l’avvio del Gateway fallisce subito.

Gli override di `baseUrl` di Firecrawl sono bloccati: il traffico hosted usa
`https://api.firecrawl.dev`; gli override self-hosted devono puntare a endpoint privati o
interni, e `http://` è accettato solo per tali destinazioni private.

Comportamento runtime attuale:

- `tools.web.fetch.provider` seleziona esplicitamente il provider di fallback per il recupero.
- Se `provider` viene omesso, OpenClaw rileva automaticamente il primo provider web-fetch
pronto tra le credenziali disponibili. `web_fetch` non in sandbox può usare
Plugin installati che dichiarano `contracts.webFetchProviders` e registrano un
provider corrispondente a runtime. Oggi il provider incluso è Firecrawl.
- Le chiamate `web_fetch` in sandbox restano limitate ai provider inclusi.
- Se Readability è disabilitato, `web_fetch` passa direttamente al fallback del
provider selezionato. Se non è disponibile alcun provider, fallisce in modo chiuso.

## [​](https://docs.openclaw.ai/it/tools/web-fetch\#limiti-e-sicurezza)  Limiti e sicurezza

- `maxChars` è limitato a `tools.web.fetch.maxCharsCap`
- Il corpo della risposta è limitato a `maxResponseBytes` prima del parsing; le risposte
troppo grandi vengono troncate con un avviso
- I nomi host privati/interni sono bloccati
- `tools.web.fetch.ssrfPolicy.allowRfc2544BenchmarkRange` e
`tools.web.fetch.ssrfPolicy.allowIpv6UniqueLocalRange` sono opt-in ristretti
per stack proxy fake-IP attendibili; lasciali non impostati salvo che il tuo proxy possieda
tali intervalli sintetici e applichi la propria policy di destinazione
- I redirect vengono controllati e limitati da `maxRedirects`
- `web_fetch` è best-effort — alcuni siti richiedono il [Browser Web](https://docs.openclaw.ai/it/tools/browser)

## [​](https://docs.openclaw.ai/it/tools/web-fetch\#profili-degli-strumenti)  Profili degli strumenti

Se usi profili degli strumenti o allowlist, aggiungi `web_fetch` o `group:web`:

```
{
  tools: {
    allow: ["web_fetch"],
    // or: allow: ["group:web"]  (includes web_fetch, web_search, and x_search)
  },
}
```

## [​](https://docs.openclaw.ai/it/tools/web-fetch\#correlati)  Correlati

- [Ricerca Web](https://docs.openclaw.ai/it/tools/web) — cerca nel web con più provider
- [Browser Web](https://docs.openclaw.ai/it/tools/browser) — automazione completa del browser per siti molto dipendenti da JS
- [Firecrawl](https://docs.openclaw.ai/it/tools/firecrawl) — strumenti di ricerca e scraping di Firecrawl

[Risoluzione dei problemi di WSL2 + Windows + Chrome CDP remoto](https://docs.openclaw.ai/it/tools/browser-wsl2-windows-remote-cdp-troubleshooting) [Web Search](https://docs.openclaw.ai/it/tools/web)

Ctrl+I