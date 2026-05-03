---
source_url: https://docs.openclaw.ai/providers/cloudflare-ai-gateway
title: "Cloudflare AI gateway - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/cloudflare-ai-gateway#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Cloudflare AI gateway

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/cloudflare-ai-gateway#getting-started)
- [Non-interactive example](https://docs.openclaw.ai/providers/cloudflare-ai-gateway#non-interactive-example)
- [Advanced configuration](https://docs.openclaw.ai/providers/cloudflare-ai-gateway#advanced-configuration)
- [Related](https://docs.openclaw.ai/providers/cloudflare-ai-gateway#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Cloudflare AI Gateway sits in front of provider APIs and lets you add analytics, caching, and controls. For Anthropic, OpenClaw uses the Anthropic Messages API through your Gateway endpoint.

| Property | Value |
| --- | --- |
| Provider | `cloudflare-ai-gateway` |
| Base URL | `https://gateway.ai.cloudflare.com/v1/<account_id>/<gateway_id>/anthropic` |
| Default model | `cloudflare-ai-gateway/claude-sonnet-4-6` |
| API key | `CLOUDFLARE_AI_GATEWAY_API_KEY` (your provider API key for requests through the Gateway) |

For Anthropic models routed through Cloudflare AI Gateway, use your **Anthropic API key** as the provider key.

When thinking is enabled for Anthropic Messages models, OpenClaw strips trailing
assistant prefill turns before sending the payload through Cloudflare AI Gateway.
Anthropic rejects response prefilling with extended thinking, while ordinary
non-thinking prefill remains available.

## [​](https://docs.openclaw.ai/providers/cloudflare-ai-gateway\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/cloudflare-ai-gateway#)

Set the provider API key and Gateway details

Run onboarding and choose the Cloudflare AI Gateway auth option:

```
openclaw onboard --auth-choice cloudflare-ai-gateway-api-key
```

This prompts for your account ID, gateway ID, and API key.

2

[Navigate to header](https://docs.openclaw.ai/providers/cloudflare-ai-gateway#)

Set a default model

Add the model to your OpenClaw config:

```
{
  agents: {
    defaults: {
      model: { primary: "cloudflare-ai-gateway/claude-sonnet-4-6" },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/cloudflare-ai-gateway#)

Verify the model is available

```
openclaw models list --provider cloudflare-ai-gateway
```

## [​](https://docs.openclaw.ai/providers/cloudflare-ai-gateway\#non-interactive-example)  Non-interactive example

For scripted or CI setups, pass all values on the command line:

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice cloudflare-ai-gateway-api-key \
  --cloudflare-ai-gateway-account-id "your-account-id" \
  --cloudflare-ai-gateway-gateway-id "your-gateway-id" \
  --cloudflare-ai-gateway-api-key "$CLOUDFLARE_AI_GATEWAY_API_KEY"
```

## [​](https://docs.openclaw.ai/providers/cloudflare-ai-gateway\#advanced-configuration)  Advanced configuration

Authenticated gateways

If you enabled Gateway authentication in Cloudflare, add the `cf-aig-authorization` header. This is **in addition to** your provider API key.

```
{
  models: {
    providers: {
      "cloudflare-ai-gateway": {
        headers: {
          "cf-aig-authorization": "Bearer <cloudflare-ai-gateway-token>",
        },
      },
    },
  },
}
```

The `cf-aig-authorization` header authenticates with the Cloudflare Gateway itself, while the provider API key (for example, your Anthropic key) authenticates with the upstream provider.

Environment note

If the Gateway runs as a daemon (launchd/systemd), make sure `CLOUDFLARE_AI_GATEWAY_API_KEY` is available to that process.

A key sitting only in `~/.profile` will not help a launchd/systemd daemon unless that environment is imported there as well. Set the key in `~/.openclaw/.env` or via `env.shellEnv` to ensure the gateway process can read it.

## [​](https://docs.openclaw.ai/providers/cloudflare-ai-gateway\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Troubleshooting** \\
\\
General troubleshooting and FAQ.](https://docs.openclaw.ai/help/troubleshooting)

[Claude Max API proxy](https://docs.openclaw.ai/providers/claude-max-api-proxy) [ComfyUI](https://docs.openclaw.ai/providers/comfy)

Ctrl+I