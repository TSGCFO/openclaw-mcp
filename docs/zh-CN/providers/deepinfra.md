---
source_url: https://docs.openclaw.ai/zh-CN/providers/deepinfra
title: "Deepinfra - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/zh-CN/providers/deepinfra#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [DeepInfra](https://docs.openclaw.ai/zh-CN/providers/deepinfra#deepinfra)
- [获取 API 密钥](https://docs.openclaw.ai/zh-CN/providers/deepinfra#%E8%8E%B7%E5%8F%96-api-%E5%AF%86%E9%92%A5)
- [CLI 设置](https://docs.openclaw.ai/zh-CN/providers/deepinfra#cli-%E8%AE%BE%E7%BD%AE)
- [配置片段](https://docs.openclaw.ai/zh-CN/providers/deepinfra#%E9%85%8D%E7%BD%AE%E7%89%87%E6%AE%B5)
- [支持的 OpenClaw 界面](https://docs.openclaw.ai/zh-CN/providers/deepinfra#%E6%94%AF%E6%8C%81%E7%9A%84-openclaw-%E7%95%8C%E9%9D%A2)
- [可用模型](https://docs.openclaw.ai/zh-CN/providers/deepinfra#%E5%8F%AF%E7%94%A8%E6%A8%A1%E5%9E%8B)
- [说明](https://docs.openclaw.ai/zh-CN/providers/deepinfra#%E8%AF%B4%E6%98%8E)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/zh-CN/providers/deepinfra\#deepinfra)  DeepInfra

DeepInfra 提供一个 **统一 API**，可通过单一端点和 API 密钥，将请求路由到最受欢迎的开源模型和前沿模型。它兼容 OpenAI，因此大多数 OpenAI SDK 只需切换 base URL 即可使用。

## [​](https://docs.openclaw.ai/zh-CN/providers/deepinfra\#%E8%8E%B7%E5%8F%96-api-%E5%AF%86%E9%92%A5)  获取 API 密钥

1. 前往 [https://deepinfra.com/](https://deepinfra.com/)
2. 登录或创建账户
3. 进入 Dashboard / Keys，生成新的 API 密钥，或使用自动创建的密钥

## [​](https://docs.openclaw.ai/zh-CN/providers/deepinfra\#cli-%E8%AE%BE%E7%BD%AE)  CLI 设置

```
openclaw onboard --deepinfra-api-key <key>
```

或者设置环境变量：

```
export DEEPINFRA_API_KEY="<your-deepinfra-api-key>" # pragma: allowlist secret
```

## [​](https://docs.openclaw.ai/zh-CN/providers/deepinfra\#%E9%85%8D%E7%BD%AE%E7%89%87%E6%AE%B5)  配置片段

```
{
  env: { DEEPINFRA_API_KEY: "<your-deepinfra-api-key>" }, // pragma: allowlist secret
  agents: {
    defaults: {
      model: { primary: "deepinfra/deepseek-ai/DeepSeek-V3.2" },
    },
  },
}
```

## [​](https://docs.openclaw.ai/zh-CN/providers/deepinfra\#%E6%94%AF%E6%8C%81%E7%9A%84-openclaw-%E7%95%8C%E9%9D%A2)  支持的 OpenClaw 界面

内置插件会注册所有与当前 OpenClaw provider 契约匹配的 DeepInfra 界面：

| 界面 | 默认模型 | OpenClaw 配置/工具 |
| --- | --- | --- |
| 聊天 / 模型 provider | `deepseek-ai/DeepSeek-V3.2` | `agents.defaults.model` |
| 图像生成/编辑 | `black-forest-labs/FLUX-1-schnell` | `image_generate`, `agents.defaults.imageGenerationModel` |
| 媒体理解 | 图像使用 `moonshotai/Kimi-K2.5` | 入站图像理解 |
| 语音转文本 | `openai/whisper-large-v3-turbo` | 入站音频转录 |
| 文本转语音 | `hexgrad/Kokoro-82M` | `messages.tts.provider: "deepinfra"` |
| 视频生成 | `Pixverse/Pixverse-T2V` | `video_generate`, `agents.defaults.videoGenerationModel` |
| 记忆嵌入 | `BAAI/bge-m3` | `agents.defaults.memorySearch.provider: "deepinfra"` |

DeepInfra 还提供重排序、分类、目标检测及其他原生模型类型。OpenClaw 当前尚未为这些类别提供一流的 provider 契约，因此此插件暂时不会注册它们。

## [​](https://docs.openclaw.ai/zh-CN/providers/deepinfra\#%E5%8F%AF%E7%94%A8%E6%A8%A1%E5%9E%8B)  可用模型

OpenClaw 会在启动时动态发现可用的 DeepInfra 模型。使用 `/models deepinfra` 查看完整的可用模型列表。[DeepInfra.com](https://deepinfra.com/) 上可用的任何模型都可以通过 `deepinfra/` 前缀使用：

```
deepinfra/MiniMaxAI/MiniMax-M2.5
deepinfra/deepseek-ai/DeepSeek-V3.2
deepinfra/moonshotai/Kimi-K2.5
deepinfra/zai-org/GLM-5.1
...以及更多
```

## [​](https://docs.openclaw.ai/zh-CN/providers/deepinfra\#%E8%AF%B4%E6%98%8E)  说明

- 模型引用格式为 `deepinfra/<provider>/<model>`（例如 `deepinfra/Qwen/Qwen3-Max`）。
- 默认模型：`deepinfra/deepseek-ai/DeepSeek-V3.2`
- Base URL：`https://api.deepinfra.com/v1/openai`
- 原生视频生成使用 `https://api.deepinfra.com/v1/inference/<model>`。

Ctrl+I