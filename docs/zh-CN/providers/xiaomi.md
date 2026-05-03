---
source_url: https://docs.openclaw.ai/zh-CN/providers/xiaomi
title: "Xiaomi MiMo - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/providers/xiaomi#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

提供商

Xiaomi MiMo

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [入门指南](https://docs.openclaw.ai/zh-CN/providers/xiaomi#%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97)
- [内置目录](https://docs.openclaw.ai/zh-CN/providers/xiaomi#%E5%86%85%E7%BD%AE%E7%9B%AE%E5%BD%95)
- [文本转语音](https://docs.openclaw.ai/zh-CN/providers/xiaomi#%E6%96%87%E6%9C%AC%E8%BD%AC%E8%AF%AD%E9%9F%B3)
- [配置示例](https://docs.openclaw.ai/zh-CN/providers/xiaomi#%E9%85%8D%E7%BD%AE%E7%A4%BA%E4%BE%8B)
- [相关内容](https://docs.openclaw.ai/zh-CN/providers/xiaomi#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Xiaomi MiMo 是 **MiMo** 模型的 API 平台。OpenClaw 使用 Xiaomi 的 OpenAI 兼容端点，并通过 API 密钥进行身份验证。

| 属性 | 值 |
| --- | --- |
| 提供商 | `xiaomi` |
| 认证 | `XIAOMI_API_KEY` |
| API | OpenAI 兼容 |
| Base URL | `https://api.xiaomimimo.com/v1` |

## [​](https://docs.openclaw.ai/zh-CN/providers/xiaomi\#%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97)  入门指南

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/xiaomi#)

获取 API 密钥

在 [Xiaomi MiMo 控制台](https://platform.xiaomimimo.com/#/console/api-keys) 中创建一个 API 密钥。

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/xiaomi#)

运行新手引导

```
openclaw onboard --auth-choice xiaomi-api-key
```

或直接传入密钥：

```
openclaw onboard --auth-choice xiaomi-api-key --xiaomi-api-key "$XIAOMI_API_KEY"
```

3

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/xiaomi#)

验证模型是否可用

```
openclaw models list --provider xiaomi
```

## [​](https://docs.openclaw.ai/zh-CN/providers/xiaomi\#%E5%86%85%E7%BD%AE%E7%9B%AE%E5%BD%95)  内置目录

| 模型引用 | 输入 | 上下文 | 最大输出 | 推理 | 说明 |
| --- | --- | --- | --- | --- | --- |
| `xiaomi/mimo-v2-flash` | text | 262,144 | 8,192 | 否 | 默认模型 |
| `xiaomi/mimo-v2-pro` | text | 1,048,576 | 32,000 | 是 | 大上下文 |
| `xiaomi/mimo-v2-omni` | text, image | 262,144 | 32,000 | 是 | 多模态 |

默认模型引用是 `xiaomi/mimo-v2-flash`。当设置了 `XIAOMI_API_KEY` 或存在认证配置文件时，系统会自动注入该提供商。

## [​](https://docs.openclaw.ai/zh-CN/providers/xiaomi\#%E6%96%87%E6%9C%AC%E8%BD%AC%E8%AF%AD%E9%9F%B3)  文本转语音

内置的 `xiaomi` 插件还会将 Xiaomi MiMo 注册为 `messages.tts` 的语音提供商。它会调用 Xiaomi 的 chat-completions TTS 协议，将文本作为 `assistant` 消息传入，并将可选的风格指引作为 `user` 消息传入。

| 属性 | 值 |
| --- | --- |
| TTS id | `xiaomi` (`mimo` alias) |
| 认证 | `XIAOMI_API_KEY` |
| API | `POST /v1/chat/completions` with `audio` |
| 默认值 | `mimo-v2.5-tts`，语音为 `mimo_default` |
| 输出 | 默认是 MP3；配置后可使用 WAV |

```
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

支持的内置语音包括 `mimo_default`、`default_zh`、`default_en`、`Mia`、`Chloe`、`Milo` 和 `Dean`。`mimo-v2-tts` 也支持较旧的 MiMo TTS 账户；默认使用当前的 MiMo-V2.5 TTS 模型。对于 Feishu 和 Telegram 等语音消息目标，OpenClaw 会在发送前使用 `ffmpeg` 将 Xiaomi 的输出转码为 48 kHz Opus。

## [​](https://docs.openclaw.ai/zh-CN/providers/xiaomi\#%E9%85%8D%E7%BD%AE%E7%A4%BA%E4%BE%8B)  配置示例

```
{
  env: { XIAOMI_API_KEY: "your-key" },
  agents: { defaults: { model: { primary: "xiaomi/mimo-v2-flash" } } },
  models: {
    mode: "merge",
    providers: {
      xiaomi: {
        baseUrl: "https://api.xiaomimimo.com/v1",
        api: "openai-completions",
        apiKey: "XIAOMI_API_KEY",
        models: [\
          {\
            id: "mimo-v2-flash",\
            name: "Xiaomi MiMo V2 Flash",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 262144,\
            maxTokens: 8192,\
          },\
          {\
            id: "mimo-v2-pro",\
            name: "Xiaomi MiMo V2 Pro",\
            reasoning: true,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 1048576,\
            maxTokens: 32000,\
          },\
          {\
            id: "mimo-v2-omni",\
            name: "Xiaomi MiMo V2 Omni",\
            reasoning: true,\
            input: ["text", "image"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 262144,\
            maxTokens: 32000,\
          },\
        ],
      },
    },
  },
}
```

自动注入行为

当你的环境中设置了 `XIAOMI_API_KEY` 或存在认证配置文件时，系统会自动注入 `xiaomi` 提供商。除非你想覆盖模型元数据或 Base URL，否则无需手动配置该提供商。

模型详情

- **mimo-v2-flash** — 轻量且快速，适合通用文本任务。不支持推理。
- **mimo-v2-pro** — 支持推理，具有 100 万 token 上下文窗口，适合长文档工作负载。
- **mimo-v2-omni** — 支持推理的多模态模型，同时接受文本和图像输入。

所有模型都使用 `xiaomi/` 前缀（例如 `xiaomi/mimo-v2-pro`）。

故障排除

- 如果模型没有显示，请确认 `XIAOMI_API_KEY` 已设置且有效。
- 当 Gateway 网关 以守护进程方式运行时，请确保该密钥对该进程可用（例如在 `~/.openclaw/.env` 中，或通过 `env.shellEnv`）。

仅在交互式 shell 中设置的密钥对由守护进程管理的 Gateway 网关 进程不可见。请使用 `~/.openclaw/.env` 或 `env.shellEnv` 配置来实现持久可用性。

## [​](https://docs.openclaw.ai/zh-CN/providers/xiaomi\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

[**模型选择** \\
\\
选择提供商、模型引用和故障切换行为。](https://docs.openclaw.ai/zh-CN/concepts/model-providers)

[**配置参考** \\
\\
完整的 OpenClaw 配置参考。](https://docs.openclaw.ai/zh-CN/gateway/configuration-reference)

[**Xiaomi MiMo 控制台** \\
\\
Xiaomi MiMo 仪表板和 API 密钥管理。](https://platform.xiaomimimo.com/)

[Vercel AI 网关](https://docs.openclaw.ai/zh-CN/providers/vercel-ai-gateway) [Z.AI](https://docs.openclaw.ai/zh-CN/providers/zai)

Ctrl+I