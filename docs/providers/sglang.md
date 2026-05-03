---
source_url: https://docs.openclaw.ai/providers/sglang
title: "SGLang - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/sglang#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

SGLang

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/sglang#getting-started)
- [Model discovery (implicit provider)](https://docs.openclaw.ai/providers/sglang#model-discovery-implicit-provider)
- [Explicit configuration (manual models)](https://docs.openclaw.ai/providers/sglang#explicit-configuration-manual-models)
- [Advanced configuration](https://docs.openclaw.ai/providers/sglang#advanced-configuration)
- [Related](https://docs.openclaw.ai/providers/sglang#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

SGLang can serve open-source models via an **OpenAI-compatible** HTTP API.
OpenClaw can connect to SGLang using the `openai-completions` API.OpenClaw can also **auto-discover** available models from SGLang when you opt
in with `SGLANG_API_KEY` (any value works if your server does not enforce auth)
and you do not define an explicit `models.providers.sglang` entry.OpenClaw treats `sglang` as a local OpenAI-compatible provider that supports
streamed usage accounting, so status/context token counts can update from
`stream_options.include_usage` responses.

## [​](https://docs.openclaw.ai/providers/sglang\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/sglang#)

Start SGLang

Launch SGLang with an OpenAI-compatible server. Your base URL should expose
`/v1` endpoints (for example `/v1/models`, `/v1/chat/completions`). SGLang
commonly runs on:

- `http://127.0.0.1:30000/v1`

2

[Navigate to header](https://docs.openclaw.ai/providers/sglang#)

Set an API key

Any value works if no auth is configured on your server:

```
export SGLANG_API_KEY="sglang-local"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/sglang#)

Run onboarding or set a model directly

```
openclaw onboard
```

Or configure the model manually:

```
{
  agents: {
    defaults: {
      model: { primary: "sglang/your-model-id" },
    },
  },
}
```

## [​](https://docs.openclaw.ai/providers/sglang\#model-discovery-implicit-provider)  Model discovery (implicit provider)

When `SGLANG_API_KEY` is set (or an auth profile exists) and you **do not**
define `models.providers.sglang`, OpenClaw will query:

- `GET http://127.0.0.1:30000/v1/models`

and convert the returned IDs into model entries.

If you set `models.providers.sglang` explicitly, auto-discovery is skipped and
you must define models manually.

## [​](https://docs.openclaw.ai/providers/sglang\#explicit-configuration-manual-models)  Explicit configuration (manual models)

Use explicit config when:

- SGLang runs on a different host/port.
- You want to pin `contextWindow`/`maxTokens` values.
- Your server requires a real API key (or you want to control headers).

```
{
  models: {
    providers: {
      sglang: {
        baseUrl: "http://127.0.0.1:30000/v1",
        apiKey: "${SGLANG_API_KEY}",
        api: "openai-completions",
        models: [\
          {\
            id: "your-model-id",\
            name: "Local SGLang Model",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 128000,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/providers/sglang\#advanced-configuration)  Advanced configuration

Proxy-style behavior

SGLang is treated as a proxy-style OpenAI-compatible `/v1` backend, not a
native OpenAI endpoint.

| Behavior | SGLang |
| --- | --- |
| OpenAI-only request shaping | Not applied |
| `service_tier`, Responses `store`, prompt-cache hints | Not sent |
| Reasoning-compat payload shaping | Not applied |
| Hidden attribution headers (`originator`, `version`, `User-Agent`) | Not injected on custom SGLang base URLs |

Troubleshooting

**Server not reachable**Verify the server is running and responding:

```
curl http://127.0.0.1:30000/v1/models
```

**Auth errors**If requests fail with auth errors, set a real `SGLANG_API_KEY` that matches
your server configuration, or configure the provider explicitly under
`models.providers.sglang`.

If you run SGLang without authentication, any non-empty value for
`SGLANG_API_KEY` is sufficient to opt in to model discovery.

## [​](https://docs.openclaw.ai/providers/sglang\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config schema including provider entries.](https://docs.openclaw.ai/gateway/configuration-reference)

[SenseAudio](https://docs.openclaw.ai/providers/senseaudio) [StepFun](https://docs.openclaw.ai/providers/stepfun)

Ctrl+I