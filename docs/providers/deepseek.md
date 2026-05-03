---
source_url: https://docs.openclaw.ai/providers/deepseek
title: "DeepSeek - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/deepseek#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

DeepSeek

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Getting started](https://docs.openclaw.ai/providers/deepseek#getting-started)
- [Built-in catalog](https://docs.openclaw.ai/providers/deepseek#built-in-catalog)
- [Thinking and tools](https://docs.openclaw.ai/providers/deepseek#thinking-and-tools)
- [Live testing](https://docs.openclaw.ai/providers/deepseek#live-testing)
- [Config example](https://docs.openclaw.ai/providers/deepseek#config-example)
- [Related](https://docs.openclaw.ai/providers/deepseek#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

[DeepSeek](https://www.deepseek.com/) provides powerful AI models with an OpenAI-compatible API.

| Property | Value |
| --- | --- |
| Provider | `deepseek` |
| Auth | `DEEPSEEK_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://api.deepseek.com` |

## [​](https://docs.openclaw.ai/providers/deepseek\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/deepseek#)

Get your API key

Create an API key at [platform.deepseek.com](https://platform.deepseek.com/api_keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/deepseek#)

Run onboarding

```
openclaw onboard --auth-choice deepseek-api-key
```

This will prompt for your API key and set `deepseek/deepseek-v4-flash` as the default model.

3

[Navigate to header](https://docs.openclaw.ai/providers/deepseek#)

Verify models are available

```
openclaw models list --provider deepseek
```

To inspect the bundled static catalog without requiring a running Gateway,
use:

```
openclaw models list --all --provider deepseek
```

Non-interactive setup

For scripted or headless installations, pass all flags directly:

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice deepseek-api-key \
  --deepseek-api-key "$DEEPSEEK_API_KEY" \
  --skip-health \
  --accept-risk
```

If the Gateway runs as a daemon (launchd/systemd), make sure `DEEPSEEK_API_KEY`
is available to that process (for example, in `~/.openclaw/.env` or via
`env.shellEnv`).

## [​](https://docs.openclaw.ai/providers/deepseek\#built-in-catalog)  Built-in catalog

| Model ref | Name | Input | Context | Max output | Notes |
| --- | --- | --- | --- | --- | --- |
| `deepseek/deepseek-v4-flash` | DeepSeek V4 Flash | text | 1,000,000 | 384,000 | Default model; V4 thinking-capable surface |
| `deepseek/deepseek-v4-pro` | DeepSeek V4 Pro | text | 1,000,000 | 384,000 | V4 thinking-capable surface |
| `deepseek/deepseek-chat` | DeepSeek Chat | text | 131,072 | 8,192 | DeepSeek V3.2 non-thinking surface |
| `deepseek/deepseek-reasoner` | DeepSeek Reasoner | text | 131,072 | 65,536 | Reasoning-enabled V3.2 surface |

V4 models support DeepSeek’s `thinking` control. OpenClaw also replays
DeepSeek `reasoning_content` on follow-up turns so thinking sessions with tool
calls can continue.
Use `/think xhigh` or `/think max` with DeepSeek V4 models to request DeepSeek’s
maximum `reasoning_effort`.

## [​](https://docs.openclaw.ai/providers/deepseek\#thinking-and-tools)  Thinking and tools

DeepSeek V4 thinking sessions have a stricter replay contract than most
OpenAI-compatible providers: after a thinking-enabled turn uses tools, DeepSeek
expects replayed assistant messages from that turn to include
`reasoning_content` on follow-up requests. OpenClaw handles this inside the
DeepSeek plugin, so normal multi-turn tool use works with
`deepseek/deepseek-v4-flash` and `deepseek/deepseek-v4-pro`.If you switch an existing session from another OpenAI-compatible provider to a
DeepSeek V4 model, older assistant tool-call turns may not have native
DeepSeek `reasoning_content`. OpenClaw fills that missing field on replayed
assistant messages for DeepSeek V4 thinking requests so the provider can accept
the history without requiring `/new`.When thinking is disabled in OpenClaw (including the UI **None** selection),
OpenClaw sends DeepSeek `thinking: { type: "disabled" }` and strips replayed
`reasoning_content` from the outgoing history. This keeps disabled-thinking
sessions on the non-thinking DeepSeek path.Use `deepseek/deepseek-v4-flash` for the default fast path. Use
`deepseek/deepseek-v4-pro` when you want the stronger V4 model and can accept
higher cost or latency.

## [​](https://docs.openclaw.ai/providers/deepseek\#live-testing)  Live testing

The direct live model suite includes DeepSeek V4 in the modern model set. To
run only the DeepSeek V4 direct-model checks:

```
OPENCLAW_LIVE_PROVIDERS=deepseek \
OPENCLAW_LIVE_MODELS="deepseek/deepseek-v4-flash,deepseek/deepseek-v4-pro" \
pnpm test:live src/agents/models.profiles.live.test.ts
```

That live check verifies both V4 models can complete and that thinking/tool
follow-up turns preserve the replay payload DeepSeek requires.

## [​](https://docs.openclaw.ai/providers/deepseek\#config-example)  Config example

```
{
  env: { DEEPSEEK_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "deepseek/deepseek-v4-flash" },
    },
  },
}
```

## [​](https://docs.openclaw.ai/providers/deepseek\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config reference for agents, models, and providers.](https://docs.openclaw.ai/gateway/configuration-reference)

[Deepinfra](https://docs.openclaw.ai/providers/deepinfra) [ElevenLabs](https://docs.openclaw.ai/providers/elevenlabs)

Ctrl+I