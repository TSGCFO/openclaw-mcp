---
source_url: https://docs.openclaw.ai/zh-CN/providers/litellm
title: "LiteLLM - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/zh-CN/providers/litellm#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

LiteLLM

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [快速开始](https://docs.openclaw.ai/zh-CN/providers/litellm#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)
- [配置](https://docs.openclaw.ai/zh-CN/providers/litellm#%E9%85%8D%E7%BD%AE)
- [环境变量](https://docs.openclaw.ai/zh-CN/providers/litellm#%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
- [配置文件](https://docs.openclaw.ai/zh-CN/providers/litellm#%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
- [高级配置](https://docs.openclaw.ai/zh-CN/providers/litellm#%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE)
- [图像生成](https://docs.openclaw.ai/zh-CN/providers/litellm#%E5%9B%BE%E5%83%8F%E7%94%9F%E6%88%90)
- [相关内容](https://docs.openclaw.ai/zh-CN/providers/litellm#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

[LiteLLM](https://litellm.ai/) 是一个开源 LLM 网关，提供统一 API 以接入 100 多家模型提供商。通过 LiteLLM 路由 OpenClaw，以获得集中式成本跟踪、日志记录，以及无需更改 OpenClaw 配置即可切换后端的灵活性。

**为什么将 LiteLLM 与 OpenClaw 一起使用？**

- **成本跟踪** — 准确查看 OpenClaw 在所有模型上的花费
- **模型路由** — 无需更改配置即可在 Claude、GPT-4、Gemini、Bedrock 之间切换
- **虚拟密钥** — 为 OpenClaw 创建带有支出限制的密钥
- **日志记录** — 完整的请求/响应日志，便于调试
- **故障回退** — 当你的主要提供商不可用时自动故障转移

## [​](https://docs.openclaw.ai/zh-CN/providers/litellm\#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)  快速开始

- 新手引导（推荐）

- 手动设置


**最适合：** 以最快方式完成可用的 LiteLLM 设置。

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/litellm#)

运行新手引导

```
openclaw onboard --auth-choice litellm-api-key
```

如果要针对远程代理进行非交互式设置，请显式传入代理 URL：

```
openclaw onboard --non-interactive --auth-choice litellm-api-key --litellm-api-key "$LITELLM_API_KEY" --custom-base-url "https://litellm.example/v1"
```

**最适合：** 完全控制安装和配置。

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/litellm#)

启动 LiteLLM Proxy

```
pip install 'litellm[proxy]'
litellm --model claude-opus-4-6
```

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/litellm#)

让 OpenClaw 指向 LiteLLM

```
export LITELLM_API_KEY="your-litellm-key"

openclaw
```

就这样。OpenClaw 现在会通过 LiteLLM 路由。

## [​](https://docs.openclaw.ai/zh-CN/providers/litellm\#%E9%85%8D%E7%BD%AE)  配置

### [​](https://docs.openclaw.ai/zh-CN/providers/litellm\#%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)  环境变量

```
export LITELLM_API_KEY="sk-litellm-key"
```

### [​](https://docs.openclaw.ai/zh-CN/providers/litellm\#%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)  配置文件

```
{
  models: {
    providers: {
      litellm: {
        baseUrl: "http://localhost:4000",
        apiKey: "${LITELLM_API_KEY}",
        api: "openai-completions",
        models: [\
          {\
            id: "claude-opus-4-6",\
            name: "Claude Opus 4.6",\
            reasoning: true,\
            input: ["text", "image"],\
            contextWindow: 200000,\
            maxTokens: 64000,\
          },\
          {\
            id: "gpt-4o",\
            name: "GPT-4o",\
            reasoning: false,\
            input: ["text", "image"],\
            contextWindow: 128000,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
  agents: {
    defaults: {
      model: { primary: "litellm/claude-opus-4-6" },
    },
  },
}
```

## [​](https://docs.openclaw.ai/zh-CN/providers/litellm\#%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE)  高级配置

### [​](https://docs.openclaw.ai/zh-CN/providers/litellm\#%E5%9B%BE%E5%83%8F%E7%94%9F%E6%88%90)  图像生成

LiteLLM 也可以通过兼容 OpenAI 的 `/images/generations` 和 `/images/edits` 路由，为 `image_generate` 工具提供支持。在 `agents.defaults.imageGenerationModel` 下配置一个 LiteLLM 图像模型：

```
{
  models: {
    providers: {
      litellm: {
        baseUrl: "http://localhost:4000",
        apiKey: "${LITELLM_API_KEY}",
      },
    },
  },
  agents: {
    defaults: {
      imageGenerationModel: {
        primary: "litellm/gpt-image-2",
        timeoutMs: 180_000,
      },
    },
  },
}
```

像 `http://localhost:4000` 这样的 loopback LiteLLM URL 无需全局私有网络覆盖即可工作。对于托管在局域网中的代理，请设置 `models.providers.litellm.request.allowPrivateNetwork: true`，因为 API 密钥将被发送到已配置的代理主机。

虚拟密钥

为 OpenClaw 创建一个带有支出限制的专用密钥：

```
curl -X POST "http://localhost:4000/key/generate" \
  -H "Authorization: Bearer $LITELLM_MASTER_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "key_alias": "openclaw",
    "max_budget": 50.00,
    "budget_duration": "monthly"
  }'
```

将生成的密钥用作 `LITELLM_API_KEY`。

模型路由

LiteLLM 可以将模型请求路由到不同后端。在你的 LiteLLM `config.yaml` 中进行配置：

```
model_list:
  - model_name: claude-opus-4-6
    litellm_params:
      model: claude-opus-4-6
      api_key: os.environ/ANTHROPIC_API_KEY

  - model_name: gpt-4o
    litellm_params:
      model: gpt-4o
      api_key: os.environ/OPENAI_API_KEY
```

OpenClaw 会继续请求 `claude-opus-4-6`——由 LiteLLM 负责处理路由。

查看使用情况

查看 LiteLLM 的仪表板或 API：

```
# 密钥信息
curl "http://localhost:4000/key/info" \
  -H "Authorization: Bearer sk-litellm-key"

# 支出日志
curl "http://localhost:4000/spend/logs" \
  -H "Authorization: Bearer $LITELLM_MASTER_KEY"
```

代理行为说明

- LiteLLM 默认运行在 `http://localhost:4000`
- OpenClaw 通过 LiteLLM 的代理式、兼容 OpenAI 的 `/v1` 端点进行连接
- 原生仅限 OpenAI 的请求整形不会通过 LiteLLM 生效：
不支持 `service_tier`、Responses `store`、prompt-cache 提示，也不支持
OpenAI reasoning 兼容载荷整形
- 在自定义 LiteLLM base URL 上，不会注入隐藏的 OpenClaw 归因请求头（`originator`、`version`、`User-Agent`）

有关通用 provider 配置和故障转移行为，请参阅 [Model Providers](https://docs.openclaw.ai/zh-CN/concepts/model-providers)。

## [​](https://docs.openclaw.ai/zh-CN/providers/litellm\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

[**LiteLLM 文档** \\
\\
LiteLLM 官方文档和 API 参考。](https://docs.litellm.ai/)

[**模型选择** \\
\\
所有 provider、模型引用和故障转移行为的概览。](https://docs.openclaw.ai/zh-CN/concepts/model-providers)

[**配置** \\
\\
完整配置参考。](https://docs.openclaw.ai/zh-CN/gateway/configuration)

[**模型选择** \\
\\
如何选择和配置模型。](https://docs.openclaw.ai/zh-CN/concepts/models)

Ctrl+I