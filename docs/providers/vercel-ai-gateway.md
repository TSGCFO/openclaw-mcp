---
source_url: https://docs.openclaw.ai/providers/vercel-ai-gateway
title: "Vercel AI gateway - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/vercel-ai-gateway#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Vercel AI gateway

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/vercel-ai-gateway#getting-started)
- [Non-interactive example](https://docs.openclaw.ai/providers/vercel-ai-gateway#non-interactive-example)
- [Model ID shorthand](https://docs.openclaw.ai/providers/vercel-ai-gateway#model-id-shorthand)
- [Advanced configuration](https://docs.openclaw.ai/providers/vercel-ai-gateway#advanced-configuration)
- [Related](https://docs.openclaw.ai/providers/vercel-ai-gateway#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

The [Vercel AI Gateway](https://vercel.com/ai-gateway) provides a unified API to
access hundreds of models through a single endpoint.

| Property | Value |
| --- | --- |
| Provider | `vercel-ai-gateway` |
| Auth | `AI_GATEWAY_API_KEY` |
| API | Anthropic Messages compatible |
| Model catalog | Auto-discovered via `/v1/models` |

OpenClaw auto-discovers the Gateway `/v1/models` catalog, so
`/models vercel-ai-gateway` includes current model refs such as
`vercel-ai-gateway/openai/gpt-5.5` and
`vercel-ai-gateway/moonshotai/kimi-k2.6`.

## [​](https://docs.openclaw.ai/providers/vercel-ai-gateway\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/vercel-ai-gateway#)

Set the API key

Run onboarding and choose the AI Gateway auth option:

```
openclaw onboard --auth-choice ai-gateway-api-key
```

2

[Navigate to header](https://docs.openclaw.ai/providers/vercel-ai-gateway#)

Set a default model

Add the model to your OpenClaw config:

```
{
  agents: {
    defaults: {
      model: { primary: "vercel-ai-gateway/anthropic/claude-opus-4.6" },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/vercel-ai-gateway#)

Verify the model is available

```
openclaw models list --provider vercel-ai-gateway
```

## [​](https://docs.openclaw.ai/providers/vercel-ai-gateway\#non-interactive-example)  Non-interactive example

For scripted or CI setups, pass all values on the command line:

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice ai-gateway-api-key \
  --ai-gateway-api-key "$AI_GATEWAY_API_KEY"
```

## [​](https://docs.openclaw.ai/providers/vercel-ai-gateway\#model-id-shorthand)  Model ID shorthand

OpenClaw accepts Vercel Claude shorthand model refs and normalizes them at
runtime:

| Shorthand input | Normalized model ref |
| --- | --- |
| `vercel-ai-gateway/claude-opus-4.6` | `vercel-ai-gateway/anthropic/claude-opus-4.6` |
| `vercel-ai-gateway/opus-4.6` | `vercel-ai-gateway/anthropic/claude-opus-4-6` |

You can use either the shorthand or the fully qualified model ref in your
configuration. OpenClaw resolves the canonical form automatically.

## [​](https://docs.openclaw.ai/providers/vercel-ai-gateway\#advanced-configuration)  Advanced configuration

Environment variable for daemon processes

If the OpenClaw Gateway runs as a daemon (launchd/systemd), make sure
`AI_GATEWAY_API_KEY` is available to that process.

A key set only in `~/.profile` will not be visible to a launchd/systemd
daemon unless that environment is explicitly imported. Set the key in
`~/.openclaw/.env` or via `env.shellEnv` to ensure the gateway process can
read it.

Provider routing

Vercel AI Gateway routes requests to the upstream provider based on the model
ref prefix. For example, `vercel-ai-gateway/anthropic/claude-opus-4.6` routes
through Anthropic, while `vercel-ai-gateway/openai/gpt-5.5` routes through
OpenAI and `vercel-ai-gateway/moonshotai/kimi-k2.6` routes through
MoonshotAI. Your single `AI_GATEWAY_API_KEY` handles authentication for all
upstream providers.

Thinking levels

`/think` options follow trusted upstream model prefixes when OpenClaw knows
the upstream provider contract. `vercel-ai-gateway/anthropic/...` uses the
Claude thinking profile, including adaptive defaults for Claude 4.6 models.
`vercel-ai-gateway/openai/gpt-5.4`, `gpt-5.5`, and Codex-style refs expose
`/think xhigh` just like the direct OpenAI/OpenAI Codex providers. Other
namespaced refs keep the normal reasoning levels unless their catalog
metadata declares more.

## [​](https://docs.openclaw.ai/providers/vercel-ai-gateway\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Troubleshooting** \\
\\
General troubleshooting and FAQ.](https://docs.openclaw.ai/help/troubleshooting)

[Venice AI](https://docs.openclaw.ai/providers/venice) [vLLM](https://docs.openclaw.ai/providers/vllm)

Ctrl+I