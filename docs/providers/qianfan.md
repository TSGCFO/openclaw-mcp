---
source_url: https://docs.openclaw.ai/providers/qianfan
title: "Qianfan - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/qianfan#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Qianfan

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/qianfan#getting-started)
- [Built-in catalog](https://docs.openclaw.ai/providers/qianfan#built-in-catalog)
- [Config example](https://docs.openclaw.ai/providers/qianfan#config-example)
- [Related](https://docs.openclaw.ai/providers/qianfan#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Qianfan is Baidu’s MaaS platform, providing a **unified API** that routes requests to many models behind a single
endpoint and API key. It is OpenAI-compatible, so most OpenAI SDKs work by switching the base URL.

| Property | Value |
| --- | --- |
| Provider | `qianfan` |
| Auth | `QIANFAN_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://qianfan.baidubce.com/v2` |

## [​](https://docs.openclaw.ai/providers/qianfan\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/qianfan#)

Create a Baidu Cloud account

Sign up or log in at the [Qianfan Console](https://console.bce.baidu.com/qianfan/ais/console/apiKey) and ensure you have Qianfan API access enabled.

2

[Navigate to header](https://docs.openclaw.ai/providers/qianfan#)

Generate an API key

Create a new application or select an existing one, then generate an API key. The key format is `bce-v3/ALTAK-...`.

3

[Navigate to header](https://docs.openclaw.ai/providers/qianfan#)

Run onboarding

```
openclaw onboard --auth-choice qianfan-api-key
```

4

[Navigate to header](https://docs.openclaw.ai/providers/qianfan#)

Verify the model is available

```
openclaw models list --provider qianfan
```

## [​](https://docs.openclaw.ai/providers/qianfan\#built-in-catalog)  Built-in catalog

| Model ref | Input | Context | Max output | Reasoning | Notes |
| --- | --- | --- | --- | --- | --- |
| `qianfan/deepseek-v3.2` | text | 98,304 | 32,768 | Yes | Default model |
| `qianfan/ernie-5.0-thinking-preview` | text, image | 119,000 | 64,000 | Yes | Multimodal |

The default bundled model ref is `qianfan/deepseek-v3.2`. You only need to override `models.providers.qianfan` when you need a custom base URL or model metadata.

## [​](https://docs.openclaw.ai/providers/qianfan\#config-example)  Config example

```
{
  env: { QIANFAN_API_KEY: "bce-v3/ALTAK-..." },
  agents: {
    defaults: {
      model: { primary: "qianfan/deepseek-v3.2" },
      models: {
        "qianfan/deepseek-v3.2": { alias: "QIANFAN" },
      },
    },
  },
  models: {
    providers: {
      qianfan: {
        baseUrl: "https://qianfan.baidubce.com/v2",
        api: "openai-completions",
        models: [\
          {\
            id: "deepseek-v3.2",\
            name: "DEEPSEEK V3.2",\
            reasoning: true,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 98304,\
            maxTokens: 32768,\
          },\
          {\
            id: "ernie-5.0-thinking-preview",\
            name: "ERNIE-5.0-Thinking-Preview",\
            reasoning: true,\
            input: ["text", "image"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 119000,\
            maxTokens: 64000,\
          },\
        ],
      },
    },
  },
}
```

Transport and compatibility

Qianfan runs through the OpenAI-compatible transport path, not native OpenAI request shaping. This means standard OpenAI SDK features work, but provider-specific parameters may not be forwarded.

Catalog and overrides

The bundled catalog currently includes `deepseek-v3.2` and `ernie-5.0-thinking-preview`. Add or override `models.providers.qianfan` only when you need a custom base URL or model metadata.

Model refs use the `qianfan/` prefix (for example `qianfan/deepseek-v3.2`).

Troubleshooting

- Ensure your API key starts with `bce-v3/ALTAK-` and has Qianfan API access enabled in the Baidu Cloud console.
- If models are not listed, confirm your account has the Qianfan service activated.
- The default base URL is `https://qianfan.baidubce.com/v2`. Only change it if you use a custom endpoint or proxy.

## [​](https://docs.openclaw.ai/providers/qianfan\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full OpenClaw configuration reference.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Agent setup** \\
\\
Configuring agent defaults and model assignments.](https://docs.openclaw.ai/concepts/agent)

[**Qianfan API docs** \\
\\
Official Qianfan API documentation.](https://cloud.baidu.com/doc/qianfan-api/s/3m7of64lb)

[Perplexity](https://docs.openclaw.ai/providers/perplexity-provider) [Qwen](https://docs.openclaw.ai/providers/qwen)

Ctrl+I