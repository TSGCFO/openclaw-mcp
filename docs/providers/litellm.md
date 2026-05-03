---
source_url: https://docs.openclaw.ai/providers/litellm
title: "LiteLLM - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/litellm#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

LiteLLM

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Quick start](https://docs.openclaw.ai/providers/litellm#quick-start)
- [Configuration](https://docs.openclaw.ai/providers/litellm#configuration)
- [Environment variables](https://docs.openclaw.ai/providers/litellm#environment-variables)
- [Config file](https://docs.openclaw.ai/providers/litellm#config-file)
- [Advanced configuration](https://docs.openclaw.ai/providers/litellm#advanced-configuration)
- [Image generation](https://docs.openclaw.ai/providers/litellm#image-generation)
- [Related](https://docs.openclaw.ai/providers/litellm#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

[LiteLLM](https://litellm.ai/) is an open-source LLM gateway that provides a unified API to 100+ model providers. Route OpenClaw through LiteLLM to get centralized cost tracking, logging, and the flexibility to switch backends without changing your OpenClaw config.

**Why use LiteLLM with OpenClaw?**

- **Cost tracking** — See exactly what OpenClaw spends across all models
- **Model routing** — Switch between Claude, GPT-4, Gemini, Bedrock without config changes
- **Virtual keys** — Create keys with spend limits for OpenClaw
- **Logging** — Full request/response logs for debugging
- **Fallbacks** — Automatic failover if your primary provider is down

## [​](https://docs.openclaw.ai/providers/litellm\#quick-start)  Quick start

- Onboarding (recommended)

- Manual setup


**Best for:** fastest path to a working LiteLLM setup.

1

[Navigate to header](https://docs.openclaw.ai/providers/litellm#)

Run onboarding

```
openclaw onboard --auth-choice litellm-api-key
```

For non-interactive setup against a remote proxy, pass the proxy URL explicitly:

```
openclaw onboard --non-interactive --auth-choice litellm-api-key --litellm-api-key "$LITELLM_API_KEY" --custom-base-url "https://litellm.example/v1"
```

**Best for:** full control over installation and config.

1

[Navigate to header](https://docs.openclaw.ai/providers/litellm#)

Start LiteLLM Proxy

```
pip install 'litellm[proxy]'
litellm --model claude-opus-4-6
```

2

[Navigate to header](https://docs.openclaw.ai/providers/litellm#)

Point OpenClaw to LiteLLM

```
export LITELLM_API_KEY="your-litellm-key"

openclaw
```

That’s it. OpenClaw now routes through LiteLLM.

## [​](https://docs.openclaw.ai/providers/litellm\#configuration)  Configuration

### [​](https://docs.openclaw.ai/providers/litellm\#environment-variables)  Environment variables

```
export LITELLM_API_KEY="sk-litellm-key"
```

### [​](https://docs.openclaw.ai/providers/litellm\#config-file)  Config file

```
{
  models: {
    providers: {
      litellm: {
        baseUrl: "http://localhost:4000",
        apiKey: "${LITELLM_API_KEY}",
        api: "openai-completions",
        models: [\
          {\
            id: "claude-opus-4-6",\
            name: "Claude Opus 4.6",\
            reasoning: true,\
            input: ["text", "image"],\
            contextWindow: 200000,\
            maxTokens: 64000,\
          },\
          {\
            id: "gpt-4o",\
            name: "GPT-4o",\
            reasoning: false,\
            input: ["text", "image"],\
            contextWindow: 128000,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
  agents: {
    defaults: {
      model: { primary: "litellm/claude-opus-4-6" },
    },
  },
}
```

## [​](https://docs.openclaw.ai/providers/litellm\#advanced-configuration)  Advanced configuration

### [​](https://docs.openclaw.ai/providers/litellm\#image-generation)  Image generation

LiteLLM can also back the `image_generate` tool through OpenAI-compatible
`/images/generations` and `/images/edits` routes. Configure a LiteLLM image
model under `agents.defaults.imageGenerationModel`:

```
{
  models: {
    providers: {
      litellm: {
        baseUrl: "http://localhost:4000",
        apiKey: "${LITELLM_API_KEY}",
      },
    },
  },
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "litellm/gpt-image-2",
        timeoutMs: 180_000,
      },
    },
  },
}
```

Loopback LiteLLM URLs such as `http://localhost:4000` work without a global
private-network override. For a LAN-hosted proxy, set
`models.providers.litellm.request.allowPrivateNetwork: true` because the API key
will be sent to the configured proxy host.

Virtual keys

Create a dedicated key for OpenClaw with spend limits:

```
curl -X POST "http://localhost:4000/key/generate" \
  -H "Authorization: Bearer $LITELLM_MASTER_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "key_alias": "openclaw",
    "max_budget": 50.00,
    "budget_duration": "monthly"
  }'
```

Use the generated key as `LITELLM_API_KEY`.

Model routing

LiteLLM can route model requests to different backends. Configure in your LiteLLM `config.yaml`:

```
model_list:
  - model_name: claude-opus-4-6
    litellm_params:
      model: claude-opus-4-6
      api_key: os.environ/ANTHROPIC_API_KEY

  - model_name: gpt-4o
    litellm_params:
      model: gpt-4o
      api_key: os.environ/OPENAI_API_KEY
```

OpenClaw keeps requesting `claude-opus-4-6` — LiteLLM handles the routing.

Viewing usage

Check LiteLLM’s dashboard or API:

```
# Key info
curl "http://localhost:4000/key/info" \
  -H "Authorization: Bearer sk-litellm-key"

# Spend logs
curl "http://localhost:4000/spend/logs" \
  -H "Authorization: Bearer $LITELLM_MASTER_KEY"
```

Proxy behavior notes

- LiteLLM runs on `http://localhost:4000` by default
- OpenClaw connects through LiteLLM’s proxy-style OpenAI-compatible `/v1`
endpoint
- Native OpenAI-only request shaping does not apply through LiteLLM:
no `service_tier`, no Responses `store`, no prompt-cache hints, and no
OpenAI reasoning-compat payload shaping
- Hidden OpenClaw attribution headers (`originator`, `version`, `User-Agent`)
are not injected on custom LiteLLM base URLs

For general provider configuration and failover behavior, see [Model Providers](https://docs.openclaw.ai/concepts/model-providers).

## [​](https://docs.openclaw.ai/providers/litellm\#related)  Related

[**LiteLLM Docs** \\
\\
Official LiteLLM documentation and API reference.](https://docs.litellm.ai/)

[**Model selection** \\
\\
Overview of all providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration** \\
\\
Full config reference.](https://docs.openclaw.ai/gateway/configuration)

[**Model selection** \\
\\
How to choose and configure models.](https://docs.openclaw.ai/concepts/models)

[Kilocode](https://docs.openclaw.ai/providers/kilocode) [LM Studio](https://docs.openclaw.ai/providers/lmstudio)

Ctrl+I