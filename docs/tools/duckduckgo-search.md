---
source_url: https://docs.openclaw.ai/tools/duckduckgo-search
title: "DuckDuckGo search - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/duckduckgo-search#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Web tools

DuckDuckGo search

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Setup](https://docs.openclaw.ai/tools/duckduckgo-search#setup)
- [Config](https://docs.openclaw.ai/tools/duckduckgo-search#config)
- [Tool parameters](https://docs.openclaw.ai/tools/duckduckgo-search#tool-parameters)
- [Notes](https://docs.openclaw.ai/tools/duckduckgo-search#notes)
- [Related](https://docs.openclaw.ai/tools/duckduckgo-search#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw supports DuckDuckGo as a **key-free**`web_search` provider. No API
key or account is required.

DuckDuckGo is an **experimental, unofficial** integration that pulls results
from DuckDuckGo’s non-JavaScript search pages — not an official API. Expect
occasional breakage from bot-challenge pages or HTML changes.

## [​](https://docs.openclaw.ai/tools/duckduckgo-search\#setup)  Setup

No API key needed — just set DuckDuckGo as your provider:

1

[Navigate to header](https://docs.openclaw.ai/tools/duckduckgo-search#)

Configure

```
openclaw configure --section web
# Select "duckduckgo" as the provider
```

## [​](https://docs.openclaw.ai/tools/duckduckgo-search\#config)  Config

```
{
  tools: {
    web: {
      search: {
        provider: "duckduckgo",
      },
    },
  },
}
```

Optional plugin-level settings for region and SafeSearch:

```
{
  plugins: {
    entries: {
      duckduckgo: {
        config: {
          webSearch: {
            region: "us-en", // DuckDuckGo region code
            safeSearch: "moderate", // "strict", "moderate", or "off"
          },
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/tools/duckduckgo-search\#tool-parameters)  Tool parameters

[​](https://docs.openclaw.ai/tools/duckduckgo-search#param-query)

query

string

required

Search query.

[​](https://docs.openclaw.ai/tools/duckduckgo-search#param-count)

count

number

default:"5"

Results to return (1–10).

[​](https://docs.openclaw.ai/tools/duckduckgo-search#param-region)

region

string

DuckDuckGo region code (e.g. `us-en`, `uk-en`, `de-de`).

[​](https://docs.openclaw.ai/tools/duckduckgo-search#param-safe-search)

safeSearch

'strict' \| 'moderate' \| 'off'

default:"moderate"

SafeSearch level.

Region and SafeSearch can also be set in plugin config (see above) — tool
parameters override config values per-query.

## [​](https://docs.openclaw.ai/tools/duckduckgo-search\#notes)  Notes

- **No API key** — works out of the box, zero configuration
- **Experimental** — gathers results from DuckDuckGo’s non-JavaScript HTML
search pages, not an official API or SDK
- **Bot-challenge risk** — DuckDuckGo may serve CAPTCHAs or block requests
under heavy or automated use
- **HTML parsing** — results depend on page structure, which can change without
notice
- **Auto-detection order** — DuckDuckGo is the first key-free fallback
(order 100) in auto-detection. API-backed providers with configured keys run
first, then Ollama Web Search (order 110), then SearXNG (order 200)
- **SafeSearch defaults to moderate** when not configured

For production use, consider [Brave Search](https://docs.openclaw.ai/tools/brave-search) (free tier
available) or another API-backed provider.

## [​](https://docs.openclaw.ai/tools/duckduckgo-search\#related)  Related

- [Web Search overview](https://docs.openclaw.ai/tools/web) — all providers and auto-detection
- [Brave Search](https://docs.openclaw.ai/tools/brave-search) — structured results with free tier
- [Exa Search](https://docs.openclaw.ai/tools/exa-search) — neural search with content extraction

[Brave search](https://docs.openclaw.ai/tools/brave-search) [Exa search](https://docs.openclaw.ai/tools/exa-search)

Ctrl+I