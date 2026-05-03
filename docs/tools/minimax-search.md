---
source_url: https://docs.openclaw.ai/tools/minimax-search
title: "MiniMax search - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/minimax-search#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Web tools

MiniMax search

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Get a Token Plan credential](https://docs.openclaw.ai/tools/minimax-search#get-a-token-plan-credential)
- [Config](https://docs.openclaw.ai/tools/minimax-search#config)
- [Region selection](https://docs.openclaw.ai/tools/minimax-search#region-selection)
- [Supported parameters](https://docs.openclaw.ai/tools/minimax-search#supported-parameters)
- [Related](https://docs.openclaw.ai/tools/minimax-search#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw supports MiniMax as a `web_search` provider through the MiniMax
Token Plan search API. It returns structured search results with titles, URLs,
snippets, and related queries.

## [​](https://docs.openclaw.ai/tools/minimax-search\#get-a-token-plan-credential)  Get a Token Plan credential

1

[Navigate to header](https://docs.openclaw.ai/tools/minimax-search#)

Create a key

Create or copy a MiniMax Token Plan key from
[MiniMax Platform](https://platform.minimax.io/user-center/basic-information/interface-key).
OAuth setups can reuse `MINIMAX_OAUTH_TOKEN` instead.

2

[Navigate to header](https://docs.openclaw.ai/tools/minimax-search#)

Store the key

Set `MINIMAX_CODE_PLAN_KEY` in the Gateway environment, or configure via:

```
openclaw configure --section web
```

OpenClaw also accepts `MINIMAX_CODING_API_KEY`, `MINIMAX_OAUTH_TOKEN`, and
`MINIMAX_API_KEY` as env aliases. `MINIMAX_API_KEY` should point at a
search-enabled Token Plan credential; ordinary MiniMax model API keys may not
be accepted by the Token Plan search endpoint.

## [​](https://docs.openclaw.ai/tools/minimax-search\#config)  Config

```
{
  plugins: {
    entries: {
      minimax: {
        config: {
          webSearch: {
            apiKey: "sk-cp-...", // optional if a MiniMax Token Plan env var is set
            region: "global", // or "cn"
          },
        },
      },
    },
  },
  tools: {
    web: {
      search: {
        provider: "minimax",
      },
    },
  },
}
```

**Environment alternative:** set `MINIMAX_CODE_PLAN_KEY`, `MINIMAX_CODING_API_KEY`,
`MINIMAX_OAUTH_TOKEN`, or `MINIMAX_API_KEY` in the Gateway environment.
For a gateway install, put it in `~/.openclaw/.env`.

## [​](https://docs.openclaw.ai/tools/minimax-search\#region-selection)  Region selection

MiniMax Search uses these endpoints:

- Global: `https://api.minimax.io/v1/coding_plan/search`
- CN: `https://api.minimaxi.com/v1/coding_plan/search`

If `plugins.entries.minimax.config.webSearch.region` is unset, OpenClaw resolves
the region in this order:

1. `tools.web.search.minimax.region` / plugin-owned `webSearch.region`
2. `MINIMAX_API_HOST`
3. `models.providers.minimax.baseUrl`
4. `models.providers.minimax-portal.baseUrl`

That means CN onboarding or `MINIMAX_API_HOST=https://api.minimaxi.com/...`
automatically keeps MiniMax Search on the CN host too.Even when you authenticated MiniMax through the OAuth `minimax-portal` path,
web search still registers as provider id `minimax`; the OAuth provider base URL
is used as a region hint for CN/global host selection, and `MINIMAX_OAUTH_TOKEN`
can satisfy the MiniMax Search bearer credential.

## [​](https://docs.openclaw.ai/tools/minimax-search\#supported-parameters)  Supported parameters

MiniMax Search supports:

- `query`
- `count` (OpenClaw trims the returned result list to the requested count)

Provider-specific filters are not currently supported.

## [​](https://docs.openclaw.ai/tools/minimax-search\#related)  Related

- [Web Search overview](https://docs.openclaw.ai/tools/web) — all providers and auto-detection
- [MiniMax](https://docs.openclaw.ai/providers/minimax) — model, image, speech, and auth setup

[Kimi search](https://docs.openclaw.ai/tools/kimi-search) [Ollama web search](https://docs.openclaw.ai/tools/ollama-search)

Ctrl+I