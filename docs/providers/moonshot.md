---
source_url: https://docs.openclaw.ai/providers/moonshot
title: "Moonshot AI - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/moonshot#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Moonshot AI

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Built-in model catalog](https://docs.openclaw.ai/providers/moonshot#built-in-model-catalog)
- [Getting started](https://docs.openclaw.ai/providers/moonshot#getting-started)
- [Config example](https://docs.openclaw.ai/providers/moonshot#config-example)
- [Kimi web search](https://docs.openclaw.ai/providers/moonshot#kimi-web-search)
- [Advanced configuration](https://docs.openclaw.ai/providers/moonshot#advanced-configuration)
- [Related](https://docs.openclaw.ai/providers/moonshot#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Moonshot provides the Kimi API with OpenAI-compatible endpoints. Configure the
provider and set the default model to `moonshot/kimi-k2.6`, or use
Kimi Coding with `kimi/kimi-code`.

Moonshot and Kimi Coding are **separate providers**. Keys are not interchangeable, endpoints differ, and model refs differ (`moonshot/...` vs `kimi/...`).

## [​](https://docs.openclaw.ai/providers/moonshot\#built-in-model-catalog)  Built-in model catalog

| Model ref | Name | Reasoning | Input | Context | Max output |
| --- | --- | --- | --- | --- | --- |
| `moonshot/kimi-k2.6` | Kimi K2.6 | No | text, image | 262,144 | 262,144 |
| `moonshot/kimi-k2.5` | Kimi K2.5 | No | text, image | 262,144 | 262,144 |
| `moonshot/kimi-k2-thinking` | Kimi K2 Thinking | Yes | text | 262,144 | 262,144 |
| `moonshot/kimi-k2-thinking-turbo` | Kimi K2 Thinking Turbo | Yes | text | 262,144 | 262,144 |
| `moonshot/kimi-k2-turbo` | Kimi K2 Turbo | No | text | 256,000 | 16,384 |

Bundled cost estimates for current Moonshot-hosted K2 models use Moonshot’s
published pay-as-you-go rates: Kimi K2.6 is 0.16/MTokcachehit,0.16/MTok cache hit,
0.16/MTokcachehit,0.95/MTok input, and 4.00/MTokoutput;KimiK2.5is4.00/MTok output; Kimi K2.5 is 4.00/MTokoutput;KimiK2.5is0.10/MTok cache hit,
0.60/MTokinput,and0.60/MTok input, and 0.60/MTokinput,and3.00/MTok output. Other legacy catalog entries keep
zero-cost placeholders unless you override them in config.

## [​](https://docs.openclaw.ai/providers/moonshot\#getting-started)  Getting started

Choose your provider and follow the setup steps.

- Moonshot API

- Kimi Coding


**Best for:** Kimi K2 models via the Moonshot Open Platform.

1

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Choose your endpoint region

| Auth choice | Endpoint | Region |
| --- | --- | --- |
| `moonshot-api-key` | `https://api.moonshot.ai/v1` | International |
| `moonshot-api-key-cn` | `https://api.moonshot.cn/v1` | China |

2

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Run onboarding

```
openclaw onboard --auth-choice moonshot-api-key
```

Or for the China endpoint:

```
openclaw onboard --auth-choice moonshot-api-key-cn
```

3

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Set a default model

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

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Verify models are available

```
openclaw models list --provider moonshot
```

5

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Run a live smoke test

Use an isolated state dir when you want to verify model access and cost
tracking without touching your normal sessions:

```
OPENCLAW_CONFIG_PATH=/tmp/openclaw-kimi/openclaw.json \
OPENCLAW_STATE_DIR=/tmp/openclaw-kimi \
openclaw agent --local \
  --session-id live-kimi-cost \
  --message 'Reply exactly: KIMI_LIVE_OK' \
  --thinking off \
  --json
```

The JSON response should report `provider: "moonshot"` and
`model: "kimi-k2.6"`. The assistant transcript entry stores normalized
token usage plus estimated cost under `usage.cost` when Moonshot returns
usage metadata.

### [​](https://docs.openclaw.ai/providers/moonshot\#config-example)  Config example

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
        apiKey: "${MOONSHOT_API_KEY}",
        api: "openai-completions",
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

**Best for:** code-focused tasks via the Kimi Coding endpoint.

Kimi Coding uses a different API key and provider prefix (`kimi/...`) than Moonshot (`moonshot/...`). Legacy model ref `kimi/k2p5` remains accepted as a compatibility id.

1

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Run onboarding

```
openclaw onboard --auth-choice kimi-code-api-key
```

2

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Set a default model

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

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Verify the model is available

```
openclaw models list --provider kimi
```

### [​](https://docs.openclaw.ai/providers/moonshot\#config-example-2)  Config example

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

## [​](https://docs.openclaw.ai/providers/moonshot\#kimi-web-search)  Kimi web search

OpenClaw also ships **Kimi** as a `web_search` provider, backed by Moonshot web
search.

1

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Run interactive web search setup

```
openclaw configure --section web
```

Choose **Kimi** in the web-search section to store
`plugins.entries.moonshot.config.webSearch.*`.

2

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Configure the web search region and model

Interactive setup prompts for:

| Setting | Options |
| --- | --- |
| API region | `https://api.moonshot.ai/v1` (international) or `https://api.moonshot.cn/v1` (China) |
| Web search model | Defaults to `kimi-k2.6` |

Config lives under `plugins.entries.moonshot.config.webSearch`:

```
{
  plugins: {
    entries: {
      moonshot: {
        config: {
          webSearch: {
            apiKey: "sk-...", // or use KIMI_API_KEY / MOONSHOT_API_KEY
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

## [​](https://docs.openclaw.ai/providers/moonshot\#advanced-configuration)  Advanced configuration

Native thinking mode

Moonshot Kimi supports binary native thinking:

- `thinking: { type: "enabled" }`
- `thinking: { type: "disabled" }`

Configure it per model via `agents.defaults.models.<provider/model>.params`:

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

OpenClaw also maps runtime `/think` levels for Moonshot:

| `/think` level | Moonshot behavior |
| --- | --- |
| `/think off` | `thinking.type=disabled` |
| Any non-off level | `thinking.type=enabled` |

When Moonshot thinking is enabled, `tool_choice` must be `auto` or `none`. OpenClaw normalizes incompatible `tool_choice` values to `auto` for compatibility.

Kimi K2.6 also accepts an optional `thinking.keep` field that controls
multi-turn retention of `reasoning_content`. Set it to `"all"` to keep full
reasoning across turns; omit it (or leave it `null`) to use the server
default strategy. OpenClaw only forwards `thinking.keep` for
`moonshot/kimi-k2.6` and strips it from other models.

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

Tool call id sanitization

Moonshot Kimi serves tool\_call ids shaped like `functions.<name>:<index>`. OpenClaw preserves them unchanged so multi-turn tool calls keep working.To force strict sanitization on a custom OpenAI-compatible provider, set `sanitizeToolCallIds: true`:

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

Streaming usage compatibility

Native Moonshot endpoints (`https://api.moonshot.ai/v1` and
`https://api.moonshot.cn/v1`) advertise streaming usage compatibility on the
shared `openai-completions` transport. OpenClaw keys that off endpoint
capabilities, so compatible custom provider ids targeting the same native
Moonshot hosts inherit the same streaming-usage behavior.With the bundled K2.6 pricing, streamed usage that includes input, output,
and cache-read tokens is also converted into local estimated USD cost for
`/status`, `/usage full`, `/usage cost`, and transcript-backed session
accounting.

Endpoint and model ref reference

| Provider | Model ref prefix | Endpoint | Auth env var |
| --- | --- | --- | --- |
| Moonshot | `moonshot/` | `https://api.moonshot.ai/v1` | `MOONSHOT_API_KEY` |
| Moonshot CN | `moonshot/` | `https://api.moonshot.cn/v1` | `MOONSHOT_API_KEY` |
| Kimi Coding | `kimi/` | Kimi Coding endpoint | `KIMI_API_KEY` |
| Web search | N/A | Same as Moonshot API region | `KIMI_API_KEY` or `MOONSHOT_API_KEY` |

- Kimi web search uses `KIMI_API_KEY` or `MOONSHOT_API_KEY`, and defaults to `https://api.moonshot.ai/v1` with model `kimi-k2.6`.
- Override pricing and context metadata in `models.providers` if needed.
- If Moonshot publishes different context limits for a model, adjust `contextWindow` accordingly.

## [​](https://docs.openclaw.ai/providers/moonshot\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Web search** \\
\\
Configuring web search providers including Kimi.](https://docs.openclaw.ai/tools/web)

[**Configuration reference** \\
\\
Full config schema for providers, models, and plugins.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Moonshot Open Platform** \\
\\
Moonshot API key management and documentation.](https://platform.moonshot.ai/)

[Mistral](https://docs.openclaw.ai/providers/mistral) [NVIDIA](https://docs.openclaw.ai/providers/nvidia)

Ctrl+I