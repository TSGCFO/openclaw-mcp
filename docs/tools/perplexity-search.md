---
source_url: https://docs.openclaw.ai/tools/perplexity-search
title: "Perplexity search - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/perplexity-search#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Web tools

Perplexity search

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Perplexity Search API](https://docs.openclaw.ai/tools/perplexity-search#perplexity-search-api)
- [Getting a Perplexity API key](https://docs.openclaw.ai/tools/perplexity-search#getting-a-perplexity-api-key)
- [OpenRouter compatibility](https://docs.openclaw.ai/tools/perplexity-search#openrouter-compatibility)
- [Config examples](https://docs.openclaw.ai/tools/perplexity-search#config-examples)
- [Native Perplexity Search API](https://docs.openclaw.ai/tools/perplexity-search#native-perplexity-search-api)
- [OpenRouter / Sonar compatibility](https://docs.openclaw.ai/tools/perplexity-search#openrouter-%2F-sonar-compatibility)
- [Where to set the key](https://docs.openclaw.ai/tools/perplexity-search#where-to-set-the-key)
- [Tool parameters](https://docs.openclaw.ai/tools/perplexity-search#tool-parameters)
- [Domain filter rules](https://docs.openclaw.ai/tools/perplexity-search#domain-filter-rules)
- [Notes](https://docs.openclaw.ai/tools/perplexity-search#notes)
- [Related](https://docs.openclaw.ai/tools/perplexity-search#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/tools/perplexity-search\#perplexity-search-api)  Perplexity Search API

OpenClaw supports Perplexity Search API as a `web_search` provider.
It returns structured results with `title`, `url`, and `snippet` fields.For compatibility, OpenClaw also supports legacy Perplexity Sonar/OpenRouter setups.
If you use `OPENROUTER_API_KEY`, an `sk-or-...` key in `plugins.entries.perplexity.config.webSearch.apiKey`, or set `plugins.entries.perplexity.config.webSearch.baseUrl` / `model`, the provider switches to the chat-completions path and returns AI-synthesized answers with citations instead of structured Search API results.

## [​](https://docs.openclaw.ai/tools/perplexity-search\#getting-a-perplexity-api-key)  Getting a Perplexity API key

1. Create a Perplexity account at [perplexity.ai/settings/api](https://www.perplexity.ai/settings/api)
2. Generate an API key in the dashboard
3. Store the key in config or set `PERPLEXITY_API_KEY` in the Gateway environment.

## [​](https://docs.openclaw.ai/tools/perplexity-search\#openrouter-compatibility)  OpenRouter compatibility

If you were already using OpenRouter for Perplexity Sonar, keep `provider: "perplexity"` and set `OPENROUTER_API_KEY` in the Gateway environment, or store an `sk-or-...` key in `plugins.entries.perplexity.config.webSearch.apiKey`.Optional compatibility controls:

- `plugins.entries.perplexity.config.webSearch.baseUrl`
- `plugins.entries.perplexity.config.webSearch.model`

## [​](https://docs.openclaw.ai/tools/perplexity-search\#config-examples)  Config examples

### [​](https://docs.openclaw.ai/tools/perplexity-search\#native-perplexity-search-api)  Native Perplexity Search API

```
{
  plugins: {
    entries: {
      perplexity: {
        config: {
          webSearch: {
            apiKey: "pplx-...",
          },
        },
      },
    },
  },
  tools: {
    web: {
      search: {
        provider: "perplexity",
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/tools/perplexity-search\#openrouter-/-sonar-compatibility)  OpenRouter / Sonar compatibility

```
{
  plugins: {
    entries: {
      perplexity: {
        config: {
          webSearch: {
            apiKey: "<openrouter-api-key>",
            baseUrl: "https://openrouter.ai/api/v1",
            model: "perplexity/sonar-pro",
          },
        },
      },
    },
  },
  tools: {
    web: {
      search: {
        provider: "perplexity",
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/tools/perplexity-search\#where-to-set-the-key)  Where to set the key

**Via config:** run `openclaw configure --section web`. It stores the key in
`~/.openclaw/openclaw.json` under `plugins.entries.perplexity.config.webSearch.apiKey`.
That field also accepts SecretRef objects.**Via environment:** set `PERPLEXITY_API_KEY` or `OPENROUTER_API_KEY`
in the Gateway process environment. For a gateway install, put it in
`~/.openclaw/.env` (or your service environment). See [Env vars](https://docs.openclaw.ai/help/faq#env-vars-and-env-loading).If `provider: "perplexity"` is configured and the Perplexity key SecretRef is unresolved with no env fallback, startup/reload fails fast.

## [​](https://docs.openclaw.ai/tools/perplexity-search\#tool-parameters)  Tool parameters

These parameters apply to the native Perplexity Search API path.

[​](https://docs.openclaw.ai/tools/perplexity-search#param-query)

query

string

required

Search query.

[​](https://docs.openclaw.ai/tools/perplexity-search#param-count)

count

number

default:"5"

Number of results to return (1–10).

[​](https://docs.openclaw.ai/tools/perplexity-search#param-country)

country

string

2-letter ISO country code (e.g. `US`, `DE`).

[​](https://docs.openclaw.ai/tools/perplexity-search#param-language)

language

string

ISO 639-1 language code (e.g. `en`, `de`, `fr`).

[​](https://docs.openclaw.ai/tools/perplexity-search#param-freshness)

freshness

'day' \| 'week' \| 'month' \| 'year'

Time filter — `day` is 24 hours.

[​](https://docs.openclaw.ai/tools/perplexity-search#param-date-after)

date\_after

string

Only results published after this date (`YYYY-MM-DD`).

[​](https://docs.openclaw.ai/tools/perplexity-search#param-date-before)

date\_before

string

Only results published before this date (`YYYY-MM-DD`).

[​](https://docs.openclaw.ai/tools/perplexity-search#param-domain-filter)

domain\_filter

string\[\]

Domain allowlist/denylist array (max 20).

[​](https://docs.openclaw.ai/tools/perplexity-search#param-max-tokens)

max\_tokens

number

default:"25000"

Total content budget (max 1000000).

[​](https://docs.openclaw.ai/tools/perplexity-search#param-max-tokens-per-page)

max\_tokens\_per\_page

number

default:"2048"

Per-page token limit.

For the legacy Sonar/OpenRouter compatibility path:

- `query`, `count`, and `freshness` are accepted
- `count` is compatibility-only there; the response is still one synthesized
answer with citations rather than an N-result list
- Search API-only filters such as `country`, `language`, `date_after`,
`date_before`, `domain_filter`, `max_tokens`, and `max_tokens_per_page`
return explicit errors

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

// Domain filtering (allowlist)
await web_search({
  query: "climate research",
  domain_filter: ["nature.com", "science.org", ".edu"],
});

// Domain filtering (denylist - prefix with -)
await web_search({
  query: "product reviews",
  domain_filter: ["-reddit.com", "-pinterest.com"],
});

// More content extraction
await web_search({
  query: "detailed AI research",
  max_tokens: 50000,
  max_tokens_per_page: 4096,
});
```

### [​](https://docs.openclaw.ai/tools/perplexity-search\#domain-filter-rules)  Domain filter rules

- Maximum 20 domains per filter
- Cannot mix allowlist and denylist in the same request
- Use `-` prefix for denylist entries (e.g., `["-reddit.com"]`)

## [​](https://docs.openclaw.ai/tools/perplexity-search\#notes)  Notes

- Perplexity Search API returns structured web search results (`title`, `url`, `snippet`)
- OpenRouter or explicit `plugins.entries.perplexity.config.webSearch.baseUrl` / `model` switches Perplexity back to Sonar chat completions for compatibility
- Sonar/OpenRouter compatibility returns one synthesized answer with citations, not structured result rows
- Results are cached for 15 minutes by default (configurable via `cacheTtlMinutes`)

## [​](https://docs.openclaw.ai/tools/perplexity-search\#related)  Related

- [Web Search overview](https://docs.openclaw.ai/tools/web) — all providers and auto-detection
- [Perplexity Search API docs](https://docs.perplexity.ai/docs/search/quickstart) — official Perplexity documentation
- [Brave Search](https://docs.openclaw.ai/tools/brave-search) — structured results with country/language filters
- [Exa Search](https://docs.openclaw.ai/tools/exa-search) — neural search with content extraction

[Ollama web search](https://docs.openclaw.ai/tools/ollama-search) [SearXNG search](https://docs.openclaw.ai/tools/searxng-search)

Ctrl+I