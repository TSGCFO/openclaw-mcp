---
source_url: https://docs.openclaw.ai/providers/models
title: "Model provider quickstart - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/models#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Overview

Model provider quickstart

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Model Providers](https://docs.openclaw.ai/providers/models#model-providers)
- [Quick start (two steps)](https://docs.openclaw.ai/providers/models#quick-start-two-steps)
- [Supported providers (starter set)](https://docs.openclaw.ai/providers/models#supported-providers-starter-set)
- [Additional bundled provider variants](https://docs.openclaw.ai/providers/models#additional-bundled-provider-variants)
- [Related](https://docs.openclaw.ai/providers/models#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/providers/models\#model-providers)  Model Providers

OpenClaw can use many LLM providers. Pick one, authenticate, then set the default
model as `provider/model`.

## [​](https://docs.openclaw.ai/providers/models\#quick-start-two-steps)  Quick start (two steps)

1. Authenticate with the provider (usually via `openclaw onboard`).
2. Set the default model:

```
{
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-6" } } },
}
```

## [​](https://docs.openclaw.ai/providers/models\#supported-providers-starter-set)  Supported providers (starter set)

- [Alibaba Model Studio](https://docs.openclaw.ai/providers/alibaba)
- [Amazon Bedrock](https://docs.openclaw.ai/providers/bedrock)
- [Anthropic (API + Claude CLI)](https://docs.openclaw.ai/providers/anthropic)
- [BytePlus (International)](https://docs.openclaw.ai/concepts/model-providers#byteplus-international)
- [Chutes](https://docs.openclaw.ai/providers/chutes)
- [ComfyUI](https://docs.openclaw.ai/providers/comfy)
- [Cloudflare AI Gateway](https://docs.openclaw.ai/providers/cloudflare-ai-gateway)
- [DeepInfra](https://docs.openclaw.ai/providers/deepinfra)
- [fal](https://docs.openclaw.ai/providers/fal)
- [Fireworks](https://docs.openclaw.ai/providers/fireworks)
- [GLM models](https://docs.openclaw.ai/providers/glm)
- [MiniMax](https://docs.openclaw.ai/providers/minimax)
- [Mistral](https://docs.openclaw.ai/providers/mistral)
- [Moonshot AI (Kimi + Kimi Coding)](https://docs.openclaw.ai/providers/moonshot)
- [OpenAI (API + Codex)](https://docs.openclaw.ai/providers/openai)
- [OpenCode (Zen + Go)](https://docs.openclaw.ai/providers/opencode)
- [OpenRouter](https://docs.openclaw.ai/providers/openrouter)
- [Qianfan](https://docs.openclaw.ai/providers/qianfan)
- [Qwen](https://docs.openclaw.ai/providers/qwen)
- [Runway](https://docs.openclaw.ai/providers/runway)
- [StepFun](https://docs.openclaw.ai/providers/stepfun)
- [Synthetic](https://docs.openclaw.ai/providers/synthetic)
- [Vercel AI Gateway](https://docs.openclaw.ai/providers/vercel-ai-gateway)
- [Venice (Venice AI)](https://docs.openclaw.ai/providers/venice)
- [xAI](https://docs.openclaw.ai/providers/xai)
- [Z.AI](https://docs.openclaw.ai/providers/zai)

## [​](https://docs.openclaw.ai/providers/models\#additional-bundled-provider-variants)  Additional bundled provider variants

- `anthropic-vertex` \- implicit Anthropic on Google Vertex support when Vertex credentials are available; no separate onboarding auth choice
- `copilot-proxy` \- local VS Code Copilot Proxy bridge; use `openclaw onboard --auth-choice copilot-proxy`
- `google-gemini-cli` \- unofficial Gemini CLI OAuth flow; requires a local `gemini` install (`brew install gemini-cli` or `npm install -g @google/gemini-cli`); default model `google-gemini-cli/gemini-3-flash-preview`; use `openclaw onboard --auth-choice google-gemini-cli` or `openclaw models auth login --provider google-gemini-cli --set-default`

For the full provider catalog (xAI, Groq, Mistral, etc.) and advanced configuration,
see [Model providers](https://docs.openclaw.ai/concepts/model-providers).

## [​](https://docs.openclaw.ai/providers/models\#related)  Related

- [Model selection](https://docs.openclaw.ai/concepts/model-providers)
- [Model failover](https://docs.openclaw.ai/concepts/model-failover)
- [Models CLI](https://docs.openclaw.ai/cli/models)

[Provider directory](https://docs.openclaw.ai/providers) [Models CLI](https://docs.openclaw.ai/concepts/models)

Ctrl+I