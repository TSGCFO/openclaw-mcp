---
source_url: https://docs.openclaw.ai/tools/searxng-search
title: "SearXNG search - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/searxng-search#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Web tools

SearXNG search

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Setup](https://docs.openclaw.ai/tools/searxng-search#setup)
- [Config](https://docs.openclaw.ai/tools/searxng-search#config)
- [Environment variable](https://docs.openclaw.ai/tools/searxng-search#environment-variable)
- [Plugin config reference](https://docs.openclaw.ai/tools/searxng-search#plugin-config-reference)
- [Notes](https://docs.openclaw.ai/tools/searxng-search#notes)
- [Related](https://docs.openclaw.ai/tools/searxng-search#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw supports [SearXNG](https://docs.searxng.org/) as a **self-hosted,**
**key-free**`web_search` provider. SearXNG is an open-source meta-search engine
that aggregates results from Google, Bing, DuckDuckGo, and other sources.Advantages:

- **Free and unlimited** — no API key or commercial subscription required
- **Privacy / air-gap** — queries never leave your network
- **Works anywhere** — no region restrictions on commercial search APIs

## [​](https://docs.openclaw.ai/tools/searxng-search\#setup)  Setup

1

[Navigate to header](https://docs.openclaw.ai/tools/searxng-search#)

Run a SearXNG instance

```
docker run -d -p 8888:8080 searxng/searxng
```

Or use any existing SearXNG deployment you have access to. See the
[SearXNG documentation](https://docs.searxng.org/) for production setup.

2

[Navigate to header](https://docs.openclaw.ai/tools/searxng-search#)

Configure

```
openclaw configure --section web
# Select "searxng" as the provider
```

Or set the env var and let auto-detection find it:

```
export SEARXNG_BASE_URL="http://localhost:8888"
```

## [​](https://docs.openclaw.ai/tools/searxng-search\#config)  Config

```
{
  tools: {
    web: {
      search: {
        provider: "searxng",
      },
    },
  },
}
```

Plugin-level settings for the SearXNG instance:

```
{
  plugins: {
    entries: {
      searxng: {
        config: {
          webSearch: {
            baseUrl: "http://localhost:8888",
            categories: "general,news", // optional
            language: "en", // optional
          },
        },
      },
    },
  },
}
```

The `baseUrl` field also accepts SecretRef objects.Transport rules:

- `https://` works for public or private SearXNG hosts
- `http://` is only accepted for trusted private-network or loopback hosts
- public SearXNG hosts must use `https://`

## [​](https://docs.openclaw.ai/tools/searxng-search\#environment-variable)  Environment variable

Set `SEARXNG_BASE_URL` as an alternative to config:

```
export SEARXNG_BASE_URL="http://localhost:8888"
```

When `SEARXNG_BASE_URL` is set and no explicit provider is configured, auto-detection
picks SearXNG automatically (at the lowest priority — any API-backed provider with a
key wins first).

## [​](https://docs.openclaw.ai/tools/searxng-search\#plugin-config-reference)  Plugin config reference

| Field | Description |
| --- | --- |
| `baseUrl` | Base URL of your SearXNG instance (required) |
| `categories` | Comma-separated categories such as `general`, `news`, or `science` |
| `language` | Language code for results such as `en`, `de`, or `fr` |

## [​](https://docs.openclaw.ai/tools/searxng-search\#notes)  Notes

- **JSON API** — uses SearXNG’s native `format=json` endpoint, not HTML scraping
- **No API key** — works with any SearXNG instance out of the box
- **Base URL validation** — `baseUrl` must be a valid `http://` or `https://`
URL; public hosts must use `https://`
- **Auto-detection order** — SearXNG is checked last (order 200) in
auto-detection. API-backed providers with configured keys run first, then
DuckDuckGo (order 100), then Ollama Web Search (order 110)
- **Self-hosted** — you control the instance, queries, and upstream search engines
- **Categories** default to `general` when not configured

For SearXNG JSON API to work, make sure your SearXNG instance has the `json`
format enabled in its `settings.yml` under `search.formats`.

## [​](https://docs.openclaw.ai/tools/searxng-search\#related)  Related

- [Web Search overview](https://docs.openclaw.ai/tools/web) — all providers and auto-detection
- [DuckDuckGo Search](https://docs.openclaw.ai/tools/duckduckgo-search) — another key-free fallback
- [Brave Search](https://docs.openclaw.ai/tools/brave-search) — structured results with free tier

[Perplexity search](https://docs.openclaw.ai/tools/perplexity-search) [Tavily](https://docs.openclaw.ai/tools/tavily)

Ctrl+I