---
source_url: https://docs.openclaw.ai/providers
title: "Provider directory - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/providers#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Overview

Provider directory

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Model Providers](https://docs.openclaw.ai/providers#model-providers)
- [Quick start](https://docs.openclaw.ai/providers#quick-start)
- [Provider docs](https://docs.openclaw.ai/providers#provider-docs)
- [Shared overview pages](https://docs.openclaw.ai/providers#shared-overview-pages)
- [Transcription providers](https://docs.openclaw.ai/providers#transcription-providers)
- [Community tools](https://docs.openclaw.ai/providers#community-tools)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/providers\#model-providers)  Model Providers

OpenClaw can use many LLM providers. Pick a provider, authenticate, then set the
default model as `provider/model`.Looking for chat channel docs (WhatsApp/Telegram/Discord/Slack/Mattermost (plugin)/etc.)? See [Channels](https://docs.openclaw.ai/channels).

## [​](https://docs.openclaw.ai/providers\#quick-start)  Quick start

1. Authenticate with the provider (usually via `openclaw onboard`).
2. Set the default model:

```
{
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-6" } } },
}
```

## [​](https://docs.openclaw.ai/providers\#provider-docs)  Provider docs

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

## [​](https://docs.openclaw.ai/providers\#shared-overview-pages)  Shared overview pages

- [Additional bundled variants](https://docs.openclaw.ai/providers/models#additional-bundled-provider-variants) \- Anthropic Vertex, Copilot Proxy, and Gemini CLI OAuth
- [Image Generation](https://docs.openclaw.ai/tools/image-generation) \- Shared `image_generate` tool, provider selection, and failover
- [Music Generation](https://docs.openclaw.ai/tools/music-generation) \- Shared `music_generate` tool, provider selection, and failover
- [Video Generation](https://docs.openclaw.ai/tools/video-generation) \- Shared `video_generate` tool, provider selection, and failover

## [​](https://docs.openclaw.ai/providers\#transcription-providers)  Transcription providers

- [Deepgram (audio transcription)](https://docs.openclaw.ai/providers/deepgram)
- [ElevenLabs](https://docs.openclaw.ai/providers/elevenlabs#speech-to-text)
- [Mistral](https://docs.openclaw.ai/providers/mistral#audio-transcription-voxtral)
- [OpenAI](https://docs.openclaw.ai/providers/openai#speech-to-text)
- [SenseAudio](https://docs.openclaw.ai/providers/senseaudio)
- [xAI](https://docs.openclaw.ai/providers/xai#speech-to-text)

## [​](https://docs.openclaw.ai/providers\#community-tools)  Community tools

- [Claude Max API Proxy](https://docs.openclaw.ai/providers/claude-max-api-proxy) \- Community proxy for Claude subscription credentials (verify Anthropic policy/terms before use)

For the full provider catalog (xAI, Groq, Mistral, etc.) and advanced configuration,
see [Model providers](https://docs.openclaw.ai/concepts/model-providers).

[Model provider quickstart](https://docs.openclaw.ai/providers/models)

Ctrl+I