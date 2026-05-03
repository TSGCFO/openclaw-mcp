# Openclaw-Mcp_Docs - Providers Models

**Pages:** 55

---

## Local models

**URL:** https://docs.openclaw.ai/gateway/local-models

**Contents:**
- Local models
- Documentation Index
- ​Hardware floor
- ​Pick a backend
- ​Recommended: LM Studio + large local model (Responses API)
  - ​Hybrid config: hosted primary, local fallback
  - ​Local-first with hosted safety net
  - ​Regional hosting / data routing
- ​Other OpenAI-compatible local proxies
- ​Smaller or stricter backends

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
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
        models: [
          {
            id: "my-local-model",
            name: "Local Model",
            reasoning: false,
            input: ["text"],
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },
            contextWindow: 196608,
            maxTokens: 8192,
          },
        ],
      },
    },
  },
}
```

Example 2 (json):
```json
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
        models: [
          {
            id: "my-local-model",
            name: "Local Model",
            reasoning: false,
            input: ["text"],
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },
            contextWindow: 196608,
            maxTokens: 8192,
          },
        ],
      },
    },
  },
}
```

Example 3 (json):
```json
{
  agents: {
    defaults: {
      model: { primary: "local/my-local-model" },
    },
  },
  models: {
    mode: "merge",
    providers: {
      local: {
        baseUrl: "http://127.0.0.1:8000/v1",
        apiKey: "sk-local",
        api: "openai-completions",
        timeoutSeconds: 300,
        models: [
          {
            id: "my-local-model",
            name: "Local Model",
            reasoning: false,
            input: ["text"],
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },
            contextWindow: 120000,
            maxTokens: 8192,
          },
        ],
      },
    },
  },
}
```

Example 4 (json):
```json
{
  agents: {
    defaults: {
      models: {
        "local/my-local-model": {
          params: {
            extra_body: {
              tool_choice: "required",
            },
          },
        },
      },
    },
  },
}
```

---

## DeepSeek

**URL:** https://docs.openclaw.ai/providers/deepseek

**Contents:**
- DeepSeek
- Documentation Index
- ​Getting started
- ​Built-in catalog
- ​Thinking and tools
- ​Live testing
- ​Config example
- ​Related
- Model selection
- Configuration reference

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify models are available

Non-interactive setup

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice deepseek-api-key
```

Example 2 (unknown):
```unknown
openclaw models list --provider deepseek
```

Example 3 (unknown):
```unknown
openclaw models list --all --provider deepseek
```

Example 4 (bash):
```bash
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice deepseek-api-key \
  --deepseek-api-key "$DEEPSEEK_API_KEY" \
  --skip-health \
  --accept-risk
```

---

## Mistral

**URL:** https://docs.openclaw.ai/providers/mistral

**Contents:**
- Mistral
- Documentation Index
- ​Getting started
- ​Built-in LLM catalog
- ​Audio transcription (Voxtral)
- ​Voice Call streaming STT
- ​Advanced configuration
- ​Related
- Model selection
- Media understanding

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify the model is available

Adjustable reasoning (mistral-small-latest)

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice mistral-api-key
```

Example 2 (bash):
```bash
openclaw onboard --mistral-api-key "$MISTRAL_API_KEY"
```

Example 3 (lua):
```lua
{
  env: { MISTRAL_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "mistral/mistral-large-latest" } } },
}
```

Example 4 (unknown):
```unknown
openclaw models list --provider mistral
```

---

## OpenCode Go

**URL:** https://docs.openclaw.ai/providers/opencode-go

**Contents:**
- OpenCode Go
- Documentation Index
- ​Built-in catalog
- ​Getting started
- ​Config example
- ​Advanced configuration
- ​Related
- OpenCode (parent)
- Model selection

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Set a Go model as default

Verify models are available

Pass the key directly

Verify models are available

Runtime ref convention

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice opencode-go
```

Example 2 (unknown):
```unknown
openclaw config set agents.defaults.model.primary "opencode-go/kimi-k2.6"
```

Example 3 (unknown):
```unknown
openclaw models list --provider opencode-go
```

Example 4 (bash):
```bash
openclaw onboard --opencode-go-api-key "$OPENCODE_API_KEY"
```

---

## FAQ: models and auth

**URL:** https://docs.openclaw.ai/help/faq-models

**Contents:**
- FAQ: models and auth
- Documentation Index
- ​Models: defaults, selection, aliases, switching
- ​Model failover and “All models failed”
- ​Auth profiles: what they are and how to manage them
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

What is the "default model"?

What model do you recommend?

How do I switch models without wiping my config?

Can I use self-hosted models (llama.cpp, vLLM, Ollama)?

What do OpenClaw, Flawd, and Krill use for models?

How do I switch models on the fly (without restarting)?

Can I use GPT 5.5 for daily tasks and Codex 5.5 for coding?

How do I configure fast mode for GPT 5.5?

Why do I see "Model ... is not allowed" and then no reply?

Why do I see "Unknown model: minimax/MiniMax-M2.7"?

Can I use MiniMax as my default and OpenAI for complex tasks?

Are opus / sonnet / gpt built-in shortcuts?

How do I define/override model shortcuts (aliases)?

How do I add models from other providers like OpenRouter or Z.AI?

How does failover work?

What does "No credentials found for profile anthropic:default" mean?

Why did it also try Google Gemini and fail?

What is an auth profile?

What are typical profile IDs?

Can I control which auth profile is tried first?

OAuth vs API key - what is the difference?

**Examples:**

Example 1 (unknown):
```unknown
agents.defaults.model.primary
```

Example 2 (unknown):
```unknown
/model sonnet
/model opus
/model gpt
/model gpt-mini
/model gemini
/model gemini-flash
/model gemini-flash-lite
```

Example 3 (elixir):
```elixir
/model opus@anthropic:default
/model opus@anthropic:work
```

Example 4 (unknown):
```unknown
/model anthropic/claude-opus-4-6
```

---

## Fal

**URL:** https://docs.openclaw.ai/providers/fal

**Contents:**
- Fal
- Documentation Index
- ​Getting started
- ​Image generation
- ​Video generation
- ​Related
- Image generation
- Video generation
- Configuration reference

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Set a default image model

Available video models

Seedance 2.0 config example

Seedance 2.0 reference-to-video config example

HeyGen video-agent config example

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice fal-api-key
```

Example 2 (json):
```json
{
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "fal/fal-ai/flux/dev",
      },
    },
  },
}
```

Example 3 (json):
```json
{
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "fal/fal-ai/flux/dev",
      },
    },
  },
}
```

Example 4 (json):
```json
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "fal/bytedance/seedance-2.0/fast/text-to-video",
      },
    },
  },
}
```

---

## Amazon Bedrock

**URL:** https://docs.openclaw.ai/providers/bedrock

**Contents:**
- Amazon Bedrock
- Documentation Index
- ​Getting started
- ​Automatic model discovery
- ​Quick setup (AWS path)
- ​Advanced configuration
- ​Related
- Model selection
- Memory search
- Memory config reference

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Set AWS credentials on the gateway host

Add a Bedrock provider and model to your config

Verify models are available

Enable discovery explicitly

Optionally add an env marker for auto mode

Verify models are discovered

Discovery config options

Claude Opus 4.7 temperature

Embeddings for memory search

**Examples:**

Example 1 (lua):
```lua
export AWS_ACCESS_KEY_ID="AKIA..."
export AWS_SECRET_ACCESS_KEY="..."
export AWS_REGION="us-east-1"
# Optional:
export AWS_SESSION_TOKEN="..."
export AWS_PROFILE="your-profile"
# Optional (Bedrock API key/bearer token):
export AWS_BEARER_TOKEN_BEDROCK="..."
```

Example 2 (json):
```json
{
  models: {
    providers: {
      "amazon-bedrock": {
        baseUrl: "https://bedrock-runtime.us-east-1.amazonaws.com",
        api: "bedrock-converse-stream",
        auth: "aws-sdk",
        models: [
          {
            id: "us.anthropic.claude-opus-4-6-v1:0",
            name: "Claude Opus 4.6 (Bedrock)",
            reasoning: true,
            input: ["text", "image"],
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },
            contextWindow: 200000,
            maxTokens: 8192,
          },
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

Example 3 (unknown):
```unknown
openclaw models list
```

Example 4 (unknown):
```unknown
openclaw config set plugins.entries.amazon-bedrock.config.discovery.enabled true
openclaw config set plugins.entries.amazon-bedrock.config.discovery.region us-east-1
```

---

## Formal verification (security models)

**URL:** https://docs.openclaw.ai/security/formal-verification

**Contents:**
- Formal verification (security models)
- Documentation Index
- ​Where the models live
- ​Important caveats
- ​Reproducing results
  - ​Gateway exposure and open gateway misconfiguration
  - ​Node exec pipeline (highest-risk capability)
  - ​Pairing store (DM gating)
  - ​Ingress gating (mentions + control-command bypass)
  - ​Routing/session-key isolation

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (markdown):
```markdown
git clone https://github.com/vignesh07/openclaw-formal-models
cd openclaw-formal-models

# Java 11+ required (TLC runs on the JVM).
# The repo vendors a pinned `tla2tools.jar` (TLA+ tools) and provides `bin/tlc` + Make targets.

make <target>
```

---

## Runway

**URL:** https://docs.openclaw.ai/providers/runway

**Contents:**
- Runway
- Documentation Index
- ​Getting started
- ​Supported modes
- ​Configuration
- ​Advanced configuration
- ​Related
- Video generation
- Configuration reference

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Set Runway as the default video provider

Environment variable aliases

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice runway-api-key
```

Example 2 (unknown):
```unknown
openclaw config set agents.defaults.videoGenerationModel.primary "runway/gen4.5"
```

Example 3 (json):
```json
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "runway/gen4.5",
      },
    },
  },
}
```

---

## Venice AI

**URL:** https://docs.openclaw.ai/providers/venice

**Contents:**
- Venice AI
- Documentation Index
- ​Why Venice in OpenClaw
- ​Privacy modes
- ​Features
- ​Getting started
- ​Model selection
- ​DeepSeek V4 replay behavior
- ​Built-in catalog (41 total)
- ​Model discovery

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Private models (26) — fully private, no logging

Anonymized models (15) — via Venice proxy

API key not recognized

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice venice-api-key
```

Example 2 (unknown):
```unknown
export VENICE_API_KEY="vapi_xxxxxxxxxxxx"
```

Example 3 (unknown):
```unknown
openclaw onboard --non-interactive \
  --auth-choice venice-api-key \
  --venice-api-key "vapi_xxxxxxxxxxxx"
```

Example 4 (unknown):
```unknown
openclaw agent --model venice/kimi-k2-5 --message "Hello, are you working?"
```

---

## Moonshot AI

**URL:** https://docs.openclaw.ai/providers/moonshot

**Contents:**
- Moonshot AI
- Documentation Index
- ​Built-in model catalog
- ​Getting started
  - ​Config example
  - ​Config example
- ​Kimi web search
- ​Advanced configuration
- ​Related
- Model selection

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Choose your endpoint region

Verify models are available

Run a live smoke test

Verify the model is available

Run interactive web search setup

Configure the web search region and model

Tool call id sanitization

Streaming usage compatibility

Endpoint and model ref reference

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice moonshot-api-key
```

Example 2 (unknown):
```unknown
openclaw onboard --auth-choice moonshot-api-key-cn
```

Example 3 (json):
```json
{
  agents: {
    defaults: {
      model: { primary: "moonshot/kimi-k2.6" },
    },
  },
}
```

Example 4 (unknown):
```unknown
openclaw models list --provider moonshot
```

---

## SGLang

**URL:** https://docs.openclaw.ai/providers/sglang

**Contents:**
- SGLang
- Documentation Index
- ​Getting started
- ​Model discovery (implicit provider)
- ​Explicit configuration (manual models)
- ​Advanced configuration
- ​Related
- Model selection
- Configuration reference

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Run onboarding or set a model directly

**Examples:**

Example 1 (unknown):
```unknown
export SGLANG_API_KEY="sglang-local"
```

Example 2 (unknown):
```unknown
openclaw onboard
```

Example 3 (json):
```json
{
  agents: {
    defaults: {
      model: { primary: "sglang/your-model-id" },
    },
  },
}
```

Example 4 (json):
```json
{
  models: {
    providers: {
      sglang: {
        baseUrl: "http://127.0.0.1:30000/v1",
        apiKey: "${SGLANG_API_KEY}",
        api: "openai-completions",
        models: [
          {
            id: "your-model-id",
            name: "Local SGLang Model",
            reasoning: false,
            input: ["text"],
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },
            contextWindow: 128000,
            maxTokens: 8192,
          },
        ],
      },
    },
  },
}
```

---

## Qianfan

**URL:** https://docs.openclaw.ai/providers/qianfan

**Contents:**
- Qianfan
- Documentation Index
- ​Getting started
- ​Built-in catalog
- ​Config example
- ​Related
- Model selection
- Configuration reference
- Agent setup
- Qianfan API docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Create a Baidu Cloud account

Verify the model is available

Transport and compatibility

Catalog and overrides

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice qianfan-api-key
```

Example 2 (unknown):
```unknown
openclaw models list --provider qianfan
```

Example 3 (lua):
```lua
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
        models: [
          {
            id: "deepseek-v3.2",
            name: "DEEPSEEK V3.2",
            reasoning: true,
            input: ["text"],
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },
            contextWindow: 98304,
            maxTokens: 32768,
          },
          {
            id: "ernie-5.0-thinking-preview",
            name: "ERNIE-5.0-Thinking-Preview",
            reasoning: true,
            input: ["text", "image"],
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },
            contextWindow: 119000,
            maxTokens: 64000,
          },
        ],
      },
    },
  },
}
```

---

## Hugging Face (inference)

**URL:** https://docs.openclaw.ai/providers/huggingface

**Contents:**
- Hugging Face (inference)
- Documentation Index
- ​Getting started
  - ​Non-interactive setup
- ​Model IDs
- ​Advanced configuration
- ​Related
- Model selection
- Model selection
- Inference Providers docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Create a fine-grained token

Select a default model

Verify the model is available

Model discovery and onboarding dropdown

Model names, aliases, and policy suffixes

Environment and daemon setup

Config: DeepSeek R1 with Qwen fallback

Config: Qwen with cheapest and fastest variants

Config: DeepSeek + Llama + GPT-OSS with aliases

Config: Multiple Qwen and DeepSeek with policy suffixes

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice huggingface-api-key
```

Example 2 (json):
```json
{
  agents: {
    defaults: {
      model: { primary: "huggingface/deepseek-ai/DeepSeek-R1" },
    },
  },
}
```

Example 3 (unknown):
```unknown
openclaw models list --provider huggingface
```

Example 4 (bash):
```bash
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice huggingface-api-key \
  --huggingface-api-key "$HF_TOKEN"
```

---

## Models

**URL:** https://docs.openclaw.ai/cli/models

**Contents:**
- Models
- Documentation Index
- ​openclaw models
- ​Common commands
  - ​Models scan
  - ​Models status
- ​Aliases + fallbacks
- ​Auth profiles
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw models status
openclaw models list
openclaw models set <model-or-alias>
openclaw models scan
```

Example 2 (unknown):
```unknown
openclaw models aliases list
openclaw models fallbacks list
```

Example 3 (typescript):
```typescript
openclaw models auth add
openclaw models auth login --provider <id>
openclaw models auth setup-token --provider <id>
openclaw models auth paste-token
```

Example 4 (powershell):
```powershell
openclaw models auth login --provider openai-codex --set-default
```

---

## LM Studio

**URL:** https://docs.openclaw.ai/providers/lmstudio

**Contents:**
- LM Studio
- Documentation Index
- ​Quick start
- ​Non-interactive onboarding
- ​Configuration
  - ​Streaming usage compatibility
  - ​Thinking compatibility
  - ​Explicit configuration
- ​Troubleshooting
  - ​LM Studio not detected

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
curl -fsSL https://lmstudio.ai/install.sh | bash
```

Example 2 (unknown):
```unknown
lms daemon up
```

Example 3 (unknown):
```unknown
lms server start --port 1234
```

Example 4 (unknown):
```unknown
export LM_API_TOKEN="your-lm-studio-api-token"
```

---

## Vydra

**URL:** https://docs.openclaw.ai/providers/vydra

**Contents:**
- Vydra
- Documentation Index
- ​Setup
- ​Capabilities
- ​Related
- Provider directory
- Image generation
- Video generation
- Configuration reference

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Run interactive onboarding

Choose a default capability

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice vydra-api-key
```

Example 2 (lua):
```lua
export VYDRA_API_KEY="vydra_live_..."
```

Example 3 (json):
```json
{
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "vydra/grok-imagine",
      },
    },
  },
}
```

Example 4 (json):
```json
{
  agents: {
    defaults: {
      videoGenerationModel: {
        primary: "vydra/veo3",
      },
    },
  },
}
```

---

## Vercel AI gateway

**URL:** https://docs.openclaw.ai/providers/vercel-ai-gateway

**Contents:**
- Vercel AI gateway
- Documentation Index
- ​Getting started
- ​Non-interactive example
- ​Model ID shorthand
- ​Advanced configuration
- ​Related
- Model selection
- Troubleshooting

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify the model is available

Environment variable for daemon processes

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice ai-gateway-api-key
```

Example 2 (json):
```json
{
  agents: {
    defaults: {
      model: { primary: "vercel-ai-gateway/anthropic/claude-opus-4.6" },
    },
  },
}
```

Example 3 (unknown):
```unknown
openclaw models list --provider vercel-ai-gateway
```

Example 4 (bash):
```bash
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice ai-gateway-api-key \
  --ai-gateway-api-key "$AI_GATEWAY_API_KEY"
```

---

## Chutes

**URL:** https://docs.openclaw.ai/providers/chutes

**Contents:**
- Chutes
- Documentation Index
- ​Getting started
- ​Discovery behavior
- ​Default aliases
- ​Built-in starter catalog
- ​Config example
- ​Related
- Model selection
- Configuration reference

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Run the OAuth onboarding flow

Verify the default model

Run the API key onboarding flow

Verify the default model

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice chutes
```

Example 2 (unknown):
```unknown
openclaw onboard --auth-choice chutes-api-key
```

Example 3 (json):
```json
{
  agents: {
    defaults: {
      model: { primary: "chutes/zai-org/GLM-4.7-TEE" },
      models: {
        "chutes/zai-org/GLM-4.7-TEE": { alias: "Chutes GLM 4.7" },
        "chutes/deepseek-ai/DeepSeek-V3.2-TEE": { alias: "Chutes DeepSeek V3.2" },
      },
    },
  },
}
```

---

## Amazon Bedrock Mantle

**URL:** https://docs.openclaw.ai/providers/bedrock-mantle

**Contents:**
- Amazon Bedrock Mantle
- Documentation Index
- ​Getting started
- ​Automatic model discovery
  - ​Supported regions
- ​Manual configuration
- ​Advanced configuration
- ​Related
- Amazon Bedrock
- Model selection

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Set the bearer token on the gateway host

Verify models are discovered

Configure AWS credentials on the gateway host

Verify models are discovered

Endpoint unavailability

Claude Opus 4.7 via the Anthropic Messages route

Relationship to Amazon Bedrock provider

**Examples:**

Example 1 (lua):
```lua
export AWS_BEARER_TOKEN_BEDROCK="..."
```

Example 2 (unknown):
```unknown
export AWS_REGION="us-west-2"
```

Example 3 (unknown):
```unknown
openclaw models list
```

Example 4 (unknown):
```unknown
export AWS_PROFILE="default"
export AWS_REGION="us-west-2"
```

---

## Volcengine (Doubao)

**URL:** https://docs.openclaw.ai/providers/volcengine

**Contents:**
- Volcengine (Doubao)
- Documentation Index
- ​Getting started
- ​Providers and endpoints
- ​Built-in catalog
- ​Text-to-speech
- ​Advanced configuration
- ​Related
- Model selection
- Configuration

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify the model is available

Default model after onboarding

Model picker fallback behavior

Environment variables for daemon processes

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice volcengine-api-key
```

Example 2 (json):
```json
{
  agents: {
    defaults: {
      model: { primary: "volcengine-plan/ark-code-latest" },
    },
  },
}
```

Example 3 (unknown):
```unknown
openclaw models list --provider volcengine
openclaw models list --provider volcengine-plan
```

Example 4 (bash):
```bash
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice volcengine-api-key \
  --volcengine-api-key "$VOLCANO_ENGINE_API_KEY"
```

---

## NVIDIA

**URL:** https://docs.openclaw.ai/providers/nvidia

**Contents:**
- NVIDIA
- Documentation Index
- ​Getting started
- ​Config example
- ​Built-in catalog
- ​Advanced configuration
- ​Related
- Model selection
- Configuration reference

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Export the key and run onboarding

OpenAI-compatible endpoint

**Examples:**

Example 1 (lua):
```lua
export NVIDIA_API_KEY="nvapi-..."
openclaw onboard --auth-choice nvidia-api-key
```

Example 2 (unknown):
```unknown
openclaw models set nvidia/nvidia/nemotron-3-super-120b-a12b
```

Example 3 (lua):
```lua
openclaw onboard --auth-choice nvidia-api-key --nvidia-api-key "nvapi-..."
```

Example 4 (lua):
```lua
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

---

## Google (Gemini)

**URL:** https://docs.openclaw.ai/providers/google

**Contents:**
- Google (Gemini)
- Documentation Index
- ​Getting started
- ​Capabilities
- ​Web search
- ​Image generation
- ​Video generation
- ​Music generation
- ​Text-to-speech
- ​Realtime voice

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify the model is available

Install the Gemini CLI

Verify the model is available

Direct Gemini cache reuse

Gemini CLI JSON usage notes

Environment and daemon setup

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice gemini-api-key
```

Example 2 (bash):
```bash
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice gemini-api-key \
  --gemini-api-key "$GEMINI_API_KEY"
```

Example 3 (json):
```json
{
  agents: {
    defaults: {
      model: { primary: "google/gemini-3.1-pro-preview" },
    },
  },
}
```

Example 4 (unknown):
```unknown
openclaw models list --provider google
```

---

## SenseAudio

**URL:** https://docs.openclaw.ai/providers/senseaudio

**Contents:**
- SenseAudio
- Documentation Index
- ​SenseAudio
- ​Getting Started
- ​Options

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Enable the audio provider

**Examples:**

Example 1 (lua):
```lua
export SENSEAUDIO_API_KEY="..."
```

Example 2 (json):
```json
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

---

## Together AI

**URL:** https://docs.openclaw.ai/providers/together

**Contents:**
- Together AI
- Documentation Index
- ​Getting started
  - ​Non-interactive example
- ​Built-in catalog
- ​Video generation
- ​Related
- Model selection
- Video generation
- Configuration reference

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice together-api-key
```

Example 2 (json):
```json
{
  agents: {
    defaults: {
      model: { primary: "together/moonshotai/Kimi-K2.5" },
    },
  },
}
```

Example 3 (bash):
```bash
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice together-api-key \
  --together-api-key "$TOGETHER_API_KEY"
```

Example 4 (json):
```json
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

---

## OpenCode

**URL:** https://docs.openclaw.ai/providers/opencode

**Contents:**
- OpenCode
- Documentation Index
- ​Getting started
- ​Config example
- ​Built-in catalogs
  - ​Zen
  - ​Go
- ​Advanced configuration
- ​Related
- Model selection

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Set a Zen model as the default

Verify models are available

Set a Go model as the default

Verify models are available

Billing and dashboard

Gemini replay behavior

Non-Gemini replay behavior

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice opencode-zen
```

Example 2 (bash):
```bash
openclaw onboard --opencode-zen-api-key "$OPENCODE_API_KEY"
```

Example 3 (unknown):
```unknown
openclaw config set agents.defaults.model.primary "opencode/claude-opus-4-6"
```

Example 4 (unknown):
```unknown
openclaw models list --provider opencode
```

---

## Alibaba Model Studio

**URL:** https://docs.openclaw.ai/providers/alibaba

**Contents:**
- Alibaba Model Studio
- Documentation Index
- ​Getting started
- ​Built-in Wan models
- ​Current limits
- ​Advanced configuration
- ​Related
- Video generation
- Qwen
- Configuration reference

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Set a default video model

Verify the provider is available

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice qwen-standard-api-key
```

Example 2 (json):
```json
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

Example 3 (unknown):
```unknown
openclaw models list --provider alibaba
```

---

## Fireworks

**URL:** https://docs.openclaw.ai/providers/fireworks

**Contents:**
- Fireworks
- Documentation Index
- ​Getting started
- ​Non-interactive example
- ​Built-in catalog
- ​Custom Fireworks model ids
- ​Related
- Model selection
- Troubleshooting

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Set up Fireworks auth through onboarding

Verify the model is available

How model id prefixing works

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice fireworks-api-key
```

Example 2 (unknown):
```unknown
openclaw models list --provider fireworks
```

Example 3 (bash):
```bash
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice fireworks-api-key \
  --fireworks-api-key "$FIREWORKS_API_KEY" \
  --skip-health \
  --accept-risk
```

Example 4 (json):
```json
{
  agents: {
    defaults: {
      model: {
        primary: "fireworks/accounts/fireworks/routers/kimi-k2p5-turbo",
      },
    },
  },
}
```

---

## Cerebras

**URL:** https://docs.openclaw.ai/providers/cerebras

**Contents:**
- Cerebras
- Documentation Index
- ​Getting Started
  - ​Non-Interactive Setup
- ​Built-In Catalog
- ​Manual Config

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify models are available

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice cerebras-api-key
```

Example 2 (unknown):
```unknown
openclaw models list --provider cerebras
```

Example 3 (bash):
```bash
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice cerebras-api-key \
  --cerebras-api-key "$CEREBRAS_API_KEY"
```

Example 4 (lua):
```lua
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
        models: [
          { id: "zai-glm-4.7", name: "Z.ai GLM 4.7" },
          { id: "gpt-oss-120b", name: "GPT OSS 120B" },
        ],
      },
    },
  },
}
```

---

## LiteLLM

**URL:** https://docs.openclaw.ai/providers/litellm

**Contents:**
- LiteLLM
- Documentation Index
- ​Quick start
- ​Configuration
  - ​Environment variables
  - ​Config file
- ​Advanced configuration
  - ​Image generation
- ​Related
- LiteLLM Docs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Point OpenClaw to LiteLLM

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice litellm-api-key
```

Example 2 (bash):
```bash
openclaw onboard --non-interactive --auth-choice litellm-api-key --litellm-api-key "$LITELLM_API_KEY" --custom-base-url "https://litellm.example/v1"
```

Example 3 (unknown):
```unknown
pip install 'litellm[proxy]'
litellm --model claude-opus-4-6
```

Example 4 (unknown):
```unknown
export LITELLM_API_KEY="your-litellm-key"

openclaw
```

---

## Synthetic

**URL:** https://docs.openclaw.ai/providers/synthetic

**Contents:**
- Synthetic
- Documentation Index
- ​Getting started
- ​Config example
- ​Built-in catalog
- ​Related
- Model selection
- Configuration reference
- Synthetic

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify the default model

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice synthetic-api-key
```

Example 2 (unknown):
```unknown
synthetic/hf:MiniMaxAI/MiniMax-M2.5
```

Example 3 (lua):
```lua
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
        models: [
          {
            id: "hf:MiniMaxAI/MiniMax-M2.5",
            name: "MiniMax M2.5",
            reasoning: false,
            input: ["text"],
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },
            contextWindow: 192000,
            maxTokens: 65536,
          },
        ],
      },
    },
  },
}
```

Example 4 (json):
```json
{
  models: {
    providers: {
      synthetic: {
        baseUrl: "https://new-api.synthetic.new/anthropic",
      },
    },
  },
}
```

---

## Gradium

**URL:** https://docs.openclaw.ai/providers/gradium

**Contents:**
- Gradium
- Documentation Index
- ​Setup
- ​Config
- ​Voices
- ​Output
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
export GRADIUM_API_KEY="gsk_..."
```

Example 2 (json):
```json
{
  messages: {
    tts: {
      auto: "always",
      provider: "gradium",
      providers: {
        gradium: {
          voiceId: "YTpq7expH9539ERJ",
          // apiKey: "${GRADIUM_API_KEY}",
          // baseUrl: "https://api.gradium.ai",
        },
      },
    },
  },
}
```

---

## GLM (Zhipu)

**URL:** https://docs.openclaw.ai/providers/glm

**Contents:**
- GLM (Zhipu)
- Documentation Index
- ​GLM models
- ​Getting started
- ​Config example
- ​Built-in catalog
- ​Advanced configuration
- ​Related
- Z.AI provider
- Model selection

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Choose an auth route and run onboarding

Set GLM as the default model

Verify models are available

Endpoint auto-detection

**Examples:**

Example 1 (markdown):
```markdown
# Example: generic auto-detect
openclaw onboard --auth-choice zai-api-key

# Example: Coding Plan global
openclaw onboard --auth-choice zai-coding-global
```

Example 2 (unknown):
```unknown
openclaw config set agents.defaults.model.primary "zai/glm-5.1"
```

Example 3 (unknown):
```unknown
openclaw models list --provider zai
```

Example 4 (lua):
```lua
{
  env: { ZAI_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "zai/glm-5.1" } } },
}
```

---

## Inworld

**URL:** https://docs.openclaw.ai/providers/inworld

**Contents:**
- Inworld
- Documentation Index
- ​Getting started
- ​Configuration options
- ​Notes
- ​Related
- Text-to-speech
- Configuration
- Providers
- Troubleshooting

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Select Inworld in messages.tts

**Examples:**

Example 1 (unknown):
```unknown
INWORLD_API_KEY=<base64-credential-from-dashboard>
```

Example 2 (json):
```json
{
  messages: {
    tts: {
      auto: "always",
      provider: "inworld",
      providers: {
        inworld: {
          voiceId: "Sarah",
          modelId: "inworld-tts-1.5-max",
        },
      },
    },
  },
}
```

---

## StepFun

**URL:** https://docs.openclaw.ai/providers/stepfun

**Contents:**
- StepFun
- Documentation Index
- ​Region and endpoint overview
- ​Built-in catalog
- ​Getting started
  - ​Model refs
  - ​Model refs
- ​Advanced configuration
- ​Related
- Model selection

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Choose your endpoint region

Non-interactive alternative

Verify models are available

Choose your endpoint region

Non-interactive alternative

Verify models are available

Full config: Standard provider

Full config: Step Plan provider

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice stepfun-standard-api-key-intl
```

Example 2 (unknown):
```unknown
openclaw onboard --auth-choice stepfun-standard-api-key-cn
```

Example 3 (bash):
```bash
openclaw onboard --auth-choice stepfun-standard-api-key-intl \
  --stepfun-api-key "$STEPFUN_API_KEY"
```

Example 4 (unknown):
```unknown
openclaw models list --provider stepfun
```

---

## Ollama

**URL:** https://docs.openclaw.ai/providers/ollama

**Contents:**
- Ollama
- Documentation Index
- ​Auth rules
- ​Getting started
  - ​Non-interactive mode
- ​Cloud models
- ​Model discovery (implicit provider)
- ​Vision and image description
- ​Configuration
- ​Common recipes

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Remote and Ollama Cloud hosts

Memory embedding scope

Verify the model is available

Choose cloud or local

Pull a local model (local only)

Enable Ollama for OpenClaw

Inspect and set your model

Local model with auto-discovery

LAN Ollama host with manual models

Cloud plus local through a signed-in daemon

Multiple Ollama hosts

Lean local model profile

Legacy OpenAI-compatible mode

Streaming configuration

WSL2 crash loop (repeated reboots)

Remote host works with curl but not OpenClaw

Model outputs tool JSON as text

Kimi or GLM returns garbled symbols

Cold local model times out

Large-context model is too slow or runs out of memory

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard
```

Example 2 (unknown):
```unknown
openclaw models list --provider ollama
```

Example 3 (unknown):
```unknown
openclaw onboard --non-interactive \
  --auth-choice ollama \
  --accept-risk
```

Example 4 (json):
```json
openclaw onboard --non-interactive \
  --auth-choice ollama \
  --custom-base-url "http://ollama-host:11434" \
  --custom-model-id "qwen3.5:27b" \
  --accept-risk
```

---

## Kilocode

**URL:** https://docs.openclaw.ai/providers/kilocode

**Contents:**
- Kilocode
- Documentation Index
- ​Kilo Gateway
- ​Getting started
- ​Default model
- ​Built-in catalog
- ​Config example
- ​Related
- Model selection
- Configuration reference

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify the model is available

Transport and compatibility

Stream wrapper and reasoning

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice kilocode-api-key
```

Example 2 (unknown):
```unknown
export KILOCODE_API_KEY="<your-kilocode-api-key>" # pragma: allowlist secret
```

Example 3 (unknown):
```unknown
openclaw models list --provider kilocode
```

Example 4 (json):
```json
{
  env: { KILOCODE_API_KEY: "<your-kilocode-api-key>" }, // pragma: allowlist secret
  agents: {
    defaults: {
      model: { primary: "kilocode/kilo/auto" },
    },
  },
}
```

---

## OpenRouter

**URL:** https://docs.openclaw.ai/providers/openrouter

**Contents:**
- OpenRouter
- Documentation Index
- ​Getting started
- ​Config example
- ​Model references
- ​Image generation
- ​Video generation
- ​Text-to-speech
- ​Authentication and headers
- ​Advanced configuration

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

(Optional) Switch to a specific model

Anthropic cache markers

Anthropic reasoning prefill

Thinking / reasoning injection

DeepSeek V4 reasoning replay

OpenAI-only request shaping

Provider routing metadata

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice openrouter-api-key
```

Example 2 (typescript):
```typescript
openclaw models set openrouter/<provider>/<model>
```

Example 3 (lua):
```lua
{
  env: { OPENROUTER_API_KEY: "sk-or-..." },
  agents: {
    defaults: {
      model: { primary: "openrouter/auto" },
    },
  },
}
```

Example 4 (lua):
```lua
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

---

## Cloudflare AI gateway

**URL:** https://docs.openclaw.ai/providers/cloudflare-ai-gateway

**Contents:**
- Cloudflare AI gateway
- Documentation Index
- ​Getting started
- ​Non-interactive example
- ​Advanced configuration
- ​Related
- Model selection
- Troubleshooting

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Set the provider API key and Gateway details

Verify the model is available

Authenticated gateways

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice cloudflare-ai-gateway-api-key
```

Example 2 (json):
```json
{
  agents: {
    defaults: {
      model: { primary: "cloudflare-ai-gateway/claude-sonnet-4-6" },
    },
  },
}
```

Example 3 (unknown):
```unknown
openclaw models list --provider cloudflare-ai-gateway
```

Example 4 (bash):
```bash
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice cloudflare-ai-gateway-api-key \
  --cloudflare-ai-gateway-account-id "your-account-id" \
  --cloudflare-ai-gateway-gateway-id "your-gateway-id" \
  --cloudflare-ai-gateway-api-key "$CLOUDFLARE_AI_GATEWAY_API_KEY"
```

---

## Deepgram

**URL:** https://docs.openclaw.ai/providers/deepgram

**Contents:**
- Deepgram
- Documentation Index
- ​Getting started
- ​Configuration options
- ​Voice Call streaming STT
- ​Notes
- ​Related
- Media tools
- Configuration
- Troubleshooting

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Enable the audio provider

Proxy and custom endpoints

**Examples:**

Example 1 (lua):
```lua
DEEPGRAM_API_KEY=dg_...
```

Example 2 (json):
```json
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

Example 3 (json):
```json
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

Example 4 (json):
```json
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

---

## Inferrs

**URL:** https://docs.openclaw.ai/providers/inferrs

**Contents:**
- Inferrs
- Documentation Index
- ​Getting started
- ​Full config example
- ​Advanced configuration
- ​Troubleshooting
- ​Related
- Local models
- Gateway troubleshooting
- Model selection

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Start inferrs with a model

Verify the server is reachable

Add an OpenClaw provider entry

Why requiresStringContent matters

Gemma and tool-schema caveat

curl /v1/models fails

messages[].content expected a string

Direct /v1/chat/completions calls pass but openclaw infer model run fails

inferrs still crashes on larger agent turns

**Examples:**

Example 1 (unknown):
```unknown
inferrs serve google/gemma-4-E2B-it \
  --host 127.0.0.1 \
  --port 8080 \
  --device metal
```

Example 2 (json):
```json
curl http://127.0.0.1:8080/health
curl http://127.0.0.1:8080/v1/models
```

Example 3 (json):
```json
{
  agents: {
    defaults: {
      model: { primary: "inferrs/google/gemma-4-E2B-it" },
      models: {
        "inferrs/google/gemma-4-E2B-it": {
          alias: "Gemma 4 (inferrs)",
        },
      },
    },
  },
  models: {
    mode: "merge",
    providers: {
      inferrs: {
        baseUrl: "http://127.0.0.1:8080/v1",
        apiKey: "inferrs-local",
        api: "openai-completions",
        models: [
          {
            id: "google/gemma-4-E2B-it",
            name: "Gemma 4 E2B (inferrs)",
            reasoning: false,
            input: ["text"],
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },
            contextWindow: 131072,
            maxTokens: 4096,
            compat: {
              requiresStringContent: true,
            },
          },
        ],
      },
    },
  },
}
```

Example 4 (unknown):
```unknown
messages[1].content: invalid type: sequence, expected a string
```

---

## ElevenLabs

**URL:** https://docs.openclaw.ai/providers/elevenlabs

**Contents:**
- ElevenLabs
- Documentation Index
- ​Authentication
- ​Text-to-speech
- ​Speech-to-text
- ​Voice Call streaming STT
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
export ELEVENLABS_API_KEY="..."
```

Example 2 (json):
```json
{
  messages: {
    tts: {
      providers: {
        elevenlabs: {
          apiKey: "${ELEVENLABS_API_KEY}",
          voiceId: "pMsXgVXv3BLzUgSXRplE",
          modelId: "eleven_multilingual_v2",
        },
      },
    },
  },
}
```

Example 3 (json):
```json
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "elevenlabs", model: "scribe_v2" }],
      },
    },
  },
}
```

Example 4 (json):
```json
{
  plugins: {
    entries: {
      "voice-call": {
        config: {
          streaming: {
            enabled: true,
            provider: "elevenlabs",
            providers: {
              elevenlabs: {
                apiKey: "${ELEVENLABS_API_KEY}",
                audioFormat: "ulaw_8000",
                commitStrategy: "vad",
                languageCode: "en",
              },
            },
          },
        },
      },
    },
  },
}
```

---

## Azure Speech

**URL:** https://docs.openclaw.ai/providers/azure-speech

**Contents:**
- Azure Speech
- Documentation Index
- ​Getting started
- ​Configuration options
- ​Notes
- ​Related
- Text-to-speech
- Configuration
- Providers
- Troubleshooting

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Create an Azure Speech resource

Select Azure Speech in messages.tts

**Examples:**

Example 1 (sass):
```sass
AZURE_SPEECH_KEY=<speech-resource-key>
AZURE_SPEECH_REGION=eastus
```

Example 2 (json):
```json
{
  messages: {
    tts: {
      auto: "always",
      provider: "azure-speech",
      providers: {
        "azure-speech": {
          voice: "en-US-JennyNeural",
          lang: "en-US",
        },
      },
    },
  },
}
```

---

## Tencent Cloud (TokenHub)

**URL:** https://docs.openclaw.ai/providers/tencent

**Contents:**
- Tencent Cloud (TokenHub)
- Documentation Index
- ​Tencent Cloud TokenHub
- ​Quick start
- ​Non-interactive setup
- ​Built-in catalog
- ​Endpoint override
- ​Notes
- ​Environment note
- ​Related documentation

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Create a TokenHub API key

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice tokenhub-api-key
```

Example 2 (unknown):
```unknown
openclaw models list --provider tencent-tokenhub
```

Example 3 (bash):
```bash
openclaw onboard --non-interactive \
  --mode local \
  --auth-choice tokenhub-api-key \
  --tokenhub-api-key "$TOKENHUB_API_KEY" \
  --skip-health \
  --accept-risk
```

Example 4 (unknown):
```unknown
openclaw config set models.providers.tencent-tokenhub.baseUrl "https://tokenhub-intl.tencentmaas.com/v1"
```

---

## Device model database

**URL:** https://docs.openclaw.ai/reference/device-models

**Contents:**
- Device model database
- Documentation Index
- ​Data source
- ​Updating the database
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (jsx):
```jsx
IOS_COMMIT="<commit sha for ios-device-identifiers.json>"
MAC_COMMIT="<commit sha for mac-device-identifiers.json>"

curl -fsSL "https://raw.githubusercontent.com/kyle-seongwoo-jun/apple-device-identifiers/${IOS_COMMIT}/ios-device-identifiers.json" \
  -o apps/macos/Sources/OpenClaw/Resources/DeviceModels/ios-device-identifiers.json

curl -fsSL "https://raw.githubusercontent.com/kyle-seongwoo-jun/apple-device-identifiers/${MAC_COMMIT}/mac-device-identifiers.json" \
  -o apps/macos/Sources/OpenClaw/Resources/DeviceModels/mac-device-identifiers.json
```

Example 2 (unknown):
```unknown
swift build --package-path apps/macos
```

---

## Perplexity

**URL:** https://docs.openclaw.ai/providers/perplexity-provider

**Contents:**
- Perplexity
- Documentation Index
- ​Getting started
- ​Search modes
- ​Native API filtering
- ​Advanced configuration
- ​Related
- Perplexity search tool
- Configuration reference

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Environment variable for daemon processes

OpenRouter proxy setup

**Examples:**

Example 1 (unknown):
```unknown
openclaw configure --section web
```

Example 2 (unknown):
```unknown
openclaw config set plugins.entries.perplexity.config.webSearch.apiKey "pplx-xxxxxxxxxxxx"
```

---

## Provider directory

**URL:** https://docs.openclaw.ai/providers

**Contents:**
- Provider directory
- Documentation Index
- ​Model Providers
- ​Quick start
- ​Provider docs
- ​Shared overview pages
- ​Transcription providers
- ​Community tools

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-6" } } },
}
```

---

## Groq

**URL:** https://docs.openclaw.ai/providers/groq

**Contents:**
- Groq
- Documentation Index
- ​Getting started
  - ​Config file example
- ​Built-in catalog
- ​Reasoning models
- ​Audio transcription
- ​Related
- Model selection
- Configuration reference

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Audio transcription details

**Examples:**

Example 1 (lua):
```lua
export GROQ_API_KEY="gsk_..."
```

Example 2 (json):
```json
{
  agents: {
    defaults: {
      model: { primary: "groq/llama-3.3-70b-versatile" },
    },
  },
}
```

Example 3 (lua):
```lua
{
  env: { GROQ_API_KEY: "gsk_..." },
  agents: {
    defaults: {
      model: { primary: "groq/llama-3.3-70b-versatile" },
    },
  },
}
```

Example 4 (json):
```json
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

---

## Arcee AI

**URL:** https://docs.openclaw.ai/providers/arcee

**Contents:**
- Arcee AI
- Documentation Index
- ​Getting started
- ​Non-interactive setup
- ​Built-in catalog
- ​Supported features
- ​Related
- OpenRouter
- Model selection

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice arceeai-api-key
```

Example 2 (json):
```json
{
  agents: {
    defaults: {
      model: { primary: "arcee/trinity-large-thinking" },
    },
  },
}
```

Example 3 (unknown):
```unknown
openclaw onboard --auth-choice arceeai-openrouter
```

Example 4 (json):
```json
{
  agents: {
    defaults: {
      model: { primary: "arcee/trinity-large-thinking" },
    },
  },
}
```

---

## Xiaomi MiMo

**URL:** https://docs.openclaw.ai/providers/xiaomi

**Contents:**
- Xiaomi MiMo
- Documentation Index
- ​Getting started
- ​Built-in catalog
- ​Text-to-speech
- ​Config example
- ​Related
- Model selection
- Configuration reference
- Xiaomi MiMo console

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify the model is available

Auto-injection behavior

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice xiaomi-api-key
```

Example 2 (bash):
```bash
openclaw onboard --auth-choice xiaomi-api-key --xiaomi-api-key "$XIAOMI_API_KEY"
```

Example 3 (unknown):
```unknown
openclaw models list --provider xiaomi
```

Example 4 (json):
```json
{
  messages: {
    tts: {
      auto: "always",
      provider: "xiaomi",
      providers: {
        xiaomi: {
          apiKey: "xiaomi_api_key",
          model: "mimo-v2.5-tts",
          voice: "mimo_default",
          format: "mp3",
          style: "Bright, natural, conversational tone.",
        },
      },
    },
  },
}
```

---

## Z.AI

**URL:** https://docs.openclaw.ai/providers/zai

**Contents:**
- Z.AI
- Documentation Index
- ​Getting started
- ​Built-in catalog
- ​Advanced configuration
- ​Related
- GLM model family
- Model selection

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify the model is listed

Pick the right onboarding choice

Verify the model is listed

Forward-resolving unknown GLM-5 models

Thinking and preserved thinking

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice zai-api-key
```

Example 2 (lua):
```lua
{
  env: { ZAI_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "zai/glm-5.1" } } },
}
```

Example 3 (unknown):
```unknown
openclaw models list --all --provider zai
```

Example 4 (markdown):
```markdown
# Coding Plan Global (recommended for Coding Plan users)
openclaw onboard --auth-choice zai-coding-global

# Coding Plan CN (China region)
openclaw onboard --auth-choice zai-coding-cn

# General API
openclaw onboard --auth-choice zai-global

# General API CN (China region)
openclaw onboard --auth-choice zai-cn
```

---

## Qwen

**URL:** https://docs.openclaw.ai/providers/qwen

**Contents:**
- Qwen
- Documentation Index
- ​Getting started
- ​Plan types and endpoints
- ​Built-in catalog
- ​Thinking Controls
- ​Multimodal add-ons
- ​Advanced configuration
- ​Related
- Model selection

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify the model is available

Verify the model is available

Image and video understanding

Qwen 3.6 Plus availability

Video generation details

Streaming usage compatibility

Multimodal endpoint regions

Environment and daemon setup

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice qwen-api-key
```

Example 2 (unknown):
```unknown
openclaw onboard --auth-choice qwen-api-key-cn
```

Example 3 (json):
```json
{
  agents: {
    defaults: {
      model: { primary: "qwen/qwen3.5-plus" },
    },
  },
}
```

Example 4 (unknown):
```unknown
openclaw models list --provider qwen
```

---

## vLLM

**URL:** https://docs.openclaw.ai/providers/vllm

**Contents:**
- vLLM
- Documentation Index
- ​Getting started
- ​Model discovery (implicit provider)
- ​Explicit configuration (manual models)
- ​Advanced configuration
- ​Troubleshooting
- ​Related
- Model selection
- OpenAI

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Start vLLM with an OpenAI-compatible server

Set the API key environment variable

Verify the model is available

Qwen thinking controls

Nemotron 3 thinking controls

Qwen tool calls appear as text

Slow first response or remote server timeout

Auth errors on requests

Tools render as raw text

**Examples:**

Example 1 (yaml):
```yaml
http://127.0.0.1:8000/v1
```

Example 2 (unknown):
```unknown
export VLLM_API_KEY="vllm-local"
```

Example 3 (json):
```json
{
  agents: {
    defaults: {
      model: { primary: "vllm/your-model-id" },
    },
  },
}
```

Example 4 (unknown):
```unknown
openclaw models list --provider vllm
```

---

## ComfyUI

**URL:** https://docs.openclaw.ai/providers/comfy

**Contents:**
- ComfyUI
- Documentation Index
- ​What it supports
- ​Getting started
- ​Configuration
  - ​Shared keys
  - ​Per-capability keys
- ​Workflow details
- ​Related
- Image Generation

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Start ComfyUI locally

Prepare your workflow JSON

Configure the provider

Set the default model

Prepare your workflow JSON

Configure the provider

Set the default model

Backward compatibility

**Examples:**

Example 1 (json):
```json
{
  plugins: {
    entries: {
      comfy: {
        config: {
          mode: "local",
          baseUrl: "http://127.0.0.1:8188",
          image: {
            workflowPath: "./workflows/flux-api.json",
            promptNodeId: "6",
            outputNodeId: "9",
          },
        },
      },
    },
  },
}
```

Example 2 (json):
```json
{
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "comfy/workflow",
      },
    },
  },
}
```

Example 3 (unknown):
```unknown
openclaw models list --provider comfy
```

Example 4 (markdown):
```markdown
# Environment variable (preferred)
export COMFY_API_KEY="your-key"

# Alternative environment variable
export COMFY_CLOUD_API_KEY="your-key"

# Or inline in config
openclaw config set plugins.entries.comfy.config.apiKey "your-key"
```

---

## MiniMax

**URL:** https://docs.openclaw.ai/providers/minimax

**Contents:**
- MiniMax
- Documentation Index
- ​Built-in catalog
- ​Getting started
  - ​Config example
- ​Configure via openclaw configure
- ​Capabilities
  - ​Image generation
  - ​Text-to-speech
  - ​Music generation

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Verify the model is available

Verify the model is available

Verify the model is available

Verify the model is available

Choose a MiniMax auth option

Pick your default model

Configuration options

Coding Plan usage details

"Unknown model: minimax/MiniMax-M2.7"

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --auth-choice minimax-global-oauth
```

Example 2 (unknown):
```unknown
openclaw models list --provider minimax-portal
```

Example 3 (unknown):
```unknown
openclaw onboard --auth-choice minimax-cn-oauth
```

Example 4 (unknown):
```unknown
openclaw models list --provider minimax-portal
```

---
