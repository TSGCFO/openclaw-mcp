---
source_url: https://docs.openclaw.ai/tools/ollama-search
title: "Ollama web search - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/tools/ollama-search#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Web tools

Ollama web search

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Setup](https://docs.openclaw.ai/tools/ollama-search#setup)
- [Config](https://docs.openclaw.ai/tools/ollama-search#config)
- [Notes](https://docs.openclaw.ai/tools/ollama-search#notes)
- [Related](https://docs.openclaw.ai/tools/ollama-search#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw supports **Ollama Web Search** as a bundled `web_search` provider. It
uses Ollama’s web-search API and returns structured results with titles, URLs,
and snippets.For local or self-hosted Ollama, this setup does not need an API key by
default. It does require:

- an Ollama host that is reachable from OpenClaw
- `ollama signin`

For direct hosted search, set the Ollama provider base URL to `https://ollama.com`
and provide a real `OLLAMA_API_KEY`.

## [​](https://docs.openclaw.ai/tools/ollama-search\#setup)  Setup

1

[Navigate to header](https://docs.openclaw.ai/tools/ollama-search#)

Start Ollama

Make sure Ollama is installed and running.

2

[Navigate to header](https://docs.openclaw.ai/tools/ollama-search#)

Sign in

Run:

```
ollama signin
```

3

[Navigate to header](https://docs.openclaw.ai/tools/ollama-search#)

Choose Ollama Web Search

Run:

```
openclaw configure --section web
```

Then select **Ollama Web Search** as the provider.

If you already use Ollama for models, Ollama Web Search reuses the same
configured host.

## [​](https://docs.openclaw.ai/tools/ollama-search\#config)  Config

```
{
  tools: {
    web: {
      search: {
        provider: "ollama",
      },
    },
  },
}
```

Optional Ollama host override:

```
{
  plugins: {
    entries: {
      ollama: {
        config: {
          webSearch: {
            baseUrl: "http://ollama-host:11434",
          },
        },
      },
    },
  },
}
```

If you already configure Ollama as a model provider, the web-search provider can
reuse that host instead:

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "http://ollama-host:11434",
      },
    },
  },
}
```

The Ollama model provider uses `baseUrl` as the canonical key. The web-search provider also honors `baseURL` on `models.providers.ollama` for compatibility with OpenAI SDK-style config examples.If no explicit Ollama base URL is set, OpenClaw uses `http://127.0.0.1:11434`.If your Ollama host expects bearer auth, OpenClaw reuses
`models.providers.ollama.apiKey` (or the matching env-backed provider auth)
for requests to that configured host.Direct hosted Ollama Web Search:

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "https://ollama.com",
        apiKey: "OLLAMA_API_KEY",
      },
    },
  },
  tools: {
    web: {
      search: {
        provider: "ollama",
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/tools/ollama-search\#notes)  Notes

- No web-search-specific API key field is required for this provider.
- If the Ollama host is auth-protected, OpenClaw reuses the normal Ollama
provider API key when present.
- If `baseUrl` is `https://ollama.com`, OpenClaw calls
`https://ollama.com/api/web_search` directly and sends the configured Ollama
API key as bearer auth.
- If the configured host does not expose web search and `OLLAMA_API_KEY` is set,
OpenClaw can fall back to `https://ollama.com/api/web_search` without sending
that env key to the local host.
- OpenClaw warns during setup if Ollama is unreachable or not signed in, but
it does not block selection.
- Runtime auto-detect can fall back to Ollama Web Search when no higher-priority
credentialed provider is configured.
- Local Ollama daemon hosts use the local proxy endpoint
`/api/experimental/web_search`, which signs and forwards to Ollama Cloud.
- `https://ollama.com` hosts use the public hosted endpoint
`/api/web_search` directly with bearer API-key auth.

## [​](https://docs.openclaw.ai/tools/ollama-search\#related)  Related

- [Web Search overview](https://docs.openclaw.ai/tools/web) — all providers and auto-detection
- [Ollama](https://docs.openclaw.ai/providers/ollama) — Ollama model setup and cloud/local modes

[MiniMax search](https://docs.openclaw.ai/tools/minimax-search) [Perplexity search](https://docs.openclaw.ai/tools/perplexity-search)

Ctrl+I