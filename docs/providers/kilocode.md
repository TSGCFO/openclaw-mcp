---
source_url: https://docs.openclaw.ai/providers/kilocode
title: "Kilocode - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/kilocode#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

Kilocode

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Kilo Gateway](https://docs.openclaw.ai/providers/kilocode#kilo-gateway)
- [Getting started](https://docs.openclaw.ai/providers/kilocode#getting-started)
- [Default model](https://docs.openclaw.ai/providers/kilocode#default-model)
- [Built-in catalog](https://docs.openclaw.ai/providers/kilocode#built-in-catalog)
- [Config example](https://docs.openclaw.ai/providers/kilocode#config-example)
- [Related](https://docs.openclaw.ai/providers/kilocode#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/providers/kilocode\#kilo-gateway)  Kilo Gateway

Kilo Gateway provides a **unified API** that routes requests to many models behind a single
endpoint and API key. It is OpenAI-compatible, so most OpenAI SDKs work by switching the base URL.

| Property | Value |
| --- | --- |
| Provider | `kilocode` |
| Auth | `KILOCODE_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://api.kilo.ai/api/gateway/` |

## [​](https://docs.openclaw.ai/providers/kilocode\#getting-started)  Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/kilocode#)

Create an account

Go to [app.kilo.ai](https://app.kilo.ai/), sign in or create an account, then navigate to API Keys and generate a new key.

2

[Navigate to header](https://docs.openclaw.ai/providers/kilocode#)

Run onboarding

```
openclaw onboard --auth-choice kilocode-api-key
```

Or set the environment variable directly:

```
export KILOCODE_API_KEY="<your-kilocode-api-key>" # pragma: allowlist secret
```

3

[Navigate to header](https://docs.openclaw.ai/providers/kilocode#)

Verify the model is available

```
openclaw models list --provider kilocode
```

## [​](https://docs.openclaw.ai/providers/kilocode\#default-model)  Default model

The default model is `kilocode/kilo/auto`, a provider-owned smart-routing
model managed by Kilo Gateway.

OpenClaw treats `kilocode/kilo/auto` as the stable default ref, but does not
publish a source-backed task-to-upstream-model mapping for that route. Exact
upstream routing behind `kilocode/kilo/auto` is owned by Kilo Gateway, not
hard-coded in OpenClaw.

## [​](https://docs.openclaw.ai/providers/kilocode\#built-in-catalog)  Built-in catalog

OpenClaw dynamically discovers available models from the Kilo Gateway at startup. Use
`/models kilocode` to see the full list of models available with your account.Any model available on the gateway can be used with the `kilocode/` prefix:

| Model ref | Notes |
| --- | --- |
| `kilocode/kilo/auto` | Default — smart routing |
| `kilocode/anthropic/claude-sonnet-4` | Anthropic via Kilo |
| `kilocode/openai/gpt-5.5` | OpenAI via Kilo |
| `kilocode/google/gemini-3-pro-preview` | Google via Kilo |
| …and many more | Use `/models kilocode` to list all |

At startup, OpenClaw queries `GET https://api.kilo.ai/api/gateway/models` and merges
discovered models ahead of the static fallback catalog. The bundled fallback always
includes `kilocode/kilo/auto` (`Kilo Auto`) with `input: ["text", "image"]`,
`reasoning: true`, `contextWindow: 1000000`, and `maxTokens: 128000`.

## [​](https://docs.openclaw.ai/providers/kilocode\#config-example)  Config example

```
{
  env: { KILOCODE_API_KEY: "<your-kilocode-api-key>" }, // pragma: allowlist secret
  agents: {
    defaults: {
      model: { primary: "kilocode/kilo/auto" },
    },
  },
}
```

Transport and compatibility

Kilo Gateway is documented in source as OpenRouter-compatible, so it stays on
the proxy-style OpenAI-compatible path rather than native OpenAI request shaping.

- Gemini-backed Kilo refs stay on the proxy-Gemini path, so OpenClaw keeps
Gemini thought-signature sanitation there without enabling native Gemini
replay validation or bootstrap rewrites.
- Kilo Gateway uses a Bearer token with your API key under the hood.

Stream wrapper and reasoning

Kilo’s shared stream wrapper adds the provider app header and normalizes
proxy reasoning payloads for supported concrete model refs.

`kilocode/kilo/auto` and other proxy-reasoning-unsupported hints skip reasoning
injection. If you need reasoning support, use a concrete model ref such as
`kilocode/anthropic/claude-sonnet-4`.

Troubleshooting

- If model discovery fails at startup, OpenClaw falls back to the bundled static catalog containing `kilocode/kilo/auto`.
- Confirm your API key is valid and that your Kilo account has the desired models enabled.
- When the Gateway runs as a daemon, ensure `KILOCODE_API_KEY` is available to that process (for example in `~/.openclaw/.env` or via `env.shellEnv`).

## [​](https://docs.openclaw.ai/providers/kilocode\#related)  Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full OpenClaw configuration reference.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Kilo Gateway** \\
\\
Kilo Gateway dashboard, API keys, and account management.](https://app.kilo.ai/)

[Inworld](https://docs.openclaw.ai/providers/inworld) [LiteLLM](https://docs.openclaw.ai/providers/litellm)

Ctrl+I