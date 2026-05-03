---
source_url: https://docs.openclaw.ai/providers/lmstudio
title: "LM Studio - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers/lmstudio#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Providers

LM Studio

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Quick start](https://docs.openclaw.ai/providers/lmstudio#quick-start)
- [Non-interactive onboarding](https://docs.openclaw.ai/providers/lmstudio#non-interactive-onboarding)
- [Configuration](https://docs.openclaw.ai/providers/lmstudio#configuration)
- [Streaming usage compatibility](https://docs.openclaw.ai/providers/lmstudio#streaming-usage-compatibility)
- [Thinking compatibility](https://docs.openclaw.ai/providers/lmstudio#thinking-compatibility)
- [Explicit configuration](https://docs.openclaw.ai/providers/lmstudio#explicit-configuration)
- [Troubleshooting](https://docs.openclaw.ai/providers/lmstudio#troubleshooting)
- [LM Studio not detected](https://docs.openclaw.ai/providers/lmstudio#lm-studio-not-detected)
- [Authentication errors (HTTP 401)](https://docs.openclaw.ai/providers/lmstudio#authentication-errors-http-401)
- [Just-in-time model loading](https://docs.openclaw.ai/providers/lmstudio#just-in-time-model-loading)
- [LAN or tailnet LM Studio host](https://docs.openclaw.ai/providers/lmstudio#lan-or-tailnet-lm-studio-host)
- [Related](https://docs.openclaw.ai/providers/lmstudio#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

LM Studio is a friendly yet powerful app for running open-weight models on your own hardware. It lets you run llama.cpp (GGUF) or MLX models (Apple Silicon). Comes in a GUI package or headless daemon (`llmster`). For product and setup docs, see [lmstudio.ai](https://lmstudio.ai/).

## [​](https://docs.openclaw.ai/providers/lmstudio\#quick-start)  Quick start

1. Install LM Studio (desktop) or `llmster` (headless), then start the local server:

```
curl -fsSL https://lmstudio.ai/install.sh | bash
```

2. Start the server

Make sure you either start the desktop app or run the daemon using the following command:

```
lms daemon up
```

```
lms server start --port 1234
```

If you are using the app, make sure you have JIT enabled for a smooth experience. Learn more in the [LM Studio JIT and TTL guide](https://lmstudio.ai/docs/developer/core/ttl-and-auto-evict).

3. If LM Studio authentication is enabled, set `LM_API_TOKEN`:

```
export LM_API_TOKEN="your-lm-studio-api-token"
```

If LM Studio authentication is disabled, you can leave the API key blank during interactive OpenClaw setup.For LM Studio auth setup details, see [LM Studio Authentication](https://lmstudio.ai/docs/developer/core/authentication).

4. Run onboarding and choose `LM Studio`:

```
openclaw onboard
```

5. In onboarding, use the `Default model` prompt to pick your LM Studio model.

You can also set or change it later:

```
openclaw models set lmstudio/qwen/qwen3.5-9b
```

LM Studio model keys follow a `author/model-name` format (e.g. `qwen/qwen3.5-9b`). OpenClaw
model refs prepend the provider name: `lmstudio/qwen/qwen3.5-9b`. You can find the exact key for
a model by running `curl http://localhost:1234/api/v1/models` and looking at the `key` field.

## [​](https://docs.openclaw.ai/providers/lmstudio\#non-interactive-onboarding)  Non-interactive onboarding

Use non-interactive onboarding when you want to script setup (CI, provisioning, remote bootstrap):

```
openclaw onboard \
  --non-interactive \
  --accept-risk \
  --auth-choice lmstudio
```

Or specify the base URL, model, and optional API key:

```
openclaw onboard \
  --non-interactive \
  --accept-risk \
  --auth-choice lmstudio \
  --custom-base-url http://localhost:1234/v1 \
  --lmstudio-api-key "$LM_API_TOKEN" \
  --custom-model-id qwen/qwen3.5-9b
```

`--custom-model-id` takes the model key as returned by LM Studio (e.g. `qwen/qwen3.5-9b`), without
the `lmstudio/` provider prefix.For authenticated LM Studio servers, pass `--lmstudio-api-key` or set `LM_API_TOKEN`.
For unauthenticated LM Studio servers, omit the key; OpenClaw stores a local non-secret marker.`--custom-api-key` remains supported for compatibility, but `--lmstudio-api-key` is preferred for LM Studio.This writes `models.providers.lmstudio` and sets the default model to
`lmstudio/<custom-model-id>`. When you provide an API key, setup also writes the
`lmstudio:default` auth profile.Interactive setup can prompt for an optional preferred load context length and applies it across the discovered LM Studio models it saves into config.
LM Studio plugin config trusts the configured LM Studio endpoint for model requests, including loopback, LAN, and tailnet hosts. You can opt out by setting `models.providers.lmstudio.request.allowPrivateNetwork: false`.

## [​](https://docs.openclaw.ai/providers/lmstudio\#configuration)  Configuration

### [​](https://docs.openclaw.ai/providers/lmstudio\#streaming-usage-compatibility)  Streaming usage compatibility

LM Studio is streaming-usage compatible. When it does not emit an OpenAI-shaped
`usage` object, OpenClaw recovers token counts from llama.cpp-style
`timings.prompt_n` / `timings.predicted_n` metadata instead.Same streaming usage behavior applies to these OpenAI-compatible local backends:

- vLLM
- SGLang
- llama.cpp
- LocalAI
- Jan
- TabbyAPI
- text-generation-webui

### [​](https://docs.openclaw.ai/providers/lmstudio\#thinking-compatibility)  Thinking compatibility

When LM Studio’s `/api/v1/models` discovery reports model-specific reasoning
options, OpenClaw exposes the matching OpenAI-compatible `reasoning_effort`
values in model compat metadata. Current LM Studio builds can advertise binary
UI options such as `allowed_options: ["off", "on"]` while rejecting those values
on `/v1/chat/completions`; OpenClaw normalizes that binary discovery shape to
`none`, `minimal`, `low`, `medium`, `high`, and `xhigh` before sending requests.
Older saved LM Studio config that contains `off`/`on` reasoning maps is
normalized the same way when the catalog is loaded.

### [​](https://docs.openclaw.ai/providers/lmstudio\#explicit-configuration)  Explicit configuration

```
{
  models: {
    providers: {
      lmstudio: {
        baseUrl: "http://localhost:1234/v1",
        apiKey: "${LM_API_TOKEN}",
        api: "openai-completions",
        models: [\
          {\
            id: "qwen/qwen3-coder-next",\
            name: "Qwen 3 Coder Next",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 128000,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/providers/lmstudio\#troubleshooting)  Troubleshooting

### [​](https://docs.openclaw.ai/providers/lmstudio\#lm-studio-not-detected)  LM Studio not detected

Make sure LM Studio is running. If authentication is enabled, also set `LM_API_TOKEN`:

```
# Start via desktop app, or headless:
lms server start --port 1234
```

Verify the API is accessible:

```
curl http://localhost:1234/api/v1/models
```

### [​](https://docs.openclaw.ai/providers/lmstudio\#authentication-errors-http-401)  Authentication errors (HTTP 401)

If setup reports HTTP 401, verify your API key:

- Check that `LM_API_TOKEN` matches the key configured in LM Studio.
- For LM Studio auth setup details, see [LM Studio Authentication](https://lmstudio.ai/docs/developer/core/authentication).
- If your server does not require authentication, leave the key blank during setup.

### [​](https://docs.openclaw.ai/providers/lmstudio\#just-in-time-model-loading)  Just-in-time model loading

LM Studio supports just-in-time (JIT) model loading, where models are loaded on first request. OpenClaw preloads models through LM Studio’s native load endpoint by default, which helps when JIT is disabled. To let LM Studio’s JIT, idle TTL, and auto-evict behavior own model lifecycle, disable OpenClaw’s preload step:

```
{
  models: {
    providers: {
      lmstudio: {
        baseUrl: "http://localhost:1234/v1",
        api: "openai-completions",
        params: { preload: false },
        models: [{ id: "qwen/qwen3.5-9b" }],
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/providers/lmstudio\#lan-or-tailnet-lm-studio-host)  LAN or tailnet LM Studio host

Use the LM Studio host’s reachable address, keep `/v1`, and make sure LM Studio is bound beyond loopback on that machine:

```
{
  models: {
    providers: {
      lmstudio: {
        baseUrl: "http://gpu-box.local:1234/v1",
        apiKey: "lmstudio",
        api: "openai-completions",
        models: [{ id: "qwen/qwen3.5-9b" }],
      },
    },
  },
}
```

Unlike generic OpenAI-compatible providers, `lmstudio` automatically trusts its configured local/private endpoint for guarded model requests. Custom loopback provider IDs such as `localhost` or `127.0.0.1` are also trusted automatically; for LAN, tailnet, or private DNS custom provider IDs, set `models.providers.<id>.request.allowPrivateNetwork: true` explicitly.

## [​](https://docs.openclaw.ai/providers/lmstudio\#related)  Related

- [Model selection](https://docs.openclaw.ai/concepts/model-providers)
- [Ollama](https://docs.openclaw.ai/providers/ollama)
- [Local models](https://docs.openclaw.ai/gateway/local-models)

[LiteLLM](https://docs.openclaw.ai/providers/litellm) [MiniMax](https://docs.openclaw.ai/providers/minimax)

Ctrl+I