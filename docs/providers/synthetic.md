---
source_url: https://docs.openclaw.ai/providers/synthetic
title: "Synthetic - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/synthetic#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Synthetic

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/synthetic#getting-started)
- [Config example](https://docs.openclaw.ai/providers/synthetic#config-example)
- [Built-in catalog](https://docs.openclaw.ai/providers/synthetic#built-in-catalog)
- [Related](https://docs.openclaw.ai/providers/synthetic#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

[Synthetic](https://synthetic.new/) exposes Anthropic-compatible endpoints.
OpenClaw registers it as the `synthetic` provider and uses the Anthropic
Messages API.

| Property | Value |
| --- | --- |
| Provider | `synthetic` |
| Auth | `SYNTHETIC_API_KEY` |
| API | Anthropic Messages |
| Base URL | `https://api.synthetic.new/anthropic` |

## [​](https://docs.openclaw.ai/providers/synthetic\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/synthetic#)

Get an API key

Obtain a `SYNTHETIC_API_KEY` from your Synthetic account, or let the
onboarding wizard prompt you for one.

2

[Navigate to header](https://docs.openclaw.ai/providers/synthetic#)

Run onboarding

```
openclaw onboard --auth-choice synthetic-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/synthetic#)

Verify the default model

After onboarding the default model is set to:

```
synthetic/hf:MiniMaxAI/MiniMax-M2.5
```

OpenClaw’s Anthropic client appends `/v1` to the base URL automatically, so use
`https://api.synthetic.new/anthropic` (not `/anthropic/v1`). If Synthetic
changes its base URL, override `models.providers.synthetic.baseUrl`.

## [​](https://docs.openclaw.ai/providers/synthetic\#config-example)  Config example

```
{
  env: { SYNTHETIC_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "synthetic/hf:MiniMaxAI/MiniMax-M2.5" },
      models: { "synthetic/hf:MiniMaxAI/MiniMax-M2.5": { alias: "MiniMax M2.5" } },
    },
  },
  models: {
    mode: "merge",
    providers: {
      synthetic: {
        baseUrl: "https://api.synthetic.new/anthropic",
        apiKey: "${SYNTHETIC_API_KEY}",
        api: "anthropic-messages",
        models: [\
          {\
            id: "hf:MiniMaxAI/MiniMax-M2.5",\
            name: "MiniMax M2.5",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 192000,\
            maxTokens: 65536,\
          },\
        ],
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/providers/synthetic\#built-in-catalog)  Built-in catalog

All Synthetic models use cost `0` (input/output/cache).

| Model ID | Context window | Max tokens | Reasoning | Input |
| --- | --- | --- | --- | --- |
| `hf:MiniMaxAI/MiniMax-M2.5` | 192,000 | 65,536 | no | text |
| `hf:moonshotai/Kimi-K2-Thinking` | 256,000 | 8,192 | yes | text |
| `hf:zai-org/GLM-4.7` | 198,000 | 128,000 | no | text |
| `hf:deepseek-ai/DeepSeek-R1-0528` | 128,000 | 8,192 | no | text |
| `hf:deepseek-ai/DeepSeek-V3-0324` | 128,000 | 8,192 | no | text |
| `hf:deepseek-ai/DeepSeek-V3.1` | 128,000 | 8,192 | no | text |
| `hf:deepseek-ai/DeepSeek-V3.1-Terminus` | 128,000 | 8,192 | no | text |
| `hf:deepseek-ai/DeepSeek-V3.2` | 159,000 | 8,192 | no | text |
| `hf:meta-llama/Llama-3.3-70B-Instruct` | 128,000 | 8,192 | no | text |
| `hf:meta-llama/Llama-4-Maverick-17B-128E-Instruct-FP8` | 524,000 | 8,192 | no | text |
| `hf:moonshotai/Kimi-K2-Instruct-0905` | 256,000 | 8,192 | no | text |
| `hf:moonshotai/Kimi-K2.5` | 256,000 | 8,192 | yes | text + image |
| `hf:openai/gpt-oss-120b` | 128,000 | 8,192 | no | text |
| `hf:Qwen/Qwen3-235B-A22B-Instruct-2507` | 256,000 | 8,192 | no | text |
| `hf:Qwen/Qwen3-Coder-480B-A35B-Instruct` | 256,000 | 8,192 | no | text |
| `hf:Qwen/Qwen3-VL-235B-A22B-Instruct` | 250,000 | 8,192 | no | text + image |
| `hf:zai-org/GLM-4.5` | 128,000 | 128,000 | no | text |
| `hf:zai-org/GLM-4.6` | 198,000 | 128,000 | no | text |
| `hf:zai-org/GLM-5` | 256,000 | 128,000 | yes | text + image |
| `hf:deepseek-ai/DeepSeek-V3` | 128,000 | 8,192 | no | text |
| `hf:Qwen/Qwen3-235B-A22B-Thinking-2507` | 256,000 | 8,192 | yes | text |

Model refs use the form `synthetic/<modelId>`. Use
`openclaw models list --provider synthetic` to see all models available on your
account.

Model allowlist

If you enable a model allowlist (`agents.defaults.models`), add every
Synthetic model you plan to use. Models not in the allowlist will be hidden
from the agent.

Base URL override

If Synthetic changes its API endpoint, override the base URL in your config:

```
{
  models: {
    providers: {
      synthetic: {
        baseUrl: "https://new-api.synthetic.new/anthropic",
      },
    },
  },
}
```

Remember that OpenClaw appends `/v1` automatically.

## [​](https://docs.openclaw.ai/providers/synthetic\#related)  Related

[**Model selection** \\
\\
Provider rules, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config schema including provider settings.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Synthetic** \\
\\
Synthetic dashboard and API docs.](https://synthetic.new/)

[StepFun](https://docs.openclaw.ai/providers/stepfun) [Tencent Cloud (TokenHub)](https://docs.openclaw.ai/providers/tencent)

Ctrl+I