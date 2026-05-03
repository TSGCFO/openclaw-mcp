---
source_url: https://docs.openclaw.ai/providers/huggingface
title: "Hugging Face (inference) - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/huggingface#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Hugging Face (inference)

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/huggingface#getting-started)
- [Non-interactive setup](https://docs.openclaw.ai/providers/huggingface#non-interactive-setup)
- [Model IDs](https://docs.openclaw.ai/providers/huggingface#model-ids)
- [Advanced configuration](https://docs.openclaw.ai/providers/huggingface#advanced-configuration)
- [Related](https://docs.openclaw.ai/providers/huggingface#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

[Hugging Face Inference Providers](https://huggingface.co/docs/inference-providers) offer OpenAI-compatible chat completions through a single router API. You get access to many models (DeepSeek, Llama, and more) with one token. OpenClaw uses the **OpenAI-compatible endpoint** (chat completions only); for text-to-image, embeddings, or speech use the [HF inference clients](https://huggingface.co/docs/api-inference/quicktour) directly.

- Provider: `huggingface`
- Auth: `HUGGINGFACE_HUB_TOKEN` or `HF_TOKEN` (fine-grained token with **Make calls to Inference Providers**)
- API: OpenAI-compatible (`https://router.huggingface.co/v1`)
- Billing: Single HF token; [pricing](https://huggingface.co/docs/inference-providers/pricing) follows provider rates with a free tier.

## [​](https://docs.openclaw.ai/providers/huggingface\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/huggingface#)

Create a fine-grained token

Go to [Hugging Face Settings Tokens](https://huggingface.co/settings/tokens/new?ownUserPermissions=inference.serverless.write&tokenType=fineGrained) and create a new fine-grained token.

The token must have the **Make calls to Inference Providers** permission enabled or API requests will be rejected.

2

[Navigate to header](https://docs.openclaw.ai/providers/huggingface#)

Run onboarding

Choose **Hugging Face** in the provider dropdown, then enter your API key when prompted:

```
openclaw onboard --auth-choice huggingface-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/huggingface#)

Select a default model

In the **Default Hugging Face model** dropdown, pick the model you want. The list is loaded from the Inference API when you have a valid token; otherwise a built-in list is shown. Your choice is saved as the default model.You can also set or change the default model later in config:

```
{
  agents: {
    defaults: {
      model: { primary: "huggingface/deepseek-ai/DeepSeek-R1" },
    },
  },
}
```

4

[Navigate to header](https://docs.openclaw.ai/providers/huggingface#)

Verify the model is available

```
openclaw models list --provider huggingface
```

### [​](https://docs.openclaw.ai/providers/huggingface\#non-interactive-setup)  Non-interactive setup

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice huggingface-api-key \
  --huggingface-api-key "$HF_TOKEN"
```

This will set `huggingface/deepseek-ai/DeepSeek-R1` as the default model.

## [​](https://docs.openclaw.ai/providers/huggingface\#model-ids)  Model IDs

Model refs use the form `huggingface/<org>/<model>` (Hub-style IDs). The list below is from **GET**`https://router.huggingface.co/v1/models`; your catalog may include more.

| Model | Ref (prefix with `huggingface/`) |
| --- | --- |
| DeepSeek R1 | `deepseek-ai/DeepSeek-R1` |
| DeepSeek V3.2 | `deepseek-ai/DeepSeek-V3.2` |
| Qwen3 8B | `Qwen/Qwen3-8B` |
| Qwen2.5 7B Instruct | `Qwen/Qwen2.5-7B-Instruct` |
| Qwen3 32B | `Qwen/Qwen3-32B` |
| Llama 3.3 70B Instruct | `meta-llama/Llama-3.3-70B-Instruct` |
| Llama 3.1 8B Instruct | `meta-llama/Llama-3.1-8B-Instruct` |
| GPT-OSS 120B | `openai/gpt-oss-120b` |
| GLM 4.7 | `zai-org/GLM-4.7` |
| Kimi K2.5 | `moonshotai/Kimi-K2.5` |

You can append `:fastest` or `:cheapest` to any model id. Set your default order in [Inference Provider settings](https://hf.co/settings/inference-providers); see [Inference Providers](https://huggingface.co/docs/inference-providers) and **GET**`https://router.huggingface.co/v1/models` for the full list.

## [​](https://docs.openclaw.ai/providers/huggingface\#advanced-configuration)  Advanced configuration

Model discovery and onboarding dropdown

OpenClaw discovers models by calling the **Inference endpoint directly**:

```
GET https://router.huggingface.co/v1/models
```

(Optional: send `Authorization: Bearer $HUGGINGFACE_HUB_TOKEN` or `$HF_TOKEN` for the full list; some endpoints return a subset without auth.) The response is OpenAI-style `{ "object": "list", "data": [ { "id": "Qwen/Qwen3-8B", "owned_by": "Qwen", ... }, ... ] }`.When you configure a Hugging Face API key (via onboarding, `HUGGINGFACE_HUB_TOKEN`, or `HF_TOKEN`), OpenClaw uses this GET to discover available chat-completion models. During **interactive setup**, after you enter your token you see a **Default Hugging Face model** dropdown populated from that list (or the built-in catalog if the request fails). At runtime (e.g. Gateway startup), when a key is present, OpenClaw again calls **GET**`https://router.huggingface.co/v1/models` to refresh the catalog. The list is merged with a built-in catalog (for metadata like context window and cost). If the request fails or no key is set, only the built-in catalog is used.

Model names, aliases, and policy suffixes

- **Name from API:** The model display name is **hydrated from GET /v1/models** when the API returns `name`, `title`, or `display_name`; otherwise it is derived from the model id (e.g. `deepseek-ai/DeepSeek-R1` becomes “DeepSeek R1”).
- **Override display name:** You can set a custom label per model in config so it appears the way you want in the CLI and UI:

```
{
  agents: {
    defaults: {
      models: {
        "huggingface/deepseek-ai/DeepSeek-R1": { alias: "DeepSeek R1 (fast)" },
        "huggingface/deepseek-ai/DeepSeek-R1:cheapest": { alias: "DeepSeek R1 (cheap)" },
      },
    },
  },
}
```

- **Policy suffixes:** OpenClaw’s bundled Hugging Face docs and helpers currently treat these two suffixes as the built-in policy variants:

  - **`:fastest`** — highest throughput.
  - **`:cheapest`** — lowest cost per output token.

You can add these as separate entries in `models.providers.huggingface.models` or set `model.primary` with the suffix. You can also set your default provider order in [Inference Provider settings](https://hf.co/settings/inference-providers) (no suffix = use that order).
- **Config merge:** Existing entries in `models.providers.huggingface.models` (e.g. in `models.json`) are kept when config is merged. So any custom `name`, `alias`, or model options you set there are preserved.

Environment and daemon setup

If the Gateway runs as a daemon (launchd/systemd), make sure `HUGGINGFACE_HUB_TOKEN` or `HF_TOKEN` is available to that process (for example, in `~/.openclaw/.env` or via `env.shellEnv`).

OpenClaw accepts both `HUGGINGFACE_HUB_TOKEN` and `HF_TOKEN` as env var aliases. Either one works; if both are set, `HUGGINGFACE_HUB_TOKEN` takes precedence.

Config: DeepSeek R1 with Qwen fallback

```
{
  agents: {
    defaults: {
      model: {
        primary: "huggingface/deepseek-ai/DeepSeek-R1",
        fallbacks: ["huggingface/Qwen/Qwen3-8B"],
      },
      models: {
        "huggingface/deepseek-ai/DeepSeek-R1": { alias: "DeepSeek R1" },
        "huggingface/Qwen/Qwen3-8B": { alias: "Qwen3 8B" },
      },
    },
  },
}
```

Config: Qwen with cheapest and fastest variants

```
{
  agents: {
    defaults: {
      model: { primary: "huggingface/Qwen/Qwen3-8B" },
      models: {
        "huggingface/Qwen/Qwen3-8B": { alias: "Qwen3 8B" },
        "huggingface/Qwen/Qwen3-8B:cheapest": { alias: "Qwen3 8B (cheapest)" },
        "huggingface/Qwen/Qwen3-8B:fastest": { alias: "Qwen3 8B (fastest)" },
      },
    },
  },
}
```

Config: DeepSeek + Llama + GPT-OSS with aliases

```
{
  agents: {
    defaults: {
      model: {
        primary: "huggingface/deepseek-ai/DeepSeek-V3.2",
        fallbacks: [\
          "huggingface/meta-llama/Llama-3.3-70B-Instruct",\
          "huggingface/openai/gpt-oss-120b",\
        ],
      },
      models: {
        "huggingface/deepseek-ai/DeepSeek-V3.2": { alias: "DeepSeek V3.2" },
        "huggingface/meta-llama/Llama-3.3-70B-Instruct": { alias: "Llama 3.3 70B" },
        "huggingface/openai/gpt-oss-120b": { alias: "GPT-OSS 120B" },
      },
    },
  },
}
```

Config: Multiple Qwen and DeepSeek with policy suffixes

```
{
  agents: {
    defaults: {
      model: { primary: "huggingface/Qwen/Qwen2.5-7B-Instruct:cheapest" },
      models: {
        "huggingface/Qwen/Qwen2.5-7B-Instruct": { alias: "Qwen2.5 7B" },
        "huggingface/Qwen/Qwen2.5-7B-Instruct:cheapest": { alias: "Qwen2.5 7B (cheap)" },
        "huggingface/deepseek-ai/DeepSeek-R1:fastest": { alias: "DeepSeek R1 (fast)" },
        "huggingface/meta-llama/Llama-3.1-8B-Instruct": { alias: "Llama 3.1 8B" },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/providers/huggingface\#related)  Related

[**Model selection** \\
\\
Overview of all providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Model selection** \\
\\
How to choose and configure models.](https://docs.openclaw.ai/concepts/models)

[**Inference Providers docs** \\
\\
Official Hugging Face Inference Providers documentation.](https://huggingface.co/docs/inference-providers)

[**Configuration** \\
\\
Full config reference.](https://docs.openclaw.ai/gateway/configuration)

[Groq](https://docs.openclaw.ai/providers/groq) [Inferrs](https://docs.openclaw.ai/providers/inferrs)

Ctrl+I