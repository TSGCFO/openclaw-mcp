---
source_url: https://docs.openclaw.ai/tools/brave-search
title: "Brave search - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/brave-search#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Web tools

Brave search

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Brave Search API](https://docs.openclaw.ai/tools/brave-search#brave-search-api)
- [Get an API key](https://docs.openclaw.ai/tools/brave-search#get-an-api-key)
- [Config example](https://docs.openclaw.ai/tools/brave-search#config-example)
- [Tool parameters](https://docs.openclaw.ai/tools/brave-search#tool-parameters)
- [Notes](https://docs.openclaw.ai/tools/brave-search#notes)
- [Related](https://docs.openclaw.ai/tools/brave-search#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/tools/brave-search\#brave-search-api)  Brave Search API

OpenClaw supports Brave Search API as a `web_search` provider.

## [​](https://docs.openclaw.ai/tools/brave-search\#get-an-api-key)  Get an API key

1. Create a Brave Search API account at [https://brave.com/search/api/](https://brave.com/search/api/)
2. In the dashboard, choose the **Search** plan and generate an API key.
3. Store the key in config or set `BRAVE_API_KEY` in the Gateway environment.

## [​](https://docs.openclaw.ai/tools/brave-search\#config-example)  Config example

```
{
  plugins: {
    entries: {
      brave: {
        config: {
          webSearch: {
            apiKey: "BRAVE_API_KEY_HERE",
            mode: "web", // or "llm-context"
            baseUrl: "https://api.search.brave.com", // optional proxy/base URL override
          },
        },
      },
    },
  },
  tools: {
    web: {
      search: {
        provider: "brave",
        maxResults: 5,
        timeoutSeconds: 30,
      },
    },
  },
}
```

Provider-specific Brave search settings now live under `plugins.entries.brave.config.webSearch.*`.
Legacy `tools.web.search.apiKey` still loads through the compatibility shim, but it is no longer the canonical config path.`webSearch.mode` controls the Brave transport:

- `web` (default): normal Brave web search with titles, URLs, and snippets
- `llm-context`: Brave LLM Context API with pre-extracted text chunks and sources for grounding

`webSearch.baseUrl` can point Brave requests at a trusted Brave-compatible proxy
or gateway. OpenClaw appends `/res/v1/web/search` or `/res/v1/llm/context` to
the configured base URL and keeps the base URL in the cache key. Public
endpoints must use `https://`; `http://` is accepted only for trusted loopback
or private-network proxy hosts.

## [​](https://docs.openclaw.ai/tools/brave-search\#tool-parameters)  Tool parameters

[​](https://docs.openclaw.ai/tools/brave-search#param-query)

query

string

required

Search query.

[​](https://docs.openclaw.ai/tools/brave-search#param-count)

count

number

default:"5"

Number of results to return (1–10).

[​](https://docs.openclaw.ai/tools/brave-search#param-country)

country

string

2-letter ISO country code (e.g. `US`, `DE`).

[​](https://docs.openclaw.ai/tools/brave-search#param-language)

language

string

ISO 639-1 language code for search results (e.g. `en`, `de`, `fr`).

[​](https://docs.openclaw.ai/tools/brave-search#param-search-lang)

search\_lang

string

Brave search-language code (e.g. `en`, `en-gb`, `zh-hans`).

[​](https://docs.openclaw.ai/tools/brave-search#param-ui-lang)

ui\_lang

string

ISO language code for UI elements.

[​](https://docs.openclaw.ai/tools/brave-search#param-freshness)

freshness

'day' \| 'week' \| 'month' \| 'year'

Time filter — `day` is 24 hours.

[​](https://docs.openclaw.ai/tools/brave-search#param-date-after)

date\_after

string

Only results published after this date (`YYYY-MM-DD`).

[​](https://docs.openclaw.ai/tools/brave-search#param-date-before)

date\_before

string

Only results published before this date (`YYYY-MM-DD`).

**Examples:**

```
// Country and language-specific search
await web_search({
  query: "renewable energy",
  country: "DE",
  language: "de",
});

// Recent results (past week)
await web_search({
  query: "AI news",
  freshness: "week",
});

// Date range search
await web_search({
  query: "AI developments",
  date_after: "2024-01-01",
  date_before: "2024-06-30",
});
```

## [​](https://docs.openclaw.ai/tools/brave-search\#notes)  Notes

- OpenClaw uses the Brave **Search** plan. If you have a legacy subscription (e.g. the original Free plan with 2,000 queries/month), it remains valid but does not include newer features like LLM Context or higher rate limits.
- Each Brave plan includes **$5/month in free credit** (renewing). The Search plan costs $5 per 1,000 requests, so the credit covers 1,000 queries/month. Set your usage limit in the Brave dashboard to avoid unexpected charges. See the [Brave API portal](https://brave.com/search/api/) for current plans.
- The Search plan includes the LLM Context endpoint and AI inference rights. Storing results to train or tune models requires a plan with explicit storage rights. See the Brave [Terms of Service](https://api-dashboard.search.brave.com/terms-of-service).
- `llm-context` mode returns grounded source entries instead of the normal web-search snippet shape.
- `llm-context` mode supports `freshness` and bounded `date_after` \+ `date_before` ranges. It does not support `ui_lang`; `date_before` without `date_after` is rejected because Brave requires custom freshness ranges to include both start and end dates.
- `ui_lang` must include a region subtag like `en-US`.
- Results are cached for 15 minutes by default (configurable via `cacheTtlMinutes`).
- Custom `webSearch.baseUrl` values are included in Brave cache identity, so
proxy-specific responses do not collide.
- Enable the `brave.http` diagnostics flag to log Brave request URLs/query params, response status/timing, and search-cache hit/miss/write events while troubleshooting. The flag never logs the API key or response bodies, but search queries can be sensitive.

## [​](https://docs.openclaw.ai/tools/brave-search\#related)  Related

- [Web Search overview](https://docs.openclaw.ai/tools/web) — all providers and auto-detection
- [Perplexity Search](https://docs.openclaw.ai/tools/perplexity-search) — structured results with domain filtering
- [Exa Search](https://docs.openclaw.ai/tools/exa-search) — neural search with content extraction

[Web Search](https://docs.openclaw.ai/tools/web) [DuckDuckGo search](https://docs.openclaw.ai/tools/duckduckgo-search)

Ctrl+I