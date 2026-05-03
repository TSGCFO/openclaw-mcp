---
source_url: https://docs.openclaw.ai/tools/firecrawl
title: "Firecrawl - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/firecrawl#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Web tools

Firecrawl

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Get an API key](https://docs.openclaw.ai/tools/firecrawl#get-an-api-key)
- [Configure Firecrawl search](https://docs.openclaw.ai/tools/firecrawl#configure-firecrawl-search)
- [Configure Firecrawl scrape + web\_fetch fallback](https://docs.openclaw.ai/tools/firecrawl#configure-firecrawl-scrape-%2B-web_fetch-fallback)
- [Firecrawl plugin tools](https://docs.openclaw.ai/tools/firecrawl#firecrawl-plugin-tools)
- [firecrawl\_search](https://docs.openclaw.ai/tools/firecrawl#firecrawl_search)
- [firecrawl\_scrape](https://docs.openclaw.ai/tools/firecrawl#firecrawl_scrape)
- [Stealth / bot circumvention](https://docs.openclaw.ai/tools/firecrawl#stealth-%2F-bot-circumvention)
- [How web\_fetch uses Firecrawl](https://docs.openclaw.ai/tools/firecrawl#how-web_fetch-uses-firecrawl)
- [Related](https://docs.openclaw.ai/tools/firecrawl#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw can use **Firecrawl** in three ways:

- as the `web_search` provider
- as explicit plugin tools: `firecrawl_search` and `firecrawl_scrape`
- as a fallback extractor for `web_fetch`

It is a hosted extraction/search service that supports bot circumvention and caching,
which helps with JS-heavy sites or pages that block plain HTTP fetches.

## [​](https://docs.openclaw.ai/tools/firecrawl\#get-an-api-key)  Get an API key

1. Create a Firecrawl account and generate an API key.
2. Store it in config or set `FIRECRAWL_API_KEY` in the gateway environment.

## [​](https://docs.openclaw.ai/tools/firecrawl\#configure-firecrawl-search)  Configure Firecrawl search

```
{
  tools: {
    web: {
      search: {
        provider: "firecrawl",
      },
    },
  },
  plugins: {
    entries: {
      firecrawl: {
        enabled: true,
        config: {
          webSearch: {
            apiKey: "FIRECRAWL_API_KEY_HERE",
            baseUrl: "https://api.firecrawl.dev",
          },
        },
      },
    },
  },
}
```

Notes:

- Choosing Firecrawl in onboarding or `openclaw configure --section web` enables the bundled Firecrawl plugin automatically.
- `web_search` with Firecrawl supports `query` and `count`.
- For Firecrawl-specific controls like `sources`, `categories`, or result scraping, use `firecrawl_search`.
- `baseUrl` overrides must stay on `https://api.firecrawl.dev`.
- `FIRECRAWL_BASE_URL` is the shared env fallback for Firecrawl search and scrape base URLs.

## [​](https://docs.openclaw.ai/tools/firecrawl\#configure-firecrawl-scrape-+-web_fetch-fallback)  Configure Firecrawl scrape + web\_fetch fallback

```
{
  plugins: {
    entries: {
      firecrawl: {
        enabled: true,
        config: {
          webFetch: {
            apiKey: "FIRECRAWL_API_KEY_HERE",
            baseUrl: "https://api.firecrawl.dev",
            onlyMainContent: true,
            maxAgeMs: 172800000,
            timeoutSeconds: 60,
          },
        },
      },
    },
  },
}
```

Notes:

- Firecrawl fallback attempts run only when an API key is available (`plugins.entries.firecrawl.config.webFetch.apiKey` or `FIRECRAWL_API_KEY`).
- `maxAgeMs` controls how old cached results can be (ms). Default is 2 days.
- Legacy `tools.web.fetch.firecrawl.*` config is auto-migrated by `openclaw doctor --fix`.
- Firecrawl scrape/base URL overrides are restricted to `https://api.firecrawl.dev`.

`firecrawl_scrape` reuses the same `plugins.entries.firecrawl.config.webFetch.*` settings and env vars.

## [​](https://docs.openclaw.ai/tools/firecrawl\#firecrawl-plugin-tools)  Firecrawl plugin tools

### [​](https://docs.openclaw.ai/tools/firecrawl\#firecrawl_search)  `firecrawl_search`

Use this when you want Firecrawl-specific search controls instead of generic `web_search`.Core parameters:

- `query`
- `count`
- `sources`
- `categories`
- `scrapeResults`
- `timeoutSeconds`

### [​](https://docs.openclaw.ai/tools/firecrawl\#firecrawl_scrape)  `firecrawl_scrape`

Use this for JS-heavy or bot-protected pages where plain `web_fetch` is weak.Core parameters:

- `url`
- `extractMode`
- `maxChars`
- `onlyMainContent`
- `maxAgeMs`
- `proxy`
- `storeInCache`
- `timeoutSeconds`

## [​](https://docs.openclaw.ai/tools/firecrawl\#stealth-/-bot-circumvention)  Stealth / bot circumvention

Firecrawl exposes a **proxy mode** parameter for bot circumvention (`basic`, `stealth`, or `auto`).
OpenClaw always uses `proxy: "auto"` plus `storeInCache: true` for Firecrawl requests.
If proxy is omitted, Firecrawl defaults to `auto`. `auto` retries with stealth proxies if a basic attempt fails, which may use more credits
than basic-only scraping.

## [​](https://docs.openclaw.ai/tools/firecrawl\#how-web_fetch-uses-firecrawl)  How `web_fetch` uses Firecrawl

`web_fetch` extraction order:

1. Readability (local)
2. Firecrawl (if selected or auto-detected as the active web-fetch fallback)
3. Basic HTML cleanup (last fallback)

The selection knob is `tools.web.fetch.provider`. If you omit it, OpenClaw
auto-detects the first ready web-fetch provider from available credentials.
Today the bundled provider is Firecrawl.

## [​](https://docs.openclaw.ai/tools/firecrawl\#related)  Related

- [Web Search overview](https://docs.openclaw.ai/tools/web) — all providers and auto-detection
- [Web Fetch](https://docs.openclaw.ai/tools/web-fetch) — web\_fetch tool with Firecrawl fallback
- [Tavily](https://docs.openclaw.ai/tools/tavily) — search + extract tools

[Exa search](https://docs.openclaw.ai/tools/exa-search) [Gemini search](https://docs.openclaw.ai/tools/gemini-search)

Ctrl+I