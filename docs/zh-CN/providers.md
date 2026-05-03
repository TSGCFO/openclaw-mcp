---
source_url: https://docs.openclaw.ai/zh-CN/providers
title: "\u63d0\u4f9b\u5546\u76ee\u5f55 - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/providers#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

概览

提供商目录

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [模型提供商](https://docs.openclaw.ai/zh-CN/providers#%E6%A8%A1%E5%9E%8B%E6%8F%90%E4%BE%9B%E5%95%86)
- [快速开始](https://docs.openclaw.ai/zh-CN/providers#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)
- [提供商文档](https://docs.openclaw.ai/zh-CN/providers#%E6%8F%90%E4%BE%9B%E5%95%86%E6%96%87%E6%A1%A3)
- [共享概览页面](https://docs.openclaw.ai/zh-CN/providers#%E5%85%B1%E4%BA%AB%E6%A6%82%E8%A7%88%E9%A1%B5%E9%9D%A2)
- [转录提供商](https://docs.openclaw.ai/zh-CN/providers#%E8%BD%AC%E5%BD%95%E6%8F%90%E4%BE%9B%E5%95%86)
- [社区工具](https://docs.openclaw.ai/zh-CN/providers#%E7%A4%BE%E5%8C%BA%E5%B7%A5%E5%85%B7)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/zh-CN/providers\#%E6%A8%A1%E5%9E%8B%E6%8F%90%E4%BE%9B%E5%95%86)  模型提供商

OpenClaw 可以使用许多 LLM 提供商。选择一个提供商，完成身份验证，然后将默认模型设置为 `provider/model`。在找聊天渠道文档（WhatsApp/Telegram/Discord/Slack/Mattermost（插件）/ 等）？请参阅 [Channels](https://docs.openclaw.ai/zh-CN/channels)。

## [​](https://docs.openclaw.ai/zh-CN/providers\#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)  快速开始

1. 使用提供商完成身份验证（通常通过 `openclaw onboard`）。
2. 设置默认模型：

```
{
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-6" } } },
}
```

## [​](https://docs.openclaw.ai/zh-CN/providers\#%E6%8F%90%E4%BE%9B%E5%95%86%E6%96%87%E6%A1%A3)  提供商文档

- [Alibaba Model Studio](https://docs.openclaw.ai/zh-CN/providers/alibaba)
- [Amazon Bedrock](https://docs.openclaw.ai/zh-CN/providers/bedrock)
- [Amazon Bedrock Mantle](https://docs.openclaw.ai/zh-CN/providers/bedrock-mantle)
- [Anthropic（API + Claude CLI）](https://docs.openclaw.ai/zh-CN/providers/anthropic)
- [Arcee AI（Trinity 模型）](https://docs.openclaw.ai/zh-CN/providers/arcee)
- [Azure Speech](https://docs.openclaw.ai/zh-CN/providers/azure-speech)
- [BytePlus（国际版）](https://docs.openclaw.ai/zh-CN/concepts/model-providers#byteplus-international)
- [Cerebras](https://docs.openclaw.ai/zh-CN/providers/cerebras)
- [Chutes](https://docs.openclaw.ai/zh-CN/providers/chutes)
- [Cloudflare AI Gateway](https://docs.openclaw.ai/zh-CN/providers/cloudflare-ai-gateway)
- [ComfyUI](https://docs.openclaw.ai/zh-CN/providers/comfy)
- [DeepSeek](https://docs.openclaw.ai/zh-CN/providers/deepseek)
- [ElevenLabs](https://docs.openclaw.ai/zh-CN/providers/elevenlabs)
- [fal](https://docs.openclaw.ai/zh-CN/providers/fal)
- [Fireworks](https://docs.openclaw.ai/zh-CN/providers/fireworks)
- [GitHub Copilot](https://docs.openclaw.ai/zh-CN/providers/github-copilot)
- [GLM 模型](https://docs.openclaw.ai/zh-CN/providers/glm)
- [Google（Gemini）](https://docs.openclaw.ai/zh-CN/providers/google)
- [Gradium](https://docs.openclaw.ai/zh-CN/providers/gradium)
- [Groq（LPU 推理）](https://docs.openclaw.ai/zh-CN/providers/groq)
- [Hugging Face（Inference）](https://docs.openclaw.ai/zh-CN/providers/huggingface)
- [inferrs（本地模型）](https://docs.openclaw.ai/zh-CN/providers/inferrs)
- [Kilocode](https://docs.openclaw.ai/zh-CN/providers/kilocode)
- [LiteLLM（统一 Gateway 网关）](https://docs.openclaw.ai/zh-CN/providers/litellm)
- [LM Studio（本地模型）](https://docs.openclaw.ai/zh-CN/providers/lmstudio)
- [MiniMax](https://docs.openclaw.ai/zh-CN/providers/minimax)
- [Mistral](https://docs.openclaw.ai/zh-CN/providers/mistral)
- [Moonshot AI（Kimi + Kimi Coding）](https://docs.openclaw.ai/zh-CN/providers/moonshot)
- [NVIDIA](https://docs.openclaw.ai/zh-CN/providers/nvidia)
- [Ollama（云端 + 本地模型）](https://docs.openclaw.ai/zh-CN/providers/ollama)
- [OpenAI（API + Codex）](https://docs.openclaw.ai/zh-CN/providers/openai)
- [OpenCode](https://docs.openclaw.ai/zh-CN/providers/opencode)
- [OpenCode Go](https://docs.openclaw.ai/zh-CN/providers/opencode-go)
- [OpenRouter](https://docs.openclaw.ai/zh-CN/providers/openrouter)
- [Perplexity（网页搜索）](https://docs.openclaw.ai/zh-CN/providers/perplexity-provider)
- [Qianfan](https://docs.openclaw.ai/zh-CN/providers/qianfan)
- [Qwen Cloud](https://docs.openclaw.ai/zh-CN/providers/qwen)
- [Runway](https://docs.openclaw.ai/zh-CN/providers/runway)
- [SenseAudio](https://docs.openclaw.ai/zh-CN/providers/senseaudio)
- [SGLang（本地模型）](https://docs.openclaw.ai/zh-CN/providers/sglang)
- [StepFun](https://docs.openclaw.ai/zh-CN/providers/stepfun)
- [Synthetic](https://docs.openclaw.ai/zh-CN/providers/synthetic)
- [腾讯云（TokenHub）](https://docs.openclaw.ai/zh-CN/providers/tencent)
- [Together AI](https://docs.openclaw.ai/zh-CN/providers/together)
- [Venice（Venice AI，注重隐私）](https://docs.openclaw.ai/zh-CN/providers/venice)
- [Vercel AI Gateway](https://docs.openclaw.ai/zh-CN/providers/vercel-ai-gateway)
- [vLLM（本地模型）](https://docs.openclaw.ai/zh-CN/providers/vllm)
- [Volcengine（Doubao）](https://docs.openclaw.ai/zh-CN/providers/volcengine)
- [Vydra](https://docs.openclaw.ai/zh-CN/providers/vydra)
- [xAI](https://docs.openclaw.ai/zh-CN/providers/xai)
- [Xiaomi](https://docs.openclaw.ai/zh-CN/providers/xiaomi)
- [Z.AI](https://docs.openclaw.ai/zh-CN/providers/zai)

## [​](https://docs.openclaw.ai/zh-CN/providers\#%E5%85%B1%E4%BA%AB%E6%A6%82%E8%A7%88%E9%A1%B5%E9%9D%A2)  共享概览页面

- [其他内置变体](https://docs.openclaw.ai/zh-CN/providers/models#additional-bundled-provider-variants) \- Anthropic Vertex、Copilot Proxy 和 Gemini CLI OAuth
- [图像生成](https://docs.openclaw.ai/zh-CN/tools/image-generation) \- 共享的 `image_generate` 工具、提供商选择和故障切换
- [音乐生成](https://docs.openclaw.ai/zh-CN/tools/music-generation) \- 共享的 `music_generate` 工具、提供商选择和故障切换
- [视频生成](https://docs.openclaw.ai/zh-CN/tools/video-generation) \- 共享的 `video_generate` 工具、提供商选择和故障切换

## [​](https://docs.openclaw.ai/zh-CN/providers\#%E8%BD%AC%E5%BD%95%E6%8F%90%E4%BE%9B%E5%95%86)  转录提供商

- [Deepgram（音频转录）](https://docs.openclaw.ai/zh-CN/providers/deepgram)
- [ElevenLabs](https://docs.openclaw.ai/zh-CN/providers/elevenlabs#speech-to-text)
- [Mistral](https://docs.openclaw.ai/zh-CN/providers/mistral#audio-transcription-voxtral)
- [OpenAI](https://docs.openclaw.ai/zh-CN/providers/openai#speech-to-text)
- [SenseAudio](https://docs.openclaw.ai/zh-CN/providers/senseaudio)
- [xAI](https://docs.openclaw.ai/zh-CN/providers/xai#speech-to-text)

## [​](https://docs.openclaw.ai/zh-CN/providers\#%E7%A4%BE%E5%8C%BA%E5%B7%A5%E5%85%B7)  社区工具

- [Claude Max API Proxy](https://docs.openclaw.ai/zh-CN/providers/claude-max-api-proxy) \- 用于 Claude 订阅凭证的社区代理（使用前请核实 Anthropic 的政策/条款）

如需查看完整的提供商目录（xAI、Groq、Mistral 等）和高级配置，请参阅 [模型提供商](https://docs.openclaw.ai/zh-CN/concepts/model-providers)。

[模型提供商快速开始](https://docs.openclaw.ai/zh-CN/providers/models)

Ctrl+I