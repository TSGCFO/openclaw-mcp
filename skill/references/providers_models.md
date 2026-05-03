# Providers Models

_44 pages from docs.openclaw.ai_


---

## Models - OpenClaw

_Source: <https://docs.openclaw.ai/cli/models>_

# `openclaw models`

Model discovery, scanning, and configuration (default model, fallbacks, auth profiles).Related:

- Providers + models: [Models](https://docs.openclaw.ai/providers/models)
- Model selection concepts + `/models` slash command: [Models concept](https://docs.openclaw.ai/concepts/models)
- Provider auth setup: [Getting started](https://docs.openclaw.ai/start/getting-started)

## Common commands

```
openclaw models status
openclaw models list
openclaw models set <model-or-alias>
openclaw models scan
```

`openclaw models status` shows the resolved default/fallbacks plus an auth overview.
When provider usage snapshots are available, the OAuth/API-key status section includes
provider usage windows and quota snapshots.
Current usage-window providers: Anthropic, GitHub Copilot, Gemini CLI, OpenAI
Codex, MiniMax, Xiaomi, and z.ai. Usage auth comes from provider-specific hooks
when available; otherwise OpenClaw falls back to matching OAuth/API-key
credentials from auth profiles, env, or config.
In `--json` output, `auth.providers` is the env/config/store-aware provider
overview, while `auth.oauth` is auth-store profile health only.
Add `--probe` to run live auth probes against each configured provider profile.
Probes are real requests (may consume tokens and trigger rate limits).
Use `--agent <id>` to inspect a configured agent’s model/auth state. When omitted,
the command uses `OPENCLAW_AGENT_DIR`/`PI_CODING_AGENT_DIR` if set, otherwise the
configured default agent.
Probe rows can come from auth profiles, env credentials, or `models.json`.Notes:

- `models set <model-or-alias>` accepts `provider/model` or an alias.
- `models list` is read-only: it reads config, auth profiles, existing catalog
state, and provider-owned catalog rows, but it does not rewrite
`models.json`.
- The `Auth` column is provider-level and read-only. It is computed from local
auth profile metadata, env markers, configured provider keys, local-provider
markers, AWS Bedrock env/profile markers, and plugin synthetic-auth metadata;
it does not load provider runtime, read keychain secrets, call provider
APIs, or prove exact per-model execution readiness.
- `models list --all --provider <id>` can include provider-owned static catalog
rows from plugin manifests or bundled provider catalog metadata even when you
have not authenticated with that provider yet. Those rows still show as
unavailable until matching auth is configured.
- `models list` keeps the control plane responsive while provider catalog
discovery is slow. The default and configured views fall back to configured or
synthetic model rows after a short wait and let discovery finish in the
background. Use `--all` when you need the exact full discovered catalog and
are willing to wait for provider discovery.
- Broad `models list --all` merges manifest catalog rows over registry rows
without loading provider runtime supplement hooks. Provider-filtered manifest
fast paths use only providers marked `static`; providers marked `refreshable`
stay registry/cache-backed and append manifest rows as supplements, while
providers marked `runtime` stay on registry/runtime discovery.
- `models list` keeps native model metadata and runtime caps distinct. In table
output, `Ctx` shows `contextTokens/contextWindow` when an effective runtime
cap differs from the native context window; JSON rows include `contextTokens`
when a provider exposes that cap.
- `models list --provider <id>` filters by provider id, such as `moonshot` or
`openai-codex`. It does not accept display labels from interactive provider
pickers, such as `Moonshot AI`.
- Model refs are parsed by splitting on the **first**`/`. If the model ID includes `/` (OpenRouter-style), include the provider prefix (example: `openrouter/moonshotai/kimi-k2`).
- If you omit the provider, OpenClaw resolves the input as an alias first, then
as a unique configured-provider match for that exact model id, and only then
falls back to the configured default provider wit

_… [truncated; see https://docs.openclaw.ai/cli/models for full content]_


---

## Local models - OpenClaw

_Source: <https://docs.openclaw.ai/gateway/local-models>_

[OpenClaw home page](https://docs.openclaw.ai/)

Protocols and APIs

Local models

Local is doable, but OpenClaw expects large context + strong defenses against prompt injection. Small cards truncate context and leak safety. Aim high: **≥2 maxed-out Mac Studios or equivalent GPU rig (~$30k+)**. A single **24 GB** GPU works only for lighter prompts with higher latency. Use the **largest / full-size model variant you can run**; aggressively quantized or “small” checkpoints raise prompt-injection risk (see [Security](https://docs.openclaw.ai/gateway/security)).If you want the lowest-friction local setup, start with [LM Studio](https://docs.openclaw.ai/providers/lmstudio) or [Ollama](https://docs.openclaw.ai/providers/ollama) and `openclaw onboard`. This page is the opinionated guide for higher-end local stacks and custom OpenAI-compatible local servers.

**WSL2 + Ollama + NVIDIA/CUDA users:** The official Ollama Linux installer enables a systemd service with `Restart=always`. On WSL2 GPU setups, autostart can reload the last model during boot and pin host memory. If your WSL2 VM repeatedly restarts after enabling Ollama, see [WSL2 crash loop](https://docs.openclaw.ai/providers/ollama#wsl2-crash-loop-repeated-reboots).

## Recommended: LM Studio + large local model (Responses API)

Best current local stack. Load a large model in LM Studio (for example, a full-size Qwen, DeepSeek, or Llama build), enable the local server (default `http://127.0.0.1:1234`), and use Responses API to keep reasoning separate from final text.

```
{
  agents: {
    defaults: {
      model: { primary: "lmstudio/my-local-model" },
      models: {
        "anthropic/claude-opus-4-6": { alias: "Opus" },
        "lmstudio/my-local-model": { alias: "Local" },
      },
    },
  },
  models: {
    mode: "merge",
    providers: {
      lmstudio: {
        baseUrl: "http://127.0.0.1:1234/v1",
        apiKey: "lmstudio",
        api: "openai-responses",
        models: [\
          {\
            id: "my-local-model",\
            name: "Local Model",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 196608,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
}
```

**Setup checklist**

- Install LM Studio: [https://lmstudio.ai](https://lmstudio.ai/)
- In LM Studio, download the **largest model build available** (avoid “small”/heavily quantized variants), start the server, confirm `http://127.0.0.1:1234/v1/models` lists it.
- Replace `my-local-model` with the actual model ID shown in LM Studio.
- Keep the model loaded; cold-load adds startup latency.
- Adjust `contextWindow`/`maxTokens` if your LM Studio build differs.
- For WhatsApp, stick to Responses API so only final text is sent.

Keep hosted models configured even when running local; use `models.mode: "merge"` so fallbacks stay available.

### Hybrid config: hosted primary, local fallback

```
{
  agents: {
    defaults: {
      model: {
        primary: "anthropic/claude-sonnet-4-6",
        fallbacks: ["lmstudio/my-local-model", "anthropic/claude-opus-4-6"],
      },
      models: {
        "anthropic/claude-sonnet-4-6": { alias: "Sonnet" },
        "lmstudio/my-local-model": { alias: "Local" },
        "anthropic/claude-opus-4-6": { alias: "Opus" },
      },
    },
  },
  models: {
    mode: "merge",
    providers: {
      lmstudio: {
        baseUrl: "http://127.0.0.1:1234/v1",
        apiKey: "lmstudio",
        api: "openai-responses",
        models: [\
          {\
            id: "my-local-model",\
            name: "Local Model",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 196608,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
}
```

### Local-first with hosted safety net

Swap the primary and fallback order; keep the s

_… [truncated; see https://docs.openclaw.ai/gateway/local-models for full content]_


---

## Provider directory - OpenClaw

_Source: <https://docs.openclaw.ai/providers>_

# Model Providers

OpenClaw can use many LLM providers. Pick a provider, authenticate, then set the
default model as `provider/model`.Looking for chat channel docs (WhatsApp/Telegram/Discord/Slack/Mattermost (plugin)/etc.)? See [Channels](https://docs.openclaw.ai/channels).

## Quick start

1. Authenticate with the provider (usually via `openclaw onboard`).
2. Set the default model:

```
{
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-6" } } },
}
```

## Provider docs

- [Alibaba Model Studio](https://docs.openclaw.ai/providers/alibaba)
- [Amazon Bedrock](https://docs.openclaw.ai/providers/bedrock)
- [Amazon Bedrock Mantle](https://docs.openclaw.ai/providers/bedrock-mantle)
- [Anthropic (API + Claude CLI)](https://docs.openclaw.ai/providers/anthropic)
- [Arcee AI (Trinity models)](https://docs.openclaw.ai/providers/arcee)
- [Azure Speech](https://docs.openclaw.ai/providers/azure-speech)
- [BytePlus (International)](https://docs.openclaw.ai/concepts/model-providers#byteplus-international)
- [Cerebras](https://docs.openclaw.ai/providers/cerebras)
- [Chutes](https://docs.openclaw.ai/providers/chutes)
- [Cloudflare AI Gateway](https://docs.openclaw.ai/providers/cloudflare-ai-gateway)
- [ComfyUI](https://docs.openclaw.ai/providers/comfy)
- [DeepSeek](https://docs.openclaw.ai/providers/deepseek)
- [ElevenLabs](https://docs.openclaw.ai/providers/elevenlabs)
- [fal](https://docs.openclaw.ai/providers/fal)
- [Fireworks](https://docs.openclaw.ai/providers/fireworks)
- [GitHub Copilot](https://docs.openclaw.ai/providers/github-copilot)
- [GLM models](https://docs.openclaw.ai/providers/glm)
- [Google (Gemini)](https://docs.openclaw.ai/providers/google)
- [Gradium](https://docs.openclaw.ai/providers/gradium)
- [Groq (LPU inference)](https://docs.openclaw.ai/providers/groq)
- [Hugging Face (Inference)](https://docs.openclaw.ai/providers/huggingface)
- [inferrs (local models)](https://docs.openclaw.ai/providers/inferrs)
- [Kilocode](https://docs.openclaw.ai/providers/kilocode)
- [LiteLLM (unified gateway)](https://docs.openclaw.ai/providers/litellm)
- [LM Studio (local models)](https://docs.openclaw.ai/providers/lmstudio)
- [MiniMax](https://docs.openclaw.ai/providers/minimax)
- [Mistral](https://docs.openclaw.ai/providers/mistral)
- [Moonshot AI (Kimi + Kimi Coding)](https://docs.openclaw.ai/providers/moonshot)
- [NVIDIA](https://docs.openclaw.ai/providers/nvidia)
- [Ollama (cloud + local models)](https://docs.openclaw.ai/providers/ollama)
- [OpenAI (API + Codex)](https://docs.openclaw.ai/providers/openai)
- [OpenCode](https://docs.openclaw.ai/providers/opencode)
- [OpenCode Go](https://docs.openclaw.ai/providers/opencode-go)
- [OpenRouter](https://docs.openclaw.ai/providers/openrouter)
- [Perplexity (web search)](https://docs.openclaw.ai/providers/perplexity-provider)
- [Qianfan](https://docs.openclaw.ai/providers/qianfan)
- [Qwen Cloud](https://docs.openclaw.ai/providers/qwen)
- [Runway](https://docs.openclaw.ai/providers/runway)
- [SenseAudio](https://docs.openclaw.ai/providers/senseaudio)
- [SGLang (local models)](https://docs.openclaw.ai/providers/sglang)
- [StepFun](https://docs.openclaw.ai/providers/stepfun)
- [Synthetic](https://docs.openclaw.ai/providers/synthetic)
- [Tencent Cloud (TokenHub)](https://docs.openclaw.ai/providers/tencent)
- [Together AI](https://docs.openclaw.ai/providers/together)
- [Venice (Venice AI, privacy-focused)](https://docs.openclaw.ai/providers/venice)
- [Vercel AI Gateway](https://docs.openclaw.ai/providers/vercel-ai-gateway)
- [vLLM (local models)](https://docs.openclaw.ai/providers/vllm)
- [Volcengine (Doubao)](https://docs.openclaw.ai/providers/volcengine)
- [Vydra](https://docs.openclaw.ai/providers/vydra)
- [xAI](https://docs.openclaw.ai/providers/xai)
- [Xiaomi](https://docs.openclaw.ai/providers/xiaomi)
- [Z.AI](https://docs.openclaw.ai/providers/zai)

## Shared overview pages

- [Additional bundled variants](https://docs.openclaw.ai/providers/models#additional-bundled-provider

_… [truncated; see https://docs.openclaw.ai/providers for full content]_


---

## Alibaba Model Studio - OpenClaw

_Source: <https://docs.openclaw.ai/providers/alibaba>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Alibaba Model Studio

OpenClaw ships a bundled `alibaba` video-generation provider for Wan models on
Alibaba Model Studio / DashScope.

- Provider: `alibaba`
- Preferred auth: `MODELSTUDIO_API_KEY`
- Also accepted: `DASHSCOPE_API_KEY`, `QWEN_API_KEY`
- API: DashScope / Model Studio async video generation

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/alibaba#)

Set an API key

```
openclaw onboard --auth-choice qwen-standard-api-key
```

2

[Navigate to header](https://docs.openclaw.ai/providers/alibaba#)

Set a default video model

```
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "alibaba/wan2.6-t2v",
      },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/alibaba#)

Verify the provider is available

```
openclaw models list --provider alibaba
```

Any of the accepted auth keys (`MODELSTUDIO_API_KEY`, `DASHSCOPE_API_KEY`, `QWEN_API_KEY`) will work. The `qwen-standard-api-key` onboarding choice configures the shared DashScope credential.

## Built-in Wan models

The bundled `alibaba` provider currently registers:

| Model ref | Mode |
| --- | --- |
| `alibaba/wan2.6-t2v` | Text-to-video |
| `alibaba/wan2.6-i2v` | Image-to-video |
| `alibaba/wan2.6-r2v` | Reference-to-video |
| `alibaba/wan2.6-r2v-flash` | Reference-to-video (fast) |
| `alibaba/wan2.7-r2v` | Reference-to-video |

## Current limits

| Parameter | Limit |
| --- | --- |
| Output videos | Up to **1** per request |
| Input images | Up to **1** |
| Input videos | Up to **4** |
| Duration | Up to **10 seconds** |
| Supported controls | `size`, `aspectRatio`, `resolution`, `audio`, `watermark` |
| Reference image/video | Remote `http(s)` URLs only |

Reference image/video mode currently requires **remote http(s) URLs**. Local file paths are not supported for reference inputs.

## Advanced configuration

Relationship to Qwen

The bundled `qwen` provider also uses Alibaba-hosted DashScope endpoints for
Wan video generation. Use:

- `qwen/...` when you want the canonical Qwen provider surface
- `alibaba/...` when you want the direct vendor-owned Wan video surface

See the [Qwen provider docs](https://docs.openclaw.ai/providers/qwen) for more detail.

Auth key priority

OpenClaw checks for auth keys in this order:

1. `MODELSTUDIO_API_KEY` (preferred)
2. `DASHSCOPE_API_KEY`
3. `QWEN_API_KEY`

Any of these will authenticate the `alibaba` provider.

## Related

[**Video generation** \\
\\
Shared video tool parameters and provider selection.](https://docs.openclaw.ai/tools/video-generation)

[**Qwen** \\
\\
Qwen provider setup and DashScope integration.](https://docs.openclaw.ai/providers/qwen)

[**Configuration reference** \\
\\
Agent defaults and model configuration.](https://docs.openclaw.ai/gateway/config-agents#agent-defaults)

[Model failover](https://docs.openclaw.ai/concepts/model-failover) [Amazon Bedrock](https://docs.openclaw.ai/providers/bedrock)

Ctrl+I


---

## Anthropic - OpenClaw

_Source: <https://docs.openclaw.ai/providers/anthropic>_

# choose: Anthropic API key
```

Or pass the key directly:

```
openclaw onboard --anthropic-api-key "$ANTHROPIC_API_KEY"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/anthropic#)

Verify the model is available

```
openclaw models list --provider anthropic
```

### Config example

```
{
  env: { ANTHROPIC_API_KEY: "sk-ant-..." },
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-6" } } },
}
```

**Best for:** reusing an existing Claude CLI login without a separate API key.

1

[Navigate to header](https://docs.openclaw.ai/providers/anthropic#)

Ensure Claude CLI is installed and logged in

Verify with:

```
claude --version
```

2

[Navigate to header](https://docs.openclaw.ai/providers/anthropic#)

Run onboarding

```
openclaw onboard
# choose: Claude CLI
```

OpenClaw detects and reuses the existing Claude CLI credentials.

3

[Navigate to header](https://docs.openclaw.ai/providers/anthropic#)

Verify the model is available

```
openclaw models list --provider anthropic
```

Setup and runtime details for the Claude CLI backend are in [CLI Backends](https://docs.openclaw.ai/gateway/cli-backends).

### Config example

Prefer the canonical Anthropic model ref plus a CLI runtime override:

```
{
  agents: {
    defaults: {
      model: { primary: "anthropic/claude-opus-4-7" },
      agentRuntime: { id: "claude-cli" },
    },
  },
}
```

Legacy `claude-cli/claude-opus-4-7` model refs still work for
compatibility, but new config should keep provider/model selection as
`anthropic/*` and put the execution backend in `agentRuntime.id`.

If you want the clearest billing path, use an Anthropic API key instead. OpenClaw also supports subscription-style options from [OpenAI Codex](https://docs.openclaw.ai/providers/openai), [Qwen Cloud](https://docs.openclaw.ai/providers/qwen), [MiniMax](https://docs.openclaw.ai/providers/minimax), and [Z.AI / GLM](https://docs.openclaw.ai/providers/glm).

## Thinking defaults (Claude 4.6)

Claude 4.6 models default to `adaptive` thinking in OpenClaw when no explicit thinking level is set.Override per-message with `/think:<level>` or in model params:

```
{
  agents: {
    defaults: {
      models: {
        "anthropic/claude-opus-4-6": {
          params: { thinking: "adaptive" },
        },
      },
    },
  },
}
```

Related Anthropic docs:

- [Adaptive thinking](https://platform.claude.com/docs/en/build-with-claude/adaptive-thinking)
- [Extended thinking](https://platform.claude.com/docs/en/build-with-claude/extended-thinking)

## Prompt caching

OpenClaw supports Anthropic’s prompt caching feature for API-key auth.

| Value | Cache duration | Description |
| --- | --- | --- |
| `"short"` (default) | 5 minutes | Applied automatically for API-key auth |
| `"long"` | 1 hour | Extended cache |
| `"none"` | No caching | Disable prompt caching |

```
{
  agents: {
    defaults: {
      models: {
        "anthropic/claude-opus-4-6": {
          params: { cacheRetention: "long" },
        },
      },
    },
  },
}
```

Per-agent cache overrides

Use model-level params as your baseline, then override specific agents via `agents.list[].params`:

```
{
  agents: {
    defaults: {
      model: { primary: "anthropic/claude-opus-4-6" },
      models: {
        "anthropic/claude-opus-4-6": {
          params: { cacheRetention: "long" },
        },
      },
    },
    list: [\
      { id: "research", default: true },\
      { id: "alerts", params: { cacheRetention: "none" } },\
    ],
  },
}
```

Config merge order:

1. `agents.defaults.models["provider/model"].params`
2. `agents.list[].params` (matching `id`, overrides by key)

This lets one agent keep a long-lived cache while another agent on the same model disables caching for bursty/low-reuse traffic.

Bedrock Claude notes

- Anthropic Claude models on Bedrock (`amazon-bedrock/*anthropic.claude*`) accept `cacheRetention` pass-through when configured.
- Non-Anthropic Bedrock models are forced to `cacheRetention: "none"` at run

_… [truncated; see https://docs.openclaw.ai/providers/anthropic for full content]_


---

## Amazon Bedrock - OpenClaw

_Source: <https://docs.openclaw.ai/providers/bedrock>_

# Optional:
export AWS_SESSION_TOKEN="..."
export AWS_PROFILE="your-profile"
# Optional (Bedrock API key/bearer token):
export AWS_BEARER_TOKEN_BEDROCK="..."
```

2

[Navigate to header](https://docs.openclaw.ai/providers/bedrock#)

Add a Bedrock provider and model to your config

No `apiKey` is required. Configure the provider with `auth: "aws-sdk"`:

```
{
  models: {
    providers: {
      "amazon-bedrock": {
        baseUrl: "https://bedrock-runtime.us-east-1.amazonaws.com",
        api: "bedrock-converse-stream",
        auth: "aws-sdk",
        models: [\
          {\
            id: "us.anthropic.claude-opus-4-6-v1:0",\
            name: "Claude Opus 4.6 (Bedrock)",\
            reasoning: true,\
            input: ["text", "image"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 200000,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
  agents: {
    defaults: {
      model: { primary: "amazon-bedrock/us.anthropic.claude-opus-4-6-v1:0" },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/bedrock#)

Verify models are available

```
openclaw models list
```

With env-marker auth (`AWS_ACCESS_KEY_ID`, `AWS_PROFILE`, or `AWS_BEARER_TOKEN_BEDROCK`), OpenClaw auto-enables the implicit Bedrock provider for model discovery without extra config.

**Best for:** EC2 instances with an IAM role attached, using the instance metadata service for authentication.

1

[Navigate to header](https://docs.openclaw.ai/providers/bedrock#)

Enable discovery explicitly

When using IMDS, OpenClaw cannot detect AWS auth from env markers alone, so you must opt in:

```
openclaw config set plugins.entries.amazon-bedrock.config.discovery.enabled true
openclaw config set plugins.entries.amazon-bedrock.config.discovery.region us-east-1
```

2

[Navigate to header](https://docs.openclaw.ai/providers/bedrock#)

Optionally add an env marker for auto mode

If you also want the env-marker auto-detection path to work (for example, for `openclaw status` surfaces):

```
export AWS_PROFILE=default
export AWS_REGION=us-east-1
```

You do **not** need a fake API key.

3

[Navigate to header](https://docs.openclaw.ai/providers/bedrock#)

Verify models are discovered

```
openclaw models list
```

The IAM role attached to your EC2 instance must have the following permissions:

- `bedrock:InvokeModel`
- `bedrock:InvokeModelWithResponseStream`
- `bedrock:ListFoundationModels` (for automatic discovery)
- `bedrock:ListInferenceProfiles` (for inference profile discovery)

Or attach the managed policy `AmazonBedrockFullAccess`.

You only need `AWS_PROFILE=default` if you specifically want an env marker for auto mode or status surfaces. The actual Bedrock runtime auth path uses the AWS SDK default chain, so IMDS instance-role auth works even without env markers.

## Automatic model discovery

OpenClaw can automatically discover Bedrock models that support **streaming**
and **text output**. Discovery uses `bedrock:ListFoundationModels` and
`bedrock:ListInferenceProfiles`, and results are cached (default: 1 hour).How the implicit provider is enabled:

- If `plugins.entries.amazon-bedrock.config.discovery.enabled` is `true`,
OpenClaw will try discovery even when no AWS env marker is present.
- If `plugins.entries.amazon-bedrock.config.discovery.enabled` is unset,
OpenClaw only auto-adds the
implicit Bedrock provider when it sees one of these AWS auth markers:
`AWS_BEARER_TOKEN_BEDROCK`, `AWS_ACCESS_KEY_ID` +
`AWS_SECRET_ACCESS_KEY`, or `AWS_PROFILE`.
- The actual Bedrock runtime auth path still uses the AWS SDK default chain, so
shared config, SSO, and IMDS instance-role auth can work even when discovery
needed `enabled: true` to opt in.

For explicit `models.providers["amazon-bedrock"]` entries, OpenClaw can still resolve Bedrock env-marker auth early from AWS env markers such as `AWS_BEARER_TOKEN_BEDROCK` without forcing full runtime auth loading. The actual model

_… [truncated; see https://docs.openclaw.ai/providers/bedrock for full content]_


---

## Cerebras - OpenClaw

_Source: <https://docs.openclaw.ai/providers/cerebras>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Cerebras

[Cerebras](https://www.cerebras.ai/) provides high-speed OpenAI-compatible inference.

| Property | Value |
| --- | --- |
| Provider | `cerebras` |
| Auth | `CEREBRAS_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://api.cerebras.ai/v1` |

## Getting Started

1

[Navigate to header](https://docs.openclaw.ai/providers/cerebras#)

Get an API key

Create an API key in the [Cerebras Cloud Console](https://cloud.cerebras.ai/).

2

[Navigate to header](https://docs.openclaw.ai/providers/cerebras#)

Run onboarding

```
openclaw onboard --auth-choice cerebras-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/cerebras#)

Verify models are available

```
openclaw models list --provider cerebras
```

### Non-Interactive Setup

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice cerebras-api-key \
  --cerebras-api-key "$CEREBRAS_API_KEY"
```

## Built-In Catalog

OpenClaw ships a static Cerebras catalog for the public OpenAI-compatible endpoint:

| Model ref | Name | Notes |
| --- | --- | --- |
| `cerebras/zai-glm-4.7` | Z.ai GLM 4.7 | Default model; preview reasoning model |
| `cerebras/gpt-oss-120b` | GPT OSS 120B | Production reasoning model |
| `cerebras/qwen-3-235b-a22b-instruct-2507` | Qwen 3 235B Instruct | Preview non-reasoning model |
| `cerebras/llama3.1-8b` | Llama 3.1 8B | Production speed-focused model |

Cerebras marks `zai-glm-4.7` and `qwen-3-235b-a22b-instruct-2507` as preview models, and `llama3.1-8b` / `qwen-3-235b-a22b-instruct-2507` are documented for deprecation on May 27, 2026. Check Cerebras’ supported-models page before relying on them for production.

## Manual Config

The bundled plugin usually means you only need the API key. Use explicit
`models.providers.cerebras` config when you want to override model metadata:

```
{
  env: { CEREBRAS_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "cerebras/zai-glm-4.7" },
    },
  },
  models: {
    mode: "merge",
    providers: {
      cerebras: {
        baseUrl: "https://api.cerebras.ai/v1",
        apiKey: "${CEREBRAS_API_KEY}",
        api: "openai-completions",
        models: [\
          { id: "zai-glm-4.7", name: "Z.ai GLM 4.7" },\
          { id: "gpt-oss-120b", name: "GPT OSS 120B" },\
        ],
      },
    },
  },
}
```

If the Gateway runs as a daemon (launchd/systemd), make sure `CEREBRAS_API_KEY`
is available to that process, for example in `~/.openclaw/.env` or through
`env.shellEnv`.

[Azure Speech](https://docs.openclaw.ai/providers/azure-speech) [Chutes](https://docs.openclaw.ai/providers/chutes)

Ctrl+I


---

## Claude Max API proxy - OpenClaw

_Source: <https://docs.openclaw.ai/providers/claude-max-api-proxy>_

# Verify Claude CLI is authenticated
claude --version
```

2

[Navigate to header](https://docs.openclaw.ai/providers/claude-max-api-proxy#)

Start the server

```
claude-max-api
# Server runs at http://localhost:3456
```

3

[Navigate to header](https://docs.openclaw.ai/providers/claude-max-api-proxy#)

Test the proxy

```
# Health check
curl http://localhost:3456/health

# List models
curl http://localhost:3456/v1/models

# Chat completion
curl http://localhost:3456/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "claude-opus-4",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```

4

[Navigate to header](https://docs.openclaw.ai/providers/claude-max-api-proxy#)

Configure OpenClaw

Point OpenClaw at the proxy as a custom OpenAI-compatible endpoint:

```
{
  env: {
    OPENAI_API_KEY: "not-needed",
    OPENAI_BASE_URL: "http://localhost:3456/v1",
  },
  agents: {
    defaults: {
      model: { primary: "openai/claude-opus-4" },
    },
  },
}
```

## Built-in catalog

| Model ID | Maps To |
| --- | --- |
| `claude-opus-4` | Claude Opus 4 |
| `claude-sonnet-4` | Claude Sonnet 4 |
| `claude-haiku-4` | Claude Haiku 4 |

## Advanced configuration

Proxy-style OpenAI-compatible notes

This path uses the same proxy-style OpenAI-compatible route as other custom
`/v1` backends:

- Native OpenAI-only request shaping does not apply
- No `service_tier`, no Responses `store`, no prompt-cache hints, and no
OpenAI reasoning-compat payload shaping
- Hidden OpenClaw attribution headers (`originator`, `version`, `User-Agent`)
are not injected on the proxy URL

Auto-start on macOS with LaunchAgent

Create a LaunchAgent to run the proxy automatically:

```
cat > ~/Library/LaunchAgents/com.claude-max-api.plist << 'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>com.claude-max-api</string>
  <key>RunAtLoad</key>
  <true/>
  <key>KeepAlive</key>
  <true/>
  <key>ProgramArguments</key>
  <array>
    <string>/usr/local/bin/node</string>
    <string>/usr/local/lib/node_modules/claude-max-api-proxy/dist/server/standalone.js</string>
  </array>
  <key>EnvironmentVariables</key>
  <dict>
    <key>PATH</key>
    <string>/usr/local/bin:/opt/homebrew/bin:~/.local/bin:/usr/bin:/bin</string>
  </dict>
</dict>
</plist>
EOF

launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.claude-max-api.plist
```

## Links

- **npm:** [https://www.npmjs.com/package/claude-max-api-proxy](https://www.npmjs.com/package/claude-max-api-proxy)
- **GitHub:** [https://github.com/atalovesyou/claude-max-api-proxy](https://github.com/atalovesyou/claude-max-api-proxy)
- **Issues:** [https://github.com/atalovesyou/claude-max-api-proxy/issues](https://github.com/atalovesyou/claude-max-api-proxy/issues)

## Notes

- This is a **community tool**, not officially supported by Anthropic or OpenClaw
- Requires an active Claude Max/Pro subscription with Claude Code CLI authenticated
- The proxy runs locally and does not send data to any third-party servers
- Streaming responses are fully supported

For native Anthropic integration with Claude CLI or API keys, see [Anthropic provider](https://docs.openclaw.ai/providers/anthropic). For OpenAI/Codex subscriptions, see [OpenAI provider](https://docs.openclaw.ai/providers/openai).

## Related

[**Anthropic provider** \\
\\
Native OpenClaw integration with Claude CLI or API keys.](https://docs.openclaw.ai/providers/anthropic)

[**OpenAI provider** \\
\\
For OpenAI/Codex subscriptions.](https://docs.openclaw.ai/providers/openai)

[**Model selection** \\
\\
Overview of all providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration** \\
\\
Full config reference.](https://docs.openclaw.ai/gateway/configuration)

[Chutes](https://docs.openclaw.ai/providers/chutes) [Cloudflare AI gateway](h

_… [truncated; see https://docs.openclaw.ai/providers/claude-max-api-proxy for full content]_


---

## Cloudflare AI gateway - OpenClaw

_Source: <https://docs.openclaw.ai/providers/cloudflare-ai-gateway>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Cloudflare AI gateway

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

## Getting started

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

## Non-interactive example

For scripted or CI setups, pass all values on the command line:

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice cloudflare-ai-gateway-api-key \
  --cloudflare-ai-gateway-account-id "your-account-id" \
  --cloudflare-ai-gateway-gateway-id "your-gateway-id" \
  --cloudflare-ai-gateway-api-key "$CLOUDFLARE_AI_GATEWAY_API_KEY"
```

## Advanced configuration

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

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Troubleshooting** \\
\\
General troubleshooting and FAQ.](https://docs.openclaw.ai/help/troubleshooting)

[Claude Max API proxy](https://docs.openclaw.ai/providers/claude-max-api-proxy) [ComfyUI](https://docs.openclaw.ai/providers/comfy)

Ctrl+I


---

## Deepgram - OpenClaw

_Source: <https://docs.openclaw.ai/providers/deepgram>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Deepgram

Deepgram is a speech-to-text API. In OpenClaw it is used for inbound
audio/voice-note transcription through `tools.media.audio` and for Voice Call
streaming STT through `plugins.entries.voice-call.config.streaming`.For batch transcription, OpenClaw uploads the complete audio file to Deepgram
and injects the transcript into the reply pipeline (`{{Transcript}}` +
`[Audio]` block). For Voice Call streaming, OpenClaw forwards live G.711
u-law frames over Deepgram’s WebSocket `listen` endpoint and emits partial or
final transcripts as Deepgram returns them.

| Detail | Value |
| --- | --- |
| Website | [deepgram.com](https://deepgram.com/) |
| Docs | [developers.deepgram.com](https://developers.deepgram.com/) |
| Auth | `DEEPGRAM_API_KEY` |
| Default model | `nova-3` |

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/deepgram#)

Set your API key

Add your Deepgram API key to the environment:

```
DEEPGRAM_API_KEY=dg_...
```

2

[Navigate to header](https://docs.openclaw.ai/providers/deepgram#)

Enable the audio provider

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "deepgram", model: "nova-3" }],
      },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/deepgram#)

Send a voice note

Send an audio message through any connected channel. OpenClaw transcribes it
via Deepgram and injects the transcript into the reply pipeline.

## Configuration options

| Option | Path | Description |
| --- | --- | --- |
| `model` | `tools.media.audio.models[].model` | Deepgram model id (default: `nova-3`) |
| `language` | `tools.media.audio.models[].language` | Language hint (optional) |
| `detect_language` | `tools.media.audio.providerOptions.deepgram.detect_language` | Enable language detection (optional) |
| `punctuate` | `tools.media.audio.providerOptions.deepgram.punctuate` | Enable punctuation (optional) |
| `smart_format` | `tools.media.audio.providerOptions.deepgram.smart_format` | Enable smart formatting (optional) |

- With language hint

- With Deepgram options

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "deepgram", model: "nova-3", language: "en" }],
      },
    },
  },
}
```

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        providerOptions: {
          deepgram: {
            detect_language: true,
            punctuate: true,
            smart_format: true,
          },
        },
        models: [{ provider: "deepgram", model: "nova-3" }],
      },
    },
  },
}
```

## Voice Call streaming STT

The bundled `deepgram` plugin also registers a realtime transcription provider
for the Voice Call plugin.

| Setting | Config path | Default |
| --- | --- | --- |
| API key | `plugins.entries.voice-call.config.streaming.providers.deepgram.apiKey` | Falls back to `DEEPGRAM_API_KEY` |
| Model | `...deepgram.model` | `nova-3` |
| Language | `...deepgram.language` | (unset) |
| Encoding | `...deepgram.encoding` | `mulaw` |
| Sample rate | `...deepgram.sampleRate` | `8000` |
| Endpointing | `...deepgram.endpointingMs` | `800` |
| Interim results | `...deepgram.interimResults` | `true` |

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          streaming: {
            enabled: true,
            provider: "deepgram",
            providers: {
              deepgram: {
                apiKey: "${DEEPGRAM_API_KEY}",
                model: "nova-3",
                endpointingMs: 800,
                language: "en-US",
              },
            },
          },
        },
      },
    },
  },
}
```

Voice Call receives telephony audio as 8 kHz G.711 u-law. The Deepgram
streaming provider defaults to `encoding: "mulaw"` and `sampleRate: 8000`, so
Twilio media frames can be forwarded directly.

## Notes

Authentication

Authentication follows the standard provider auth

_… [truncated; see https://docs.openclaw.ai/providers/deepgram for full content]_


---

## DeepSeek - OpenClaw

_Source: <https://docs.openclaw.ai/providers/deepseek>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

DeepSeek

[DeepSeek](https://www.deepseek.com/) provides powerful AI models with an OpenAI-compatible API.

| Property | Value |
| --- | --- |
| Provider | `deepseek` |
| Auth | `DEEPSEEK_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://api.deepseek.com` |

## Getting started

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

## Built-in catalog

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

## Thinking and tools

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

## Live testing

The direct live model suite includes DeepSeek V4 in the modern model set. To
run only the DeepSeek V4 direct-model checks:

```
OPENCLAW_LIVE_PROVIDERS=deepseek \
OPENCLAW_LIVE_MODELS="deepseek/deepseek-v4-flash,deepseek/deepseek-v4-pro" \
pnpm test:live src/agents/models.profiles.live.test.ts
```

That live check verifies both V4 models can complete and that thinking/tool
follow-up turns preserve the replay payload DeepSeek r

_… [truncated; see https://docs.openclaw.ai/providers/deepseek for full content]_


---

## GitHub Copilot - OpenClaw

_Source: <https://docs.openclaw.ai/providers/github-copilot>_

# Skip confirmation
openclaw models auth login-github-copilot --yes

# Login and set the default model in one step
openclaw models auth login --provider github-copilot --method device --set-default
```

## Non-interactive onboarding

If you already have a GitHub OAuth access token for Copilot, import it during
headless setup with `openclaw onboard --non-interactive`:

```
openclaw onboard --non-interactive --accept-risk \
  --auth-choice github-copilot \
  --github-copilot-token "$COPILOT_GITHUB_TOKEN" \
  --skip-channels --skip-health
```

You can also omit `--auth-choice`; passing `--github-copilot-token` infers the
GitHub Copilot provider auth choice. If the flag is omitted, onboarding falls
back to `COPILOT_GITHUB_TOKEN`, `GH_TOKEN`, then `GITHUB_TOKEN`. Use
`--secret-input-mode ref` with `COPILOT_GITHUB_TOKEN` set to store an env-backed
`tokenRef` instead of plaintext in `auth-profiles.json`.

Interactive TTY required

The device-login flow requires an interactive TTY. Run it directly in a
terminal, not in a non-interactive script or CI pipeline.

Model availability depends on your plan

Copilot model availability depends on your GitHub plan. If a model is
rejected, try another ID (for example `github-copilot/gpt-4.1`).

Transport selection

Claude model IDs use the Anthropic Messages transport automatically. GPT,
o-series, and Gemini models keep the OpenAI Responses transport. OpenClaw
selects the correct transport based on the model ref.

Request compatibility

OpenClaw sends Copilot IDE-style request headers on Copilot transports,
including built-in compaction, tool-result, and image follow-up turns. It
does not enable provider-level Responses continuation for Copilot unless
that behavior has been verified against Copilot’s API.

Environment variable resolution order

OpenClaw resolves Copilot auth from environment variables in the following
priority order:

| Priority | Variable | Notes |
| --- | --- | --- |
| 1 | `COPILOT_GITHUB_TOKEN` | Highest priority, Copilot-specific |
| 2 | `GH_TOKEN` | GitHub CLI token (fallback) |
| 3 | `GITHUB_TOKEN` | Standard GitHub token (lowest) |

When multiple variables are set, OpenClaw uses the highest-priority one.
The device-login flow (`openclaw models auth login-github-copilot`) stores
its token in the auth profile store and takes precedence over all environment
variables.

Token storage

The login stores a GitHub token in the auth profile store and exchanges it
for a Copilot API token when OpenClaw runs. You do not need to manage the
token manually.

The device-login command requires an interactive TTY. Use non-interactive
onboarding when you need headless setup.

## Memory search embeddings

GitHub Copilot can also serve as an embedding provider for
[memory search](https://docs.openclaw.ai/concepts/memory-search). If you have a Copilot subscription and
have logged in, OpenClaw can use it for embeddings without a separate API key.

### Auto-detection

When `memorySearch.provider` is `"auto"` (the default), GitHub Copilot is tried
at priority 15 — after local embeddings but before OpenAI and other paid
providers. If a GitHub token is available, OpenClaw discovers available
embedding models from the Copilot API and picks the best one automatically.

### Explicit config

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "github-copilot",
        // Optional: override the auto-discovered model
        model: "text-embedding-3-small",
      },
    },
  },
}
```

### How it works

1. OpenClaw resolves your GitHub token (from env vars or auth profile).
2. Exchanges it for a short-lived Copilot API token.
3. Queries the Copilot `/models` endpoint to discover available embedding models.
4. Picks the best model (prefers `text-embedding-3-small`).
5. Sends embedding requests to the Copilot `/embeddings` endpoint.

Model availability depends on your GitHub plan. If no embedding models are
available, OpenClaw skips Copilot and tries the next provider.

## Related

[**

_… [truncated; see https://docs.openclaw.ai/providers/github-copilot for full content]_


---

## GLM (Zhipu) - OpenClaw

_Source: <https://docs.openclaw.ai/providers/glm>_

# GLM models

GLM is a **model family** (not a company) available through the Z.AI platform. In OpenClaw, GLM
models are accessed via the `zai` provider and model IDs like `zai/glm-5`.

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/glm#)

Choose an auth route and run onboarding

Pick the onboarding choice that matches your Z.AI plan and region:

| Auth choice | Best for |
| --- | --- |
| `zai-api-key` | Generic API-key setup with endpoint auto-detection |
| `zai-coding-global` | Coding Plan users (global) |
| `zai-coding-cn` | Coding Plan users (China region) |
| `zai-global` | General API (global) |
| `zai-cn` | General API (China region) |

```
# Example: generic auto-detect
openclaw onboard --auth-choice zai-api-key

# Example: Coding Plan global
openclaw onboard --auth-choice zai-coding-global
```

2

[Navigate to header](https://docs.openclaw.ai/providers/glm#)

Set GLM as the default model

```
openclaw config set agents.defaults.model.primary "zai/glm-5.1"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/glm#)

Verify models are available

```
openclaw models list --provider zai
```

## Config example

```
{
  env: { ZAI_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "zai/glm-5.1" } } },
}
```

`zai-api-key` lets OpenClaw detect the matching Z.AI endpoint from the key and
apply the correct base URL automatically. Use the explicit regional choices when
you want to force a specific Coding Plan or general API surface.

## Built-in catalog

OpenClaw currently seeds the bundled `zai` provider with these GLM refs:

| Model | Model |
| --- | --- |
| `glm-5.1` | `glm-4.7` |
| `glm-5` | `glm-4.7-flash` |
| `glm-5-turbo` | `glm-4.7-flashx` |
| `glm-5v-turbo` | `glm-4.6` |
| `glm-4.5` | `glm-4.6v` |
| `glm-4.5-air` |  |
| `glm-4.5-flash` |  |
| `glm-4.5v` |  |

The default bundled model ref is `zai/glm-5.1`. GLM versions and availability
can change; check Z.AI’s docs for the latest.

## Advanced configuration

Endpoint auto-detection

When you use the `zai-api-key` auth choice, OpenClaw inspects the key format
to determine the correct Z.AI base URL. Explicit regional choices
(`zai-coding-global`, `zai-coding-cn`, `zai-global`, `zai-cn`) override
auto-detection and pin the endpoint directly.

Provider details

GLM models are served by the `zai` runtime provider. For full provider
configuration, regional endpoints, and additional capabilities, see
[Z.AI provider docs](https://docs.openclaw.ai/providers/zai).

## Related

[**Z.AI provider** \\
\\
Full Z.AI provider configuration and regional endpoints.](https://docs.openclaw.ai/providers/zai)

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[GitHub Copilot](https://docs.openclaw.ai/providers/github-copilot) [Google (Gemini)](https://docs.openclaw.ai/providers/google)

Ctrl+I


---

## Google (Gemini) - OpenClaw

_Source: <https://docs.openclaw.ai/providers/google>_

# Homebrew
brew install gemini-cli

# or npm
npm install -g @google/gemini-cli
```

OpenClaw supports both Homebrew installs and global npm installs, including
common Windows/npm layouts.

2

[Navigate to header](https://docs.openclaw.ai/providers/google#)

Log in via OAuth

```
openclaw models auth login --provider google-gemini-cli --set-default
```

3

[Navigate to header](https://docs.openclaw.ai/providers/google#)

Verify the model is available

```
openclaw models list --provider google
```

- Default model: `google/gemini-3.1-pro-preview`
- Runtime: `google-gemini-cli`
- Alias: `gemini-cli`

Gemini 3.1 Pro’s Gemini API model id is `gemini-3.1-pro-preview`. OpenClaw accepts the shorter `google/gemini-3.1-pro` as a convenience alias and normalizes it before provider calls.**Environment variables:**

- `OPENCLAW_GEMINI_OAUTH_CLIENT_ID`
- `OPENCLAW_GEMINI_OAUTH_CLIENT_SECRET`

(Or the `GEMINI_CLI_*` variants.)

If Gemini CLI OAuth requests fail after login, set `GOOGLE_CLOUD_PROJECT` or
`GOOGLE_CLOUD_PROJECT_ID` on the gateway host and retry.

If login fails before the browser flow starts, make sure the local `gemini`
command is installed and on `PATH`.

`google-gemini-cli/*` model refs are legacy compatibility aliases. New
configs should use `google/*` model refs plus the `google-gemini-cli`
runtime when they want local Gemini CLI execution.

## Capabilities

| Capability | Supported |
| --- | --- |
| Chat completions | Yes |
| Image generation | Yes |
| Music generation | Yes |
| Text-to-speech | Yes |
| Realtime voice | Yes (Google Live API) |
| Image understanding | Yes |
| Audio transcription | Yes |
| Video understanding | Yes |
| Web search (Grounding) | Yes |
| Thinking/reasoning | Yes (Gemini 2.5+ / Gemini 3+) |
| Gemma 4 models | Yes |

## Web search

The bundled `gemini` web-search provider uses Gemini Google Search grounding.
Configure a dedicated search key under `plugins.entries.google.config.webSearch`,
or let it reuse `models.providers.google.apiKey` after `GEMINI_API_KEY`:

```
{
  plugins: {
    entries: {
      google: {
        config: {
          webSearch: {
            apiKey: "AIza...", // optional if GEMINI_API_KEY or models.providers.google.apiKey is set
            baseUrl: "https://generativelanguage.googleapis.com/v1beta", // falls back to models.providers.google.baseUrl
            model: "gemini-2.5-flash",
          },
        },
      },
    },
  },
}
```

Credential precedence is dedicated `webSearch.apiKey`, then `GEMINI_API_KEY`,
then `models.providers.google.apiKey`. `webSearch.baseUrl` is optional and
exists for operator proxies or compatible Gemini API endpoints; when omitted,
Gemini web search reuses `models.providers.google.baseUrl`. See
[Gemini search](https://docs.openclaw.ai/tools/gemini-search) for the provider-specific tool behavior.

Gemini 3 models use `thinkingLevel` rather than `thinkingBudget`. OpenClaw maps
Gemini 3, Gemini 3.1, and `gemini-*-latest` alias reasoning controls to
`thinkingLevel` so default/low-latency runs do not send disabled
`thinkingBudget` values.`/think adaptive` keeps Google’s dynamic thinking semantics instead of choosing
a fixed OpenClaw level. Gemini 3 and Gemini 3.1 omit a fixed `thinkingLevel` so
Google can choose the level; Gemini 2.5 sends Google’s dynamic sentinel
`thinkingBudget: -1`.Gemma 4 models (for example `gemma-4-26b-a4b-it`) support thinking mode. OpenClaw
rewrites `thinkingBudget` to a supported Google `thinkingLevel` for Gemma 4.
Setting thinking to `off` preserves thinking disabled instead of mapping to
`MINIMAL`.

## Image generation

The bundled `google` image-generation provider defaults to
`google/gemini-3.1-flash-image-preview`.

- Also supports `google/gemini-3-pro-image-preview`
- Generate: up to 4 images per request
- Edit mode: enabled, up to 5 input images
- Geometry controls: `size`, `aspectRatio`, and `resolution`

To use Google as the default image provider:

```
{
  agents: {
    defaults: {
      imageGenerationModel:

_… [truncated; see https://docs.openclaw.ai/providers/google for full content]_


---

## Groq - OpenClaw

_Source: <https://docs.openclaw.ai/providers/groq>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Groq

[Groq](https://groq.com/) provides ultra-fast inference on open-source models
(Llama, Gemma, Mistral, and more) using custom LPU hardware. OpenClaw connects
to Groq through its OpenAI-compatible API.

| Property | Value |
| --- | --- |
| Provider | `groq` |
| Auth | `GROQ_API_KEY` |
| API | OpenAI-compatible |

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/groq#)

Get an API key

Create an API key at [console.groq.com/keys](https://console.groq.com/keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/groq#)

Set the API key

```
export GROQ_API_KEY="gsk_..."
```

3

[Navigate to header](https://docs.openclaw.ai/providers/groq#)

Set a default model

```
{
  agents: {
    defaults: {
      model: { primary: "groq/llama-3.3-70b-versatile" },
    },
  },
}
```

### Config file example

```
{
  env: { GROQ_API_KEY: "gsk_..." },
  agents: {
    defaults: {
      model: { primary: "groq/llama-3.3-70b-versatile" },
    },
  },
}
```

## Built-in catalog

OpenClaw ships a manifest-backed Groq catalog for fast provider-filtered model
listing. Run `openclaw models list --all --provider groq` to see the bundled
rows, or check
[console.groq.com/docs/models](https://console.groq.com/docs/models).

| Model | Notes |
| --- | --- |
| **Llama 3.3 70B Versatile** | General-purpose, large context |
| **Llama 3.1 8B Instant** | Fast, lightweight |
| **Gemma 2 9B** | Compact, efficient |
| **Mixtral 8x7B** | MoE architecture, strong reasoning |

Use `openclaw models list --all --provider groq` for the manifest-backed Groq
rows known to this OpenClaw version.

## Reasoning models

OpenClaw maps its shared `/think` levels to Groq’s model-specific
`reasoning_effort` values. For `qwen/qwen3-32b`, disabled thinking sends
`none` and enabled thinking sends `default`. For Groq GPT-OSS reasoning models,
OpenClaw sends `low`, `medium`, or `high`; disabled thinking omits
`reasoning_effort` because those models do not support a disabled value.

## Audio transcription

Groq also provides fast Whisper-based audio transcription. When configured as a
media-understanding provider, OpenClaw uses Groq’s `whisper-large-v3-turbo`
model to transcribe voice messages through the shared `tools.media.audio`
surface.

```
{
  tools: {
    media: {
      audio: {
        models: [{ provider: "groq" }],
      },
    },
  },
}
```

Audio transcription details

| Property | Value |
| --- | --- |
| Shared config path | `tools.media.audio` |
| Default base URL | `https://api.groq.com/openai/v1` |
| Default model | `whisper-large-v3-turbo` |
| API endpoint | OpenAI-compatible `/audio/transcriptions` |

Environment note

If the Gateway runs as a daemon (launchd/systemd), make sure `GROQ_API_KEY` is
available to that process (for example, in `~/.openclaw/.env` or via
`env.shellEnv`).

Keys set only in your interactive shell are not visible to daemon-managed
gateway processes. Use `~/.openclaw/.env` or `env.shellEnv` config for
persistent availability.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config schema including provider and audio settings.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Groq Console** \\
\\
Groq dashboard, API docs, and pricing.](https://console.groq.com/)

[**Groq model list** \\
\\
Official Groq model catalog.](https://console.groq.com/docs/models)

[Gradium](https://docs.openclaw.ai/providers/gradium) [Hugging Face (inference)](https://docs.openclaw.ai/providers/huggingface)

Ctrl+I


---

## Hugging Face (inference) - OpenClaw

_Source: <https://docs.openclaw.ai/providers/huggingface>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Hugging Face (inference)

[Hugging Face Inference Providers](https://huggingface.co/docs/inference-providers) offer OpenAI-compatible chat completions through a single router API. You get access to many models (DeepSeek, Llama, and more) with one token. OpenClaw uses the **OpenAI-compatible endpoint** (chat completions only); for text-to-image, embeddings, or speech use the [HF inference clients](https://huggingface.co/docs/api-inference/quicktour) directly.

- Provider: `huggingface`
- Auth: `HUGGINGFACE_HUB_TOKEN` or `HF_TOKEN` (fine-grained token with **Make calls to Inference Providers**)
- API: OpenAI-compatible (`https://router.huggingface.co/v1`)
- Billing: Single HF token; [pricing](https://huggingface.co/docs/inference-providers/pricing) follows provider rates with a free tier.

## Getting started

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

### Non-interactive setup

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice huggingface-api-key \
  --huggingface-api-key "$HF_TOKEN"
```

This will set `huggingface/deepseek-ai/DeepSeek-R1` as the default model.

## Model IDs

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

## Advanced configuration

Model discovery and onboarding dropdown

OpenClaw discovers models by calling the **Inference endpoint directly**:

```
GET https://router.huggingface.co/v1/models
```

(Optional: send `Authorization: Bearer $HUGGINGFACE_HUB_TOKEN` or `$HF_TOKEN` for the full list; some endpoints return a subset without auth.) The response is OpenAI-style `{ "object": "list", "data": [ { "id": "Qwen/Qwen3-8B", "owned_by": "Qwen", ... }, ... ] }`.When you configure a Hugging Face API key (via onboarding, `HUGGINGFACE_HUB_TOKEN`, or `HF_TOKEN`), OpenClaw uses

_… [truncated; see https://docs.openclaw.ai/providers/huggingface for full content]_


---

## Kilocode - OpenClaw

_Source: <https://docs.openclaw.ai/providers/kilocode>_

# Kilo Gateway

Kilo Gateway provides a **unified API** that routes requests to many models behind a single
endpoint and API key. It is OpenAI-compatible, so most OpenAI SDKs work by switching the base URL.

| Property | Value |
| --- | --- |
| Provider | `kilocode` |
| Auth | `KILOCODE_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://api.kilo.ai/api/gateway/` |

## Getting started

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

## Default model

The default model is `kilocode/kilo/auto`, a provider-owned smart-routing
model managed by Kilo Gateway.

OpenClaw treats `kilocode/kilo/auto` as the stable default ref, but does not
publish a source-backed task-to-upstream-model mapping for that route. Exact
upstream routing behind `kilocode/kilo/auto` is owned by Kilo Gateway, not
hard-coded in OpenClaw.

## Built-in catalog

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

## Config example

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

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full OpenClaw configuration reference.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Kilo Gateway**

_… [truncated; see https://docs.openclaw.ai/providers/kilocode for full content]_


---

## LiteLLM - OpenClaw

_Source: <https://docs.openclaw.ai/providers/litellm>_

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

## Related

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


---

## LM Studio - OpenClaw

_Source: <https://docs.openclaw.ai/providers/lmstudio>_

# Start via desktop app, or headless:
lms server start --port 1234
```

Verify the API is accessible:

```
curl http://localhost:1234/api/v1/models
```

### Authentication errors (HTTP 401)

If setup reports HTTP 401, verify your API key:

- Check that `LM_API_TOKEN` matches the key configured in LM Studio.
- For LM Studio auth setup details, see [LM Studio Authentication](https://lmstudio.ai/docs/developer/core/authentication).
- If your server does not require authentication, leave the key blank during setup.

### Just-in-time model loading

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

### LAN or tailnet LM Studio host

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

## Related

- [Model selection](https://docs.openclaw.ai/concepts/model-providers)
- [Ollama](https://docs.openclaw.ai/providers/ollama)
- [Local models](https://docs.openclaw.ai/gateway/local-models)

[LiteLLM](https://docs.openclaw.ai/providers/litellm) [MiniMax](https://docs.openclaw.ai/providers/minimax)

Ctrl+I


---

## https://docs.openclaw.ai/providers/lmstudio.md

_Source: <https://docs.openclaw.ai/providers/lmstudio.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# LM Studio

LM Studio is a friendly yet powerful app for running open-weight models on your own hardware. It lets you run llama.cpp (GGUF) or MLX models (Apple Silicon). Comes in a GUI package or headless daemon (\`llmster\`). For product and setup docs, see \[lmstudio.ai\](https://lmstudio.ai/).

\## Quick start

1\. Install LM Studio (desktop) or \`llmster\` (headless), then start the local server:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
curl -fsSL https://lmstudio.ai/install.sh \| bash
\`\`\`

2\. Start the server

Make sure you either start the desktop app or run the daemon using the following command:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
lms daemon up
\`\`\`

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
lms server start --port 1234
\`\`\`

If you are using the app, make sure you have JIT enabled for a smooth experience. Learn more in the \[LM Studio JIT and TTL guide\](https://lmstudio.ai/docs/developer/core/ttl-and-auto-evict).

3\. If LM Studio authentication is enabled, set \`LM\_API\_TOKEN\`:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
export LM\_API\_TOKEN="your-lm-studio-api-token"
\`\`\`

If LM Studio authentication is disabled, you can leave the API key blank during interactive OpenClaw setup.

For LM Studio auth setup details, see \[LM Studio Authentication\](https://lmstudio.ai/docs/developer/core/authentication).

4\. Run onboarding and choose \`LM Studio\`:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw onboard
\`\`\`

5\. In onboarding, use the \`Default model\` prompt to pick your LM Studio model.

You can also set or change it later:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw models set lmstudio/qwen/qwen3.5-9b
\`\`\`

LM Studio model keys follow a \`author/model-name\` format (e.g. \`qwen/qwen3.5-9b\`). OpenClaw
model refs prepend the provider name: \`lmstudio/qwen/qwen3.5-9b\`. You can find the exact key for
a model by running \`curl http://localhost:1234/api/v1/models\` and looking at the \`key\` field.

\## Non-interactive onboarding

Use non-interactive onboarding when you want to script setup (CI, provisioning, remote bootstrap):

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw onboard \
 --non-interactive \
 --accept-risk \
 --auth-choice lmstudio
\`\`\`

Or specify the base URL, model, and optional API key:

\`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
openclaw onboard \
 --non-interactive \
 --accept-risk \
 --auth-choice lmstudio \
 --custom-base-url http://localhost:1234/v1 \
 --lmstudio-api-key "$LM\_API\_TOKEN" \
 --custom-model-id qwen/qwen3.5-9b
\`\`\`

\`--custom-model-id\` takes the model key as returned by LM Studio (e.g. \`qwen/qwen3.5-9b\`), without
the \`lmstudio/\` provider prefix.

For authenticated LM Studio servers, pass \`--lmstudio-api-key\` or set \`LM\_API\_TOKEN\`.
For unauthenticated LM Studio servers, omit the key; OpenClaw stores a local non-secret marker.

\`--custom-api-key\` remains supported for compatibility, but \`--lmstudio-api-key\` is preferred for LM Studio.

This writes \`models.providers.lmstudio\` and sets the default model to
\`lmstudio/\`. When you provide an API key, setup also writes the
\`lmstudio:default\` auth profile.

Interactive setup can prompt for an optional preferred load context length and applies it across the discovered LM Studio models it saves into config.
LM Studio plugin config trusts the configured LM Studio endpoint for model requests, including loopback, LAN, and tailnet hosts. You can opt out by setting \`models.providers.lmstudio.request.allowPrivateNetwork: false\`.

\## Configuration

\### Streaming usage compatibility

LM Studio is streaming-usage compatible. Wh

_… [truncated; see https://docs.openclaw.ai/providers/lmstudio.md for full content]_


---

## MiniMax - OpenClaw

_Source: <https://docs.openclaw.ai/providers/minimax>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

MiniMax

OpenClaw’s MiniMax provider defaults to **MiniMax M2.7**.MiniMax also provides:

- Bundled speech synthesis via T2A v2
- Bundled image understanding via `MiniMax-VL-01`
- Bundled music generation via `music-2.6`
- Bundled `web_search` through the MiniMax Token Plan search API

Provider split:

| Provider ID | Auth | Capabilities |
| --- | --- | --- |
| `minimax` | API key | Text, image generation, music generation, video generation, image understanding, speech, web search |
| `minimax-portal` | OAuth | Text, image generation, music generation, video generation, image understanding, speech |

## Built-in catalog

| Model | Type | Description |
| --- | --- | --- |
| `MiniMax-M2.7` | Chat (reasoning) | Default hosted reasoning model |
| `MiniMax-M2.7-highspeed` | Chat (reasoning) | Faster M2.7 reasoning tier |
| `MiniMax-VL-01` | Vision | Image understanding model |
| `image-01` | Image generation | Text-to-image and image-to-image editing |
| `music-2.6` | Music generation | Default music model |
| `music-2.5` | Music generation | Previous music generation tier |
| `music-2.0` | Music generation | Legacy music generation tier |
| `MiniMax-Hailuo-2.3` | Video generation | Text-to-video and image reference flows |

## Getting started

Choose your preferred auth method and follow the setup steps.

- OAuth (Coding Plan)

- API key

**Best for:** quick setup with MiniMax Coding Plan via OAuth, no API key required.

- International

- China

1

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Run onboarding

```
openclaw onboard --auth-choice minimax-global-oauth
```

This authenticates against `api.minimax.io`.

2

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Verify the model is available

```
openclaw models list --provider minimax-portal
```

1

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Run onboarding

```
openclaw onboard --auth-choice minimax-cn-oauth
```

This authenticates against `api.minimaxi.com`.

2

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Verify the model is available

```
openclaw models list --provider minimax-portal
```

OAuth setups use the `minimax-portal` provider id. Model refs follow the form `minimax-portal/MiniMax-M2.7`.

Referral link for MiniMax Coding Plan (10% off): [MiniMax Coding Plan](https://platform.minimax.io/subscribe/coding-plan?code=DbXJTRClnb&source=link)

**Best for:** hosted MiniMax with Anthropic-compatible API.

- International

- China

1

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Run onboarding

```
openclaw onboard --auth-choice minimax-global-api
```

This configures `api.minimax.io` as the base URL.

2

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Verify the model is available

```
openclaw models list --provider minimax
```

1

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Run onboarding

```
openclaw onboard --auth-choice minimax-cn-api
```

This configures `api.minimaxi.com` as the base URL.

2

[Navigate to header](https://docs.openclaw.ai/providers/minimax#)

Verify the model is available

```
openclaw models list --provider minimax
```

### Config example

```
{
  env: { MINIMAX_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "minimax/MiniMax-M2.7" } } },
  models: {
    mode: "merge",
    providers: {
      minimax: {
        baseUrl: "https://api.minimax.io/anthropic",
        apiKey: "${MINIMAX_API_KEY}",
        api: "anthropic-messages",
        models: [\
          {\
            id: "MiniMax-M2.7",\
            name: "MiniMax M2.7",\
            reasoning: true,\
            input: ["text"],\
            cost: { input: 0.3, output: 1.2, cacheRead: 0.06, cacheWrite: 0.375 },\
            contextWindow: 204800,\
            maxTokens: 131072,\
          },\
          {\
            id: "MiniMax-M2.7-highspeed",\
            name: "MiniMax M

_… [truncated; see https://docs.openclaw.ai/providers/minimax for full content]_


---

## Config example

_Source: <https://docs.openclaw.ai/providers/minimax.md>_

\
\> Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt
\> Use this file to discover all available pages before exploring further.

\# MiniMax

OpenClaw's MiniMax provider defaults to \*\*MiniMax M2.7\*\*.

MiniMax also provides:

\\* Bundled speech synthesis via T2A v2
\\* Bundled image understanding via \`MiniMax-VL-01\`
\\* Bundled music generation via \`music-2.6\`
\\* Bundled \`web\_search\` through the MiniMax Token Plan search API

Provider split:

\| Provider ID \| Auth \| Capabilities \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`minimax\` \| API key \| Text, image generation, music generation, video generation, image understanding, speech, web search \|
\| \`minimax-portal\` \| OAuth \| Text, image generation, music generation, video generation, image understanding, speech \|

\## Built-in catalog

\| Model \| Type \| Description \|
\| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \| \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- \|
\| \`MiniMax-M2.7\` \| Chat (reasoning) \| Default hosted reasoning model \|
\| \`MiniMax-M2.7-highspeed\` \| Chat (reasoning) \| Faster M2.7 reasoning tier \|
\| \`MiniMax-VL-01\` \| Vision \| Image understanding model \|
\| \`image-01\` \| Image generation \| Text-to-image and image-to-image editing \|
\| \`music-2.6\` \| Music generation \| Default music model \|
\| \`music-2.5\` \| Music generation \| Previous music generation tier \|
\| \`music-2.0\` \| Music generation \| Legacy music generation tier \|
\| \`MiniMax-Hailuo-2.3\` \| Video generation \| Text-to-video and image reference flows \|

\## Getting started

Choose your preferred auth method and follow the setup steps.

 \*\*Best for:\*\* quick setup with MiniMax Coding Plan via OAuth, no API key required.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw onboard --auth-choice minimax-global-oauth
 \`\`\`

 This authenticates against \`api.minimax.io\`.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw models list --provider minimax-portal
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw onboard --auth-choice minimax-cn-oauth
 \`\`\`

 This authenticates against \`api.minimaxi.com\`.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw models list --provider minimax-portal
 \`\`\`

 OAuth setups use the \`minimax-portal\` provider id. Model refs follow the form \`minimax-portal/MiniMax-M2.7\`.

 Referral link for MiniMax Coding Plan (10% off): \[MiniMax Coding Plan\](https://platform.minimax.io/subscribe/coding-plan?code=DbXJTRClnb\\&source=link)

 \*\*Best for:\*\* hosted MiniMax with Anthropic-compatible API.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw onboard --auth-choice minimax-global-api
 \`\`\`

 This configures \`api.minimax.io\` as the base URL.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw models list --provider minimax
 \`\`\`

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw onboard --auth-choice minimax-cn-api
 \`\`\`

 This configures \`api.minimaxi.com\` as the base URL.

 \`\`\`bash theme={"theme":{"light":"min-light","dark":"min-dark"}}
 openclaw models list --provider minimax
 \`\`\`

 ### Config example

 \`\`\`json5 theme={"theme":{"light":"min-light","dark":"min-dark"}}
 {
 env: { MINIMAX\_API\_KEY: "sk-..." },
 agents: { defaults: { model: { primary: "minimax/MiniMax-M2.7" } } },
 models: {
 mode: "merge",
 providers: {
 minimax: {
 baseUrl: "https://api.minimax.io/anthropic",
 apiKey: "${MINIMAX\_API\_KEY}",
 api: "anthropic-messages",
 models: \[\
 {\
 id: "MiniMax-M2.7",\

_… [truncated; see https://docs.openclaw.ai/providers/minimax.md for full content]_


---

## Mistral - OpenClaw

_Source: <https://docs.openclaw.ai/providers/mistral>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Mistral

OpenClaw supports Mistral for both text/image model routing (`mistral/...`) and
audio transcription via Voxtral in media understanding.
Mistral can also be used for memory embeddings (`memorySearch.provider = "mistral"`).

- Provider: `mistral`
- Auth: `MISTRAL_API_KEY`
- API: Mistral Chat Completions (`https://api.mistral.ai/v1`)

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/mistral#)

Get your API key

Create an API key in the [Mistral Console](https://console.mistral.ai/).

2

[Navigate to header](https://docs.openclaw.ai/providers/mistral#)

Run onboarding

```
openclaw onboard --auth-choice mistral-api-key
```

Or pass the key directly:

```
openclaw onboard --mistral-api-key "$MISTRAL_API_KEY"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/mistral#)

Set a default model

```
{
  env: { MISTRAL_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "mistral/mistral-large-latest" } } },
}
```

4

[Navigate to header](https://docs.openclaw.ai/providers/mistral#)

Verify the model is available

```
openclaw models list --provider mistral
```

## Built-in LLM catalog

OpenClaw currently ships this bundled Mistral catalog:

| Model ref | Input | Context | Max output | Notes |
| --- | --- | --- | --- | --- |
| `mistral/mistral-large-latest` | text, image | 262,144 | 16,384 | Default model |
| `mistral/mistral-medium-2508` | text, image | 262,144 | 8,192 | Mistral Medium 3.1 |
| `mistral/mistral-small-latest` | text, image | 128,000 | 16,384 | Mistral Small 4; adjustable reasoning via API `reasoning_effort` |
| `mistral/pixtral-large-latest` | text, image | 128,000 | 32,768 | Pixtral |
| `mistral/codestral-latest` | text | 256,000 | 4,096 | Coding |
| `mistral/devstral-medium-latest` | text | 262,144 | 32,768 | Devstral 2 |
| `mistral/magistral-small` | text | 128,000 | 40,000 | Reasoning-enabled |

## Audio transcription (Voxtral)

Use Voxtral for batch audio transcription through the media understanding
pipeline.

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "mistral", model: "voxtral-mini-latest" }],
      },
    },
  },
}
```

The media transcription path uses `/v1/audio/transcriptions`. The default audio model for Mistral is `voxtral-mini-latest`.

## Voice Call streaming STT

The bundled `mistral` plugin registers Voxtral Realtime as a Voice Call
streaming STT provider.

| Setting | Config path | Default |
| --- | --- | --- |
| API key | `plugins.entries.voice-call.config.streaming.providers.mistral.apiKey` | Falls back to `MISTRAL_API_KEY` |
| Model | `...mistral.model` | `voxtral-mini-transcribe-realtime-2602` |
| Encoding | `...mistral.encoding` | `pcm_mulaw` |
| Sample rate | `...mistral.sampleRate` | `8000` |
| Target delay | `...mistral.targetStreamingDelayMs` | `800` |

```
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          streaming: {
            enabled: true,
            provider: "mistral",
            providers: {
              mistral: {
                apiKey: "${MISTRAL_API_KEY}",
                targetStreamingDelayMs: 800,
              },
            },
          },
        },
      },
    },
  },
}
```

OpenClaw defaults Mistral realtime STT to `pcm_mulaw` at 8 kHz so Voice Call
can forward Twilio media frames directly. Use `encoding: "pcm_s16le"` and a
matching `sampleRate` only if your upstream stream is already raw PCM.

## Advanced configuration

Adjustable reasoning (mistral-small-latest)

`mistral/mistral-small-latest` maps to Mistral Small 4 and supports [adjustable reasoning](https://docs.mistral.ai/capabilities/reasoning/adjustable) on the Chat Completions API via `reasoning_effort` (`none` minimizes extra thinking in the output; `high` surfaces full thinking traces before the final answer).OpenClaw maps the session **thinking** level to Mistral’s API:

| OpenClaw thinking level | Mistral

_… [truncated; see https://docs.openclaw.ai/providers/mistral for full content]_


---

## Model provider quickstart - OpenClaw

_Source: <https://docs.openclaw.ai/providers/models>_

# Model Providers

OpenClaw can use many LLM providers. Pick one, authenticate, then set the default
model as `provider/model`.

## Quick start (two steps)

1. Authenticate with the provider (usually via `openclaw onboard`).
2. Set the default model:

```
{
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-6" } } },
}
```

## Supported providers (starter set)

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

## Additional bundled provider variants

- `anthropic-vertex` \- implicit Anthropic on Google Vertex support when Vertex credentials are available; no separate onboarding auth choice
- `copilot-proxy` \- local VS Code Copilot Proxy bridge; use `openclaw onboard --auth-choice copilot-proxy`
- `google-gemini-cli` \- unofficial Gemini CLI OAuth flow; requires a local `gemini` install (`brew install gemini-cli` or `npm install -g @google/gemini-cli`); default model `google-gemini-cli/gemini-3-flash-preview`; use `openclaw onboard --auth-choice google-gemini-cli` or `openclaw models auth login --provider google-gemini-cli --set-default`

For the full provider catalog (xAI, Groq, Mistral, etc.) and advanced configuration,
see [Model providers](https://docs.openclaw.ai/concepts/model-providers).

## Related

- [Model selection](https://docs.openclaw.ai/concepts/model-providers)
- [Model failover](https://docs.openclaw.ai/concepts/model-failover)
- [Models CLI](https://docs.openclaw.ai/cli/models)

[Provider directory](https://docs.openclaw.ai/providers) [Models CLI](https://docs.openclaw.ai/concepts/models)

Ctrl+I


---

## Moonshot AI - OpenClaw

_Source: <https://docs.openclaw.ai/providers/moonshot>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Moonshot AI

Moonshot provides the Kimi API with OpenAI-compatible endpoints. Configure the
provider and set the default model to `moonshot/kimi-k2.6`, or use
Kimi Coding with `kimi/kimi-code`.

Moonshot and Kimi Coding are **separate providers**. Keys are not interchangeable, endpoints differ, and model refs differ (`moonshot/...` vs `kimi/...`).

## Built-in model catalog

| Model ref | Name | Reasoning | Input | Context | Max output |
| --- | --- | --- | --- | --- | --- |
| `moonshot/kimi-k2.6` | Kimi K2.6 | No | text, image | 262,144 | 262,144 |
| `moonshot/kimi-k2.5` | Kimi K2.5 | No | text, image | 262,144 | 262,144 |
| `moonshot/kimi-k2-thinking` | Kimi K2 Thinking | Yes | text | 262,144 | 262,144 |
| `moonshot/kimi-k2-thinking-turbo` | Kimi K2 Thinking Turbo | Yes | text | 262,144 | 262,144 |
| `moonshot/kimi-k2-turbo` | Kimi K2 Turbo | No | text | 256,000 | 16,384 |

Bundled cost estimates for current Moonshot-hosted K2 models use Moonshot’s
published pay-as-you-go rates: Kimi K2.6 is 0.16/MTokcachehit,0.16/MTok cache hit,
0.16/MTokcachehit,0.95/MTok input, and 4.00/MTokoutput;KimiK2.5is4.00/MTok output; Kimi K2.5 is 4.00/MTokoutput;KimiK2.5is0.10/MTok cache hit,
0.60/MTokinput,and0.60/MTok input, and 0.60/MTokinput,and3.00/MTok output. Other legacy catalog entries keep
zero-cost placeholders unless you override them in config.

## Getting started

Choose your provider and follow the setup steps.

- Moonshot API

- Kimi Coding

**Best for:** Kimi K2 models via the Moonshot Open Platform.

1

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Choose your endpoint region

| Auth choice | Endpoint | Region |
| --- | --- | --- |
| `moonshot-api-key` | `https://api.moonshot.ai/v1` | International |
| `moonshot-api-key-cn` | `https://api.moonshot.cn/v1` | China |

2

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Run onboarding

```
openclaw onboard --auth-choice moonshot-api-key
```

Or for the China endpoint:

```
openclaw onboard --auth-choice moonshot-api-key-cn
```

3

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Set a default model

```
{
  agents: {
    defaults: {
      model: { primary: "moonshot/kimi-k2.6" },
    },
  },
}
```

4

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Verify models are available

```
openclaw models list --provider moonshot
```

5

[Navigate to header](https://docs.openclaw.ai/providers/moonshot#)

Run a live smoke test

Use an isolated state dir when you want to verify model access and cost
tracking without touching your normal sessions:

```
OPENCLAW_CONFIG_PATH=/tmp/openclaw-kimi/openclaw.json \
OPENCLAW_STATE_DIR=/tmp/openclaw-kimi \
openclaw agent --local \
  --session-id live-kimi-cost \
  --message 'Reply exactly: KIMI_LIVE_OK' \
  --thinking off \
  --json
```

The JSON response should report `provider: "moonshot"` and
`model: "kimi-k2.6"`. The assistant transcript entry stores normalized
token usage plus estimated cost under `usage.cost` when Moonshot returns
usage metadata.

### Config example

```
{
  env: { MOONSHOT_API_KEY: "sk-..." },
  agents: {
    defaults: {
      model: { primary: "moonshot/kimi-k2.6" },
      models: {
        // moonshot-kimi-k2-aliases:start
        "moonshot/kimi-k2.6": { alias: "Kimi K2.6" },
        "moonshot/kimi-k2.5": { alias: "Kimi K2.5" },
        "moonshot/kimi-k2-thinking": { alias: "Kimi K2 Thinking" },
        "moonshot/kimi-k2-thinking-turbo": { alias: "Kimi K2 Thinking Turbo" },
        "moonshot/kimi-k2-turbo": { alias: "Kimi K2 Turbo" },
        // moonshot-kimi-k2-aliases:end
      },
    },
  },
  models: {
    mode: "merge",
    providers: {
      moonshot: {
        baseUrl: "https://api.moonshot.ai/v1",
        apiKey: "${MOONSHOT_API_KEY}",
        api: "openai-completions",
        models: [\
          // moonshot-kimi-k2-models:start\
          {\
            id: "kimi-k2.6",\

_… [truncated; see https://docs.openclaw.ai/providers/moonshot for full content]_


---

## NVIDIA - OpenClaw

_Source: <https://docs.openclaw.ai/providers/nvidia>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

NVIDIA

NVIDIA provides an OpenAI-compatible API at `https://integrate.api.nvidia.com/v1` for
open models for free. Authenticate with an API key from
[build.nvidia.com](https://build.nvidia.com/settings/api-keys).

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/nvidia#)

Get your API key

Create an API key at [build.nvidia.com](https://build.nvidia.com/settings/api-keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/nvidia#)

Export the key and run onboarding

```
export NVIDIA_API_KEY="nvapi-..."
openclaw onboard --auth-choice nvidia-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/nvidia#)

Set an NVIDIA model

```
openclaw models set nvidia/nvidia/nemotron-3-super-120b-a12b
```

If you pass `--nvidia-api-key` instead of the env var, the value lands in shell
history and `ps` output. Prefer the `NVIDIA_API_KEY` environment variable when
possible.

For non-interactive setup, you can also pass the key directly:

```
openclaw onboard --auth-choice nvidia-api-key --nvidia-api-key "nvapi-..."
```

## Config example

```
{
  env: { NVIDIA_API_KEY: "nvapi-..." },
  models: {
    providers: {
      nvidia: {
        baseUrl: "https://integrate.api.nvidia.com/v1",
        api: "openai-completions",
      },
    },
  },
  agents: {
    defaults: {
      model: { primary: "nvidia/nvidia/nemotron-3-super-120b-a12b" },
    },
  },
}
```

## Built-in catalog

| Model ref | Name | Context | Max output |
| --- | --- | --- | --- |
| `nvidia/nvidia/nemotron-3-super-120b-a12b` | NVIDIA Nemotron 3 Super 120B | 262,144 | 8,192 |
| `nvidia/moonshotai/kimi-k2.5` | Kimi K2.5 | 262,144 | 8,192 |
| `nvidia/minimaxai/minimax-m2.5` | Minimax M2.5 | 196,608 | 8,192 |
| `nvidia/z-ai/glm5` | GLM 5 | 202,752 | 8,192 |

## Advanced configuration

Auto-enable behavior

The provider auto-enables when the `NVIDIA_API_KEY` environment variable is set.
No explicit provider config is required beyond the key.

Catalog and pricing

The bundled catalog is static. Costs default to `0` in source since NVIDIA
currently offers free API access for the listed models.

OpenAI-compatible endpoint

NVIDIA uses the standard `/v1` completions endpoint. Any OpenAI-compatible
tooling should work out of the box with the NVIDIA base URL.

NVIDIA models are currently free to use. Check
[build.nvidia.com](https://build.nvidia.com/) for the latest availability and
rate-limit details.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config reference for agents, models, and providers.](https://docs.openclaw.ai/gateway/configuration-reference)

[Moonshot AI](https://docs.openclaw.ai/providers/moonshot) [Ollama](https://docs.openclaw.ai/providers/ollama)

Ctrl+I


---

## Ollama - OpenClaw

_Source: <https://docs.openclaw.ai/providers/ollama>_

# or
ollama pull gpt-oss:20b
# or
ollama pull llama3.3
```

3

[Navigate to header](https://docs.openclaw.ai/providers/ollama#)

Enable Ollama for OpenClaw

For `Cloud only`, use your real `OLLAMA_API_KEY`. For host-backed setups, any placeholder value works:

```
# Cloud
export OLLAMA_API_KEY="your-ollama-api-key"

# Local-only
export OLLAMA_API_KEY="ollama-local"

# Or configure in your config file
openclaw config set models.providers.ollama.apiKey "OLLAMA_API_KEY"
```

4

[Navigate to header](https://docs.openclaw.ai/providers/ollama#)

Inspect and set your model

```
openclaw models list
openclaw models set ollama/gemma4
```

Or set the default in config:

```
{
  agents: {
    defaults: {
      model: { primary: "ollama/gemma4" },
    },
  },
}
```

## Cloud models

- Cloud + Local

- Cloud only

- Local only

`Cloud + Local` uses a reachable Ollama host as the control point for both local and cloud models. This is Ollama’s preferred hybrid flow.Use **Cloud + Local** during setup. OpenClaw prompts for the Ollama base URL, discovers local models from that host, and checks whether the host is signed in for cloud access with `ollama signin`. When the host is signed in, OpenClaw also suggests hosted cloud defaults such as `kimi-k2.5:cloud`, `minimax-m2.7:cloud`, and `glm-5.1:cloud`.If the host is not signed in yet, OpenClaw keeps the setup local-only until you run `ollama signin`.

`Cloud only` runs against Ollama’s hosted API at `https://ollama.com`.Use **Cloud only** during setup. OpenClaw prompts for `OLLAMA_API_KEY`, sets `baseUrl: "https://ollama.com"`, and seeds the hosted cloud model list. This path does **not** require a local Ollama server or `ollama signin`.The cloud model list shown during `openclaw onboard` is populated live from `https://ollama.com/api/tags`, capped at 500 entries, so the picker reflects the current hosted catalog rather than a static seed. If `ollama.com` is unreachable or returns no models at setup time, OpenClaw falls back to the previous hardcoded suggestions so onboarding still completes.

In local-only mode, OpenClaw discovers models from the configured Ollama instance. This path is for local or self-hosted Ollama servers.OpenClaw currently suggests `gemma4` as the local default.

## Model discovery (implicit provider)

When you set `OLLAMA_API_KEY` (or an auth profile) and **do not** define `models.providers.ollama` or another custom remote provider with `api: "ollama"`, OpenClaw discovers models from the local Ollama instance at `http://127.0.0.1:11434`.

| Behavior | Detail |
| --- | --- |
| Catalog query | Queries `/api/tags` |
| Capability detection | Uses best-effort `/api/show` lookups to read `contextWindow`, expanded `num_ctx` Modelfile parameters, and capabilities including vision/tools |
| Vision models | Models with a `vision` capability reported by `/api/show` are marked as image-capable (`input: ["text", "image"]`), so OpenClaw auto-injects images into the prompt |
| Reasoning detection | Uses `/api/show` capabilities when available, including `thinking`; falls back to a model-name heuristic (`r1`, `reasoning`, `think`) when Ollama omits capabilities |
| Token limits | Sets `maxTokens` to the default Ollama max-token cap used by OpenClaw |
| Costs | Sets all costs to `0` |

This avoids manual model entries while keeping the catalog aligned with the local Ollama instance. You can use a full ref such as `ollama/<pulled-model>:latest` in local `infer model run`; OpenClaw resolves that installed model from Ollama’s live catalog without requiring a hand-written `models.json` entry.For signed-in Ollama hosts, some `:cloud` models may be usable through `/api/chat`
and `/api/show` before they appear in `/api/tags`. When you explicitly select a
full `ollama/<model>:cloud` ref, OpenClaw validates that exact missing model with
`/api/show` and adds it to the runtime catalog only if Ollama confirms model
metadata. Typos still fail as unknown models instead of being auto-created.

```
# See

_… [truncated; see https://docs.openclaw.ai/providers/ollama for full content]_


---

## OpenAI - OpenClaw

_Source: <https://docs.openclaw.ai/providers/openai>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

OpenAI

OpenAI provides developer APIs for GPT models, and Codex is also available as a
ChatGPT-plan coding agent through OpenAI’s Codex clients. OpenClaw keeps those
surfaces separate so config stays predictable.OpenClaw supports three OpenAI-family routes. Most ChatGPT/Codex subscribers
who want Codex behavior should use the native Codex app-server runtime. The
model prefix selects the provider/model name; a separate runtime setting selects
who executes the embedded agent loop:

- **API key** \- direct OpenAI Platform access with usage-based billing (`openai/*` models)
- **Codex subscription with native Codex runtime** \- ChatGPT/Codex sign-in plus Codex app-server execution (`openai/*` models plus `agents.defaults.agentRuntime.id: "codex"`)
- **Codex subscription through PI** \- ChatGPT/Codex sign-in with the normal OpenClaw PI runner (`openai-codex/*` models)

OpenAI explicitly supports subscription OAuth usage in external tools and workflows like OpenClaw.Provider, model, runtime, and channel are separate layers. If those labels are
getting mixed together, read [Agent runtimes](https://docs.openclaw.ai/concepts/agent-runtimes) before
changing config.

## Quick choice

| Goal | Use | Notes |
| --- | --- | --- |
| ChatGPT/Codex subscription with native Codex runtime | `openai/gpt-5.5` plus `agentRuntime.id: "codex"` | Recommended Codex setup for most users. Sign in with `openai-codex` auth. |
| Direct API-key billing | `openai/gpt-5.5` | Set `OPENAI_API_KEY` or run OpenAI API-key onboarding. |
| ChatGPT/Codex subscription auth through PI | `openai-codex/gpt-5.5` | Use only when you intentionally want the normal PI runner. |
| Image generation or editing | `openai/gpt-image-2` | Works with either `OPENAI_API_KEY` or OpenAI Codex OAuth. |
| Transparent-background images | `openai/gpt-image-1.5` | Use `outputFormat=png` or `webp` and `openai.background=transparent`. |

## Naming map

The names are similar but not interchangeable:

| Name you see | Layer | Meaning |
| --- | --- | --- |
| `openai` | Provider prefix | Direct OpenAI Platform API route. |
| `openai-codex` | Provider prefix | OpenAI Codex OAuth/subscription route through the normal OpenClaw PI runner. |
| `codex` plugin | Plugin | Bundled OpenClaw plugin that provides native Codex app-server runtime and `/codex` chat controls. |
| `agentRuntime.id: codex` | Agent runtime | Force the native Codex app-server harness for embedded turns. |
| `/codex ...` | Chat command set | Bind/control Codex app-server threads from a conversation. |
| `runtime: "acp", agentId: "codex"` | ACP session route | Explicit fallback path that runs Codex through ACP/acpx. |

This means a config can intentionally contain both `openai-codex/*` and the
`codex` plugin. That is valid when you want Codex OAuth through PI and also want
native `/codex` chat controls available. `openclaw doctor` warns about that
combination so you can confirm it is intentional; it does not rewrite it.

GPT-5.5 is available through both direct OpenAI Platform API-key access and
subscription/OAuth routes. For ChatGPT/Codex subscription plus native Codex
execution, use `openai/gpt-5.5` with `agentRuntime.id: "codex"`. Use
`openai-codex/gpt-5.5` only for Codex OAuth through PI, or `openai/gpt-5.5`
without a Codex runtime override for direct `OPENAI_API_KEY` traffic.

Enabling the OpenAI plugin, or selecting an `openai-codex/*` model, does not
enable the bundled Codex app-server plugin. OpenClaw enables that plugin only
when you explicitly select the native Codex harness with
`agentRuntime.id: "codex"` or use a legacy `codex/*` model ref.
If the bundled `codex` plugin is enabled but `openai-codex/*` still resolves
through PI, `openclaw doctor` warns and leaves the route unchanged.

## OpenClaw feature coverage

| OpenAI capability | OpenClaw surface | Status |
| --- | --- | --- |
| Chat / Responses | `openai/<model>` model provider | Yes |
| Codex subscription mode

_… [truncated; see https://docs.openclaw.ai/providers/openai for full content]_


---

## OpenCode - OpenClaw

_Source: <https://docs.openclaw.ai/providers/opencode>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

OpenCode

OpenCode exposes two hosted catalogs in OpenClaw:

| Catalog | Prefix | Runtime provider |
| --- | --- | --- |
| **Zen** | `opencode/...` | `opencode` |
| **Go** | `opencode-go/...` | `opencode-go` |

Both catalogs use the same OpenCode API key. OpenClaw keeps the runtime provider ids
split so upstream per-model routing stays correct, but onboarding and docs treat them
as one OpenCode setup.

## Getting started

- Zen catalog

- Go catalog

**Best for:** the curated OpenCode multi-model proxy (Claude, GPT, Gemini).

1

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Run onboarding

```
openclaw onboard --auth-choice opencode-zen
```

Or pass the key directly:

```
openclaw onboard --opencode-zen-api-key "$OPENCODE_API_KEY"
```

2

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Set a Zen model as the default

```
openclaw config set agents.defaults.model.primary "opencode/claude-opus-4-6"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Verify models are available

```
openclaw models list --provider opencode
```

**Best for:** the OpenCode-hosted Kimi, GLM, and MiniMax lineup.

1

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Run onboarding

```
openclaw onboard --auth-choice opencode-go
```

Or pass the key directly:

```
openclaw onboard --opencode-go-api-key "$OPENCODE_API_KEY"
```

2

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Set a Go model as the default

```
openclaw config set agents.defaults.model.primary "opencode-go/kimi-k2.6"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/opencode#)

Verify models are available

```
openclaw models list --provider opencode-go
```

## Config example

```
{
  env: { OPENCODE_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "opencode/claude-opus-4-6" } } },
}
```

## Built-in catalogs

### Zen

| Property | Value |
| --- | --- |
| Runtime provider | `opencode` |
| Example models | `opencode/claude-opus-4-6`, `opencode/gpt-5.5`, `opencode/gemini-3-pro` |

### Go

| Property | Value |
| --- | --- |
| Runtime provider | `opencode-go` |
| Example models | `opencode-go/kimi-k2.6`, `opencode-go/glm-5`, `opencode-go/minimax-m2.5` |

## Advanced configuration

API key aliases

`OPENCODE_ZEN_API_KEY` is also supported as an alias for `OPENCODE_API_KEY`.

Shared credentials

Entering one OpenCode key during setup stores credentials for both runtime
providers. You do not need to onboard each catalog separately.

Billing and dashboard

You sign in to OpenCode, add billing details, and copy your API key. Billing
and catalog availability are managed from the OpenCode dashboard.

Gemini replay behavior

Gemini-backed OpenCode refs stay on the proxy-Gemini path, so OpenClaw keeps
Gemini thought-signature sanitation there without enabling native Gemini
replay validation or bootstrap rewrites.

Non-Gemini replay behavior

Non-Gemini OpenCode refs keep the minimal OpenAI-compatible replay policy.

Entering one OpenCode key during setup stores credentials for both the Zen and
Go runtime providers, so you only need to onboard once.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config reference for agents, models, and providers.](https://docs.openclaw.ai/gateway/configuration-reference)

[OpenAI](https://docs.openclaw.ai/providers/openai) [OpenCode Go](https://docs.openclaw.ai/providers/opencode-go)

Ctrl+I


---

## OpenCode Go - OpenClaw

_Source: <https://docs.openclaw.ai/providers/opencode-go>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

OpenCode Go

OpenCode Go is the Go catalog within [OpenCode](https://docs.openclaw.ai/providers/opencode).
It uses the same `OPENCODE_API_KEY` as the Zen catalog, but keeps the runtime
provider id `opencode-go` so upstream per-model routing stays correct.

| Property | Value |
| --- | --- |
| Runtime provider | `opencode-go` |
| Auth | `OPENCODE_API_KEY` |
| Parent setup | [OpenCode](https://docs.openclaw.ai/providers/opencode) |

## Built-in catalog

OpenClaw sources most Go catalog rows from the bundled pi model registry and
supplements current upstream rows while the registry catches up. Run
`openclaw models list --provider opencode-go` for the current model list.The provider includes:

| Model ref | Name |
| --- | --- |
| `opencode-go/glm-5` | GLM-5 |
| `opencode-go/glm-5.1` | GLM-5.1 |
| `opencode-go/kimi-k2.5` | Kimi K2.5 |
| `opencode-go/kimi-k2.6` | Kimi K2.6 (3x limits) |
| `opencode-go/deepseek-v4-pro` | DeepSeek V4 Pro |
| `opencode-go/deepseek-v4-flash` | DeepSeek V4 Flash |
| `opencode-go/mimo-v2-omni` | MiMo V2 Omni |
| `opencode-go/mimo-v2-pro` | MiMo V2 Pro |
| `opencode-go/minimax-m2.5` | MiniMax M2.5 |
| `opencode-go/minimax-m2.7` | MiniMax M2.7 |
| `opencode-go/qwen3.5-plus` | Qwen3.5 Plus |
| `opencode-go/qwen3.6-plus` | Qwen3.6 Plus |

## Getting started

- Interactive

- Non-interactive

1

[Navigate to header](https://docs.openclaw.ai/providers/opencode-go#)

Run onboarding

```
openclaw onboard --auth-choice opencode-go
```

2

[Navigate to header](https://docs.openclaw.ai/providers/opencode-go#)

Set a Go model as default

```
openclaw config set agents.defaults.model.primary "opencode-go/kimi-k2.6"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/opencode-go#)

Verify models are available

```
openclaw models list --provider opencode-go
```

1

[Navigate to header](https://docs.openclaw.ai/providers/opencode-go#)

Pass the key directly

```
openclaw onboard --opencode-go-api-key "$OPENCODE_API_KEY"
```

2

[Navigate to header](https://docs.openclaw.ai/providers/opencode-go#)

Verify models are available

```
openclaw models list --provider opencode-go
```

## Config example

```
{
  env: { OPENCODE_API_KEY: "YOUR_API_KEY_HERE" }, // pragma: allowlist secret
  agents: { defaults: { model: { primary: "opencode-go/kimi-k2.6" } } },
}
```

## Advanced configuration

Routing behavior

OpenClaw handles per-model routing automatically when the model ref uses
`opencode-go/...`. No additional provider config is required.

Runtime ref convention

Runtime refs stay explicit: `opencode/...` for Zen, `opencode-go/...` for Go.
This keeps upstream per-model routing correct across both catalogs.

Shared credentials

The same `OPENCODE_API_KEY` is used by both the Zen and Go catalogs. Entering
the key during setup stores credentials for both runtime providers.

See [OpenCode](https://docs.openclaw.ai/providers/opencode) for the shared onboarding overview and the full
Zen + Go catalog reference.

## Related

[**OpenCode (parent)** \\
\\
Shared onboarding, catalog overview, and advanced notes.](https://docs.openclaw.ai/providers/opencode)

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[OpenCode](https://docs.openclaw.ai/providers/opencode) [OpenRouter](https://docs.openclaw.ai/providers/openrouter)

Ctrl+I


---

## OpenRouter - OpenClaw

_Source: <https://docs.openclaw.ai/providers/openrouter>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

OpenRouter

OpenRouter provides a **unified API** that routes requests to many models behind a single
endpoint and API key. It is OpenAI-compatible, so most OpenAI SDKs work by switching the base URL.

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/openrouter#)

Get your API key

Create an API key at [openrouter.ai/keys](https://openrouter.ai/keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/openrouter#)

Run onboarding

```
openclaw onboard --auth-choice openrouter-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/openrouter#)

(Optional) Switch to a specific model

Onboarding defaults to `openrouter/auto`. Pick a concrete model later:

```
openclaw models set openrouter/<provider>/<model>
```

## Config example

```
{
  env: { OPENROUTER_API_KEY: "sk-or-..." },
  agents: {
    defaults: {
      model: { primary: "openrouter/auto" },
    },
  },
}
```

## Model references

Model refs follow the pattern `openrouter/<provider>/<model>`. For the full list of
available providers and models, see [/concepts/model-providers](https://docs.openclaw.ai/concepts/model-providers).

Bundled fallback examples:

| Model ref | Notes |
| --- | --- |
| `openrouter/auto` | OpenRouter automatic routing |
| `openrouter/moonshotai/kimi-k2.6` | Kimi K2.6 via MoonshotAI |

## Image generation

OpenRouter can also back the `image_generate` tool. Use an OpenRouter image model under `agents.defaults.imageGenerationModel`:

```
{
  env: { OPENROUTER_API_KEY: "sk-or-..." },
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "openrouter/google/gemini-3.1-flash-image-preview",
        timeoutMs: 180_000,
      },
    },
  },
}
```

OpenClaw sends image requests to OpenRouter’s chat completions image API with `modalities: ["image", "text"]`. Gemini image models receive supported `aspectRatio` and `resolution` hints through OpenRouter’s `image_config`. Use `agents.defaults.imageGenerationModel.timeoutMs` for slower OpenRouter image models; the `image_generate` tool’s per-call `timeoutMs` parameter still wins.

## Video generation

OpenRouter can also back the `video_generate` tool through its asynchronous `/videos` API. Use an OpenRouter video model under `agents.defaults.videoGenerationModel`:

```
{
  env: { OPENROUTER_API_KEY: "sk-or-..." },
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "openrouter/google/veo-3.1-fast",
      },
    },
  },
}
```

OpenClaw submits text-to-video and image-to-video jobs to OpenRouter, polls
the returned `polling_url`, and downloads the completed video from
OpenRouter’s `unsigned_urls` or the documented job content endpoint.
Reference images are sent as first/last frame images by default; images
tagged with `reference_image` are sent as OpenRouter input references. The
bundled `google/veo-3.1-fast` default advertises the currently supported 4/6/8
second durations, `720P`/`1080P` resolutions, and `16:9`/`9:16` aspect
ratios. Video-to-video is not registered for OpenRouter because the upstream
video generation API currently accepts text and image references.

## Text-to-speech

OpenRouter can also be used as a TTS provider through its OpenAI-compatible
`/audio/speech` endpoint.

```
{
  messages: {
    tts: {
      auto: "always",
      provider: "openrouter",
      providers: {
        openrouter: {
          model: "hexgrad/kokoro-82m",
          voice: "af_alloy",
          responseFormat: "mp3",
        },
      },
    },
  },
}
```

If `messages.tts.providers.openrouter.apiKey` is omitted, TTS reuses
`models.providers.openrouter.apiKey`, then `OPENROUTER_API_KEY`.

## Authentication and headers

OpenRouter uses a Bearer token with your API key under the hood.On real OpenRouter requests (`https://openrouter.ai/api/v1`), OpenClaw also adds
OpenRouter’s documented app-attribution headers:

| Header | Value |
| --- | --- |
| `HTTP-Referer` | `ht

_… [truncated; see https://docs.openclaw.ai/providers/openrouter for full content]_


---

## Qianfan - OpenClaw

_Source: <https://docs.openclaw.ai/providers/qianfan>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Qianfan

Qianfan is Baidu’s MaaS platform, providing a **unified API** that routes requests to many models behind a single
endpoint and API key. It is OpenAI-compatible, so most OpenAI SDKs work by switching the base URL.

| Property | Value |
| --- | --- |
| Provider | `qianfan` |
| Auth | `QIANFAN_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://qianfan.baidubce.com/v2` |

## Getting started

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

## Built-in catalog

| Model ref | Input | Context | Max output | Reasoning | Notes |
| --- | --- | --- | --- | --- | --- |
| `qianfan/deepseek-v3.2` | text | 98,304 | 32,768 | Yes | Default model |
| `qianfan/ernie-5.0-thinking-preview` | text, image | 119,000 | 64,000 | Yes | Multimodal |

The default bundled model ref is `qianfan/deepseek-v3.2`. You only need to override `models.providers.qianfan` when you need a custom base URL or model metadata.

## Config example

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

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full OpenClaw configuration reference.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Agent setup** \\
\\
Configuring agent defaults and model assignments.](https://docs.openclaw.ai/concepts/agent)

[**Qianfan API docs*

_… [truncated; see https://docs.openclaw.ai/providers/qianfan for full content]_


---

## Qwen - OpenClaw

_Source: <https://docs.openclaw.ai/providers/qwen>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Qwen

**Qwen OAuth has been removed.** The free-tier OAuth integration
(`qwen-portal`) that used `portal.qwen.ai` endpoints is no longer available.
See [Issue #49557](https://github.com/openclaw/openclaw/issues/49557) for
background.

OpenClaw now treats Qwen as a first-class bundled provider with canonical id
`qwen`. The bundled provider targets the Qwen Cloud / Alibaba DashScope and
Coding Plan endpoints and keeps legacy `modelstudio` ids working as a
compatibility alias.

- Provider: `qwen`
- Preferred env var: `QWEN_API_KEY`
- Also accepted for compatibility: `MODELSTUDIO_API_KEY`, `DASHSCOPE_API_KEY`
- API style: OpenAI-compatible

If you want `qwen3.6-plus`, prefer the **Standard (pay-as-you-go)** endpoint.
Coding Plan support can lag behind the public catalog.

## Getting started

Choose your plan type and follow the setup steps.

- Coding Plan (subscription)

- Standard (pay-as-you-go)

**Best for:** subscription-based access through the Qwen Coding Plan.

1

[Navigate to header](https://docs.openclaw.ai/providers/qwen#)

Get your API key

Create or copy an API key from [home.qwencloud.com/api-keys](https://home.qwencloud.com/api-keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/qwen#)

Run onboarding

For the **Global** endpoint:

```
openclaw onboard --auth-choice qwen-api-key
```

For the **China** endpoint:

```
openclaw onboard --auth-choice qwen-api-key-cn
```

3

[Navigate to header](https://docs.openclaw.ai/providers/qwen#)

Set a default model

```
{
  agents: {
    defaults: {
      model: { primary: "qwen/qwen3.5-plus" },
    },
  },
}
```

4

[Navigate to header](https://docs.openclaw.ai/providers/qwen#)

Verify the model is available

```
openclaw models list --provider qwen
```

Legacy `modelstudio-*` auth-choice ids and `modelstudio/...` model refs still
work as compatibility aliases, but new setup flows should prefer the canonical
`qwen-*` auth-choice ids and `qwen/...` model refs. If you define an exact
custom `models.providers.modelstudio` entry with another `api` value, that
custom provider owns `modelstudio/...` refs instead of the Qwen compatibility
alias.

**Best for:** pay-as-you-go access through the Standard Model Studio endpoint, including models like `qwen3.6-plus` that may not be available on the Coding Plan.

1

[Navigate to header](https://docs.openclaw.ai/providers/qwen#)

Get your API key

Create or copy an API key from [home.qwencloud.com/api-keys](https://home.qwencloud.com/api-keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/qwen#)

Run onboarding

For the **Global** endpoint:

```
openclaw onboard --auth-choice qwen-standard-api-key
```

For the **China** endpoint:

```
openclaw onboard --auth-choice qwen-standard-api-key-cn
```

3

[Navigate to header](https://docs.openclaw.ai/providers/qwen#)

Set a default model

```
{
  agents: {
    defaults: {
      model: { primary: "qwen/qwen3.5-plus" },
    },
  },
}
```

4

[Navigate to header](https://docs.openclaw.ai/providers/qwen#)

Verify the model is available

```
openclaw models list --provider qwen
```

Legacy `modelstudio-*` auth-choice ids and `modelstudio/...` model refs still
work as compatibility aliases, but new setup flows should prefer the canonical
`qwen-*` auth-choice ids and `qwen/...` model refs. If you define an exact
custom `models.providers.modelstudio` entry with another `api` value, that
custom provider owns `modelstudio/...` refs instead of the Qwen compatibility
alias.

## Plan types and endpoints

| Plan | Region | Auth choice | Endpoint |
| --- | --- | --- | --- |
| Standard (pay-as-you-go) | China | `qwen-standard-api-key-cn` | `dashscope.aliyuncs.com/compatible-mode/v1` |
| Standard (pay-as-you-go) | Global | `qwen-standard-api-key` | `dashscope-intl.aliyuncs.com/compatible-mode/v1` |
| Coding Plan (subscription) | China | `qwen-api-key-cn` | `coding.dashscope.aliyuncs.com/v1` |
| Coding Plan (subscription) | Global

_… [truncated; see https://docs.openclaw.ai/providers/qwen for full content]_


---

## SenseAudio - OpenClaw

_Source: <https://docs.openclaw.ai/providers/senseaudio>_

# SenseAudio

SenseAudio can transcribe inbound audio/voice-note attachments through
OpenClaw’s shared `tools.media.audio` pipeline. OpenClaw posts multipart audio
to the OpenAI-compatible transcription endpoint and injects the returned text
as `{{Transcript}}` plus an `[Audio]` block.

| Detail | Value |
| --- | --- |
| Website | [senseaudio.cn](https://senseaudio.cn/) |
| Docs | [senseaudio.cn/docs](https://senseaudio.cn/docs) |
| Auth | `SENSEAUDIO_API_KEY` |
| Default model | `senseaudio-asr-pro-1.5-260319` |
| Default URL | `https://api.senseaudio.cn/v1` |

## Getting Started

1

[Navigate to header](https://docs.openclaw.ai/providers/senseaudio#)

Set your API key

```
export SENSEAUDIO_API_KEY="..."
```

2

[Navigate to header](https://docs.openclaw.ai/providers/senseaudio#)

Enable the audio provider

```
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "senseaudio", model: "senseaudio-asr-pro-1.5-260319" }],
      },
    },
  },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/senseaudio#)

Send a voice note

Send an audio message through any connected channel. OpenClaw uploads the
audio to SenseAudio and uses the transcript in the reply pipeline.

## Options

| Option | Path | Description |
| --- | --- | --- |
| `model` | `tools.media.audio.models[].model` | SenseAudio ASR model id |
| `language` | `tools.media.audio.models[].language` | Optional language hint |
| `prompt` | `tools.media.audio.prompt` | Optional transcription prompt |
| `baseUrl` | `tools.media.audio.baseUrl` or model | Override the OpenAI-compatible base |
| `headers` | `tools.media.audio.request.headers` | Extra request headers |

SenseAudio is batch STT only in OpenClaw. Voice Call realtime transcription
continues to use providers with streaming STT support.

[Runway](https://docs.openclaw.ai/providers/runway) [SGLang](https://docs.openclaw.ai/providers/sglang)

Ctrl+I


---

## SGLang - OpenClaw

_Source: <https://docs.openclaw.ai/providers/sglang>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

SGLang

SGLang can serve open-source models via an **OpenAI-compatible** HTTP API.
OpenClaw can connect to SGLang using the `openai-completions` API.OpenClaw can also **auto-discover** available models from SGLang when you opt
in with `SGLANG_API_KEY` (any value works if your server does not enforce auth)
and you do not define an explicit `models.providers.sglang` entry.OpenClaw treats `sglang` as a local OpenAI-compatible provider that supports
streamed usage accounting, so status/context token counts can update from
`stream_options.include_usage` responses.

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/sglang#)

Start SGLang

Launch SGLang with an OpenAI-compatible server. Your base URL should expose
`/v1` endpoints (for example `/v1/models`, `/v1/chat/completions`). SGLang
commonly runs on:

- `http://127.0.0.1:30000/v1`

2

[Navigate to header](https://docs.openclaw.ai/providers/sglang#)

Set an API key

Any value works if no auth is configured on your server:

```
export SGLANG_API_KEY="sglang-local"
```

3

[Navigate to header](https://docs.openclaw.ai/providers/sglang#)

Run onboarding or set a model directly

```
openclaw onboard
```

Or configure the model manually:

```
{
  agents: {
    defaults: {
      model: { primary: "sglang/your-model-id" },
    },
  },
}
```

## Model discovery (implicit provider)

When `SGLANG_API_KEY` is set (or an auth profile exists) and you **do not**
define `models.providers.sglang`, OpenClaw will query:

- `GET http://127.0.0.1:30000/v1/models`

and convert the returned IDs into model entries.

If you set `models.providers.sglang` explicitly, auto-discovery is skipped and
you must define models manually.

## Explicit configuration (manual models)

Use explicit config when:

- SGLang runs on a different host/port.
- You want to pin `contextWindow`/`maxTokens` values.
- Your server requires a real API key (or you want to control headers).

```
{
  models: {
    providers: {
      sglang: {
        baseUrl: "http://127.0.0.1:30000/v1",
        apiKey: "${SGLANG_API_KEY}",
        api: "openai-completions",
        models: [\
          {\
            id: "your-model-id",\
            name: "Local SGLang Model",\
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

## Advanced configuration

Proxy-style behavior

SGLang is treated as a proxy-style OpenAI-compatible `/v1` backend, not a
native OpenAI endpoint.

| Behavior | SGLang |
| --- | --- |
| OpenAI-only request shaping | Not applied |
| `service_tier`, Responses `store`, prompt-cache hints | Not sent |
| Reasoning-compat payload shaping | Not applied |
| Hidden attribution headers (`originator`, `version`, `User-Agent`) | Not injected on custom SGLang base URLs |

Troubleshooting

**Server not reachable**Verify the server is running and responding:

```
curl http://127.0.0.1:30000/v1/models
```

**Auth errors**If requests fail with auth errors, set a real `SGLANG_API_KEY` that matches
your server configuration, or configure the provider explicitly under
`models.providers.sglang`.

If you run SGLang without authentication, any non-empty value for
`SGLANG_API_KEY` is sufficient to opt in to model discovery.

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Configuration reference** \\
\\
Full config schema including provider entries.](https://docs.openclaw.ai/gateway/configuration-reference)

[SenseAudio](https://docs.openclaw.ai/providers/senseaudio) [StepFun](https://docs.openclaw.ai/providers/stepfun)

Ctrl+I


---

## Synthetic - OpenClaw

_Source: <https://docs.openclaw.ai/providers/synthetic>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Synthetic

[Synthetic](https://synthetic.new/) exposes Anthropic-compatible endpoints.
OpenClaw registers it as the `synthetic` provider and uses the Anthropic
Messages API.

| Property | Value |
| --- | --- |
| Provider | `synthetic` |
| Auth | `SYNTHETIC_API_KEY` |
| API | Anthropic Messages |
| Base URL | `https://api.synthetic.new/anthropic` |

## Getting started

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

## Config example

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

## Built-in catalog

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

If Synthetic changes its API endpoint, overri

_… [truncated; see https://docs.openclaw.ai/providers/synthetic for full content]_


---

## Together AI - OpenClaw

_Source: <https://docs.openclaw.ai/providers/together>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Together AI

[Together AI](https://together.ai/) provides access to leading open-source
models including Llama, DeepSeek, Kimi, and more through a unified API.

| Property | Value |
| --- | --- |
| Provider | `together` |
| Auth | `TOGETHER_API_KEY` |
| API | OpenAI-compatible |
| Base URL | `https://api.together.xyz/v1` |

## Getting started

1

[Navigate to header](https://docs.openclaw.ai/providers/together#)

Get an API key

Create an API key at
[api.together.ai/settings/api-keys](https://api.together.ai/settings/api-keys).

2

[Navigate to header](https://docs.openclaw.ai/providers/together#)

Run onboarding

```
openclaw onboard --auth-choice together-api-key
```

3

[Navigate to header](https://docs.openclaw.ai/providers/together#)

Set a default model

```
{
  agents: {
    defaults: {
      model: { primary: "together/moonshotai/Kimi-K2.5" },
    },
  },
}
```

### Non-interactive example

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice together-api-key \
  --together-api-key "$TOGETHER_API_KEY"
```

The onboarding preset sets `together/moonshotai/Kimi-K2.5` as the default
model.

## Built-in catalog

OpenClaw ships this bundled Together catalog:

| Model ref | Name | Input | Context | Notes |
| --- | --- | --- | --- | --- |
| `together/moonshotai/Kimi-K2.5` | Kimi K2.5 | text, image | 262,144 | Default model; reasoning enabled |
| `together/zai-org/GLM-4.7` | GLM 4.7 Fp8 | text | 202,752 | General-purpose text model |
| `together/meta-llama/Llama-3.3-70B-Instruct-Turbo` | Llama 3.3 70B Instruct Turbo | text | 131,072 | Fast instruction model |
| `together/meta-llama/Llama-4-Scout-17B-16E-Instruct` | Llama 4 Scout 17B 16E Instruct | text, image | 10,000,000 | Multimodal |
| `together/meta-llama/Llama-4-Maverick-17B-128E-Instruct-FP8` | Llama 4 Maverick 17B 128E Instruct FP8 | text, image | 20,000,000 | Multimodal |
| `together/deepseek-ai/DeepSeek-V3.1` | DeepSeek V3.1 | text | 131,072 | General text model |
| `together/deepseek-ai/DeepSeek-R1` | DeepSeek R1 | text | 131,072 | Reasoning model |
| `together/moonshotai/Kimi-K2-Instruct-0905` | Kimi K2-Instruct 0905 | text | 262,144 | Secondary Kimi text model |

## Video generation

The bundled `together` plugin also registers video generation through the
shared `video_generate` tool.

| Property | Value |
| --- | --- |
| Default video model | `together/Wan-AI/Wan2.2-T2V-A14B` |
| Modes | text-to-video, single-image reference |
| Supported parameters | `aspectRatio`, `resolution` |

To use Together as the default video provider:

```
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "together/Wan-AI/Wan2.2-T2V-A14B",
      },
    },
  },
}
```

See [Video Generation](https://docs.openclaw.ai/tools/video-generation) for the shared tool parameters,
provider selection, and failover behavior.

Environment note

If the Gateway runs as a daemon (launchd/systemd), make sure
`TOGETHER_API_KEY` is available to that process (for example, in
`~/.openclaw/.env` or via `env.shellEnv`).

Keys set only in your interactive shell are not visible to daemon-managed
gateway processes. Use `~/.openclaw/.env` or `env.shellEnv` config for
persistent availability.

Troubleshooting

- Verify your key works: `openclaw models list --provider together`
- If models are not appearing, confirm the API key is set in the correct
environment for your Gateway process.
- Model refs use the form `together/<model-id>`.

## Related

[**Model selection** \\
\\
Provider rules, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Video generation** \\
\\
Shared video generation tool parameters and provider selection.](https://docs.openclaw.ai/tools/video-generation)

[**Configuration reference** \\
\\
Full config schema including provider settings.](https://docs.openclaw.ai/gateway/configuration-reference)

[**Together AI** \\
\\
Together AI dashboard, API docs,

_… [truncated; see https://docs.openclaw.ai/providers/together for full content]_


---

## Venice AI - OpenClaw

_Source: <https://docs.openclaw.ai/providers/venice>_

# Use the default private model
openclaw agent --model venice/kimi-k2-5 --message "Quick health check"

# Use Claude Opus via Venice (anonymized)
openclaw agent --model venice/claude-opus-4-6 --message "Summarize this task"

# Use uncensored model
openclaw agent --model venice/venice-uncensored --message "Draft options"

# Use vision model with image
openclaw agent --model venice/qwen3-vl-235b-a22b --message "Review attached image"

# Use coding model
openclaw agent --model venice/qwen3-coder-480b-a35b-instruct --message "Refactor this function"
```

## Troubleshooting

API key not recognized

```
echo $VENICE_API_KEY
openclaw models list | grep venice
```

Ensure the key starts with `vapi_`.

Model not available

The Venice model catalog updates dynamically. Run `openclaw models list` to see currently available models. Some models may be temporarily offline.

Connection issues

Venice API is at `https://api.venice.ai/api/v1`. Ensure your network allows HTTPS connections.

More help: [Troubleshooting](https://docs.openclaw.ai/help/troubleshooting) and [FAQ](https://docs.openclaw.ai/help/faq).

## Advanced configuration

Config file example

```
{
  env: { VENICE_API_KEY: "vapi_..." },
  agents: { defaults: { model: { primary: "venice/kimi-k2-5" } } },
  models: {
    mode: "merge",
    providers: {
      venice: {
        baseUrl: "https://api.venice.ai/api/v1",
        apiKey: "${VENICE_API_KEY}",
        api: "openai-completions",
        models: [\
          {\
            id: "kimi-k2-5",\
            name: "Kimi K2.5",\
            reasoning: true,\
            input: ["text", "image"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 256000,\
            maxTokens: 65536,\
          },\
        ],
      },
    },
  },
}
```

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Venice AI** \\
\\
Venice AI homepage and account signup.](https://venice.ai/)

[**API documentation** \\
\\
Venice API reference and developer docs.](https://docs.venice.ai/)

[**Pricing** \\
\\
Current Venice credit rates and plans.](https://venice.ai/pricing)

[Together AI](https://docs.openclaw.ai/providers/together) [Vercel AI gateway](https://docs.openclaw.ai/providers/vercel-ai-gateway)

Ctrl+I


---

## Vercel AI gateway - OpenClaw

_Source: <https://docs.openclaw.ai/providers/vercel-ai-gateway>_

[OpenClaw home page](https://docs.openclaw.ai/)

Providers

Vercel AI gateway

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

## Getting started

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

## Non-interactive example

For scripted or CI setups, pass all values on the command line:

```
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice ai-gateway-api-key \
  --ai-gateway-api-key "$AI_GATEWAY_API_KEY"
```

## Model ID shorthand

OpenClaw accepts Vercel Claude shorthand model refs and normalizes them at
runtime:

| Shorthand input | Normalized model ref |
| --- | --- |
| `vercel-ai-gateway/claude-opus-4.6` | `vercel-ai-gateway/anthropic/claude-opus-4.6` |
| `vercel-ai-gateway/opus-4.6` | `vercel-ai-gateway/anthropic/claude-opus-4-6` |

You can use either the shorthand or the fully qualified model ref in your
configuration. OpenClaw resolves the canonical form automatically.

## Advanced configuration

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

## Related

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[**Troubleshooting** \\
\\
General troubleshooting and FAQ.](https://docs.openclaw.ai/help/troubleshooting)

[Venice AI](https://docs.openclaw.ai/providers/venice) [vLLM](https://docs.openclaw.ai/providers/vllm)

Ctrl+I


---

## Z.AI - OpenClaw

_Source: <https://docs.openclaw.ai/providers/zai>_

# Coding Plan Global (recommended for Coding Plan users)
openclaw onboard --auth-choice zai-coding-global

# Coding Plan CN (China region)
openclaw onboard --auth-choice zai-coding-cn

# General API
openclaw onboard --auth-choice zai-global

# General API CN (China region)
openclaw onboard --auth-choice zai-cn
```

2

[Navigate to header](https://docs.openclaw.ai/providers/zai#)

Set a default model

```
{
  env: { ZAI_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "zai/glm-5.1" } } },
}
```

3

[Navigate to header](https://docs.openclaw.ai/providers/zai#)

Verify the model is listed

```
openclaw models list --all --provider zai
```

## Built-in catalog

OpenClaw ships the bundled `zai` provider catalog in the plugin manifest, so read-only
listing can show known GLM rows without loading provider runtime:

```
openclaw models list --all --provider zai
```

The manifest-backed catalog currently includes:

| Model ref | Notes |
| --- | --- |
| `zai/glm-5.1` | Default model |
| `zai/glm-5` |  |
| `zai/glm-5-turbo` |  |
| `zai/glm-5v-turbo` |  |
| `zai/glm-4.7` |  |
| `zai/glm-4.7-flash` |  |
| `zai/glm-4.7-flashx` |  |
| `zai/glm-4.6` |  |
| `zai/glm-4.6v` |  |
| `zai/glm-4.5` |  |
| `zai/glm-4.5-air` |  |
| `zai/glm-4.5-flash` |  |
| `zai/glm-4.5v` |  |

GLM models are available as `zai/<model>` (example: `zai/glm-5`). The default bundled model ref is `zai/glm-5.1`.

## Advanced configuration

Forward-resolving unknown GLM-5 models

Unknown `glm-5*` ids still forward-resolve on the bundled provider path by
synthesizing provider-owned metadata from the `glm-4.7` template when the id
matches the current GLM-5 family shape.

Tool-call streaming

`tool_stream` is enabled by default for Z.AI tool-call streaming. To disable it:

```
{
  agents: {
    defaults: {
      models: {
        "zai/<model>": {
          params: { tool_stream: false },
        },
      },
    },
  },
}
```

Thinking and preserved thinking

Z.AI thinking follows OpenClaw’s `/think` controls. With thinking off,
OpenClaw sends `thinking: { type: "disabled" }` to avoid responses that
spend the output budget on `reasoning_content` before visible text.Preserved thinking is opt-in because Z.AI requires the full historical
`reasoning_content` to be replayed, which increases prompt tokens. Enable it
per model:

```
{
  agents: {
    defaults: {
      models: {
        "zai/glm-5.1": {
          params: { preserveThinking: true },
        },
      },
    },
  },
}
```

When enabled and thinking is on, OpenClaw sends
`thinking: { type: "enabled", clear_thinking: false }` and replays prior
`reasoning_content` for the same OpenAI-compatible transcript.Advanced users can still override the exact provider payload with
`params.extra_body.thinking`.

Image understanding

The bundled Z.AI plugin registers image understanding.

| Property | Value |
| --- | --- |
| Model | `glm-4.6v` |

Image understanding is auto-resolved from the configured Z.AI auth — no
additional config is needed.

Auth details

- Z.AI uses Bearer auth with your API key.
- The `zai-api-key` onboarding choice auto-detects the matching Z.AI endpoint from the key prefix.
- Use the explicit regional choices (`zai-coding-global`, `zai-coding-cn`, `zai-global`, `zai-cn`) when you want to force a specific API surface.

## Related

[**GLM model family** \\
\\
Model family overview for GLM.](https://docs.openclaw.ai/providers/glm)

[**Model selection** \\
\\
Choosing providers, model refs, and failover behavior.](https://docs.openclaw.ai/concepts/model-providers)

[Xiaomi MiMo](https://docs.openclaw.ai/providers/xiaomi)

Ctrl+I


---

_4 additional pages omitted to keep this file ≤ 146 KB. See https://docs.openclaw.ai for full content._
