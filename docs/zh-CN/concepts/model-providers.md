---
source_url: https://docs.openclaw.ai/zh-CN/concepts/model-providers
title: "\u6a21\u578b\u63d0\u4f9b\u5546 - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/concepts/model-providers#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

配置

模型提供商

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [快速规则](https://docs.openclaw.ai/zh-CN/concepts/model-providers#%E5%BF%AB%E9%80%9F%E8%A7%84%E5%88%99)
- [插件拥有的提供商行为](https://docs.openclaw.ai/zh-CN/concepts/model-providers#%E6%8F%92%E4%BB%B6%E6%8B%A5%E6%9C%89%E7%9A%84%E6%8F%90%E4%BE%9B%E5%95%86%E8%A1%8C%E4%B8%BA)
- [API key 轮换](https://docs.openclaw.ai/zh-CN/concepts/model-providers#api-key-%E8%BD%AE%E6%8D%A2)
- [内置提供商（pi-ai 目录）](https://docs.openclaw.ai/zh-CN/concepts/model-providers#%E5%86%85%E7%BD%AE%E6%8F%90%E4%BE%9B%E5%95%86%EF%BC%88pi-ai-%E7%9B%AE%E5%BD%95%EF%BC%89)
- [OpenAI](https://docs.openclaw.ai/zh-CN/concepts/model-providers#openai)
- [Anthropic](https://docs.openclaw.ai/zh-CN/concepts/model-providers#anthropic)
- [OpenAI Codex OAuth](https://docs.openclaw.ai/zh-CN/concepts/model-providers#openai-codex-oauth)
- [其他订阅式托管选项](https://docs.openclaw.ai/zh-CN/concepts/model-providers#%E5%85%B6%E4%BB%96%E8%AE%A2%E9%98%85%E5%BC%8F%E6%89%98%E7%AE%A1%E9%80%89%E9%A1%B9)
- [OpenCode](https://docs.openclaw.ai/zh-CN/concepts/model-providers#opencode)
- [Google Gemini（API key）](https://docs.openclaw.ai/zh-CN/concepts/model-providers#google-gemini%EF%BC%88api-key%EF%BC%89)
- [Google Vertex 和 Gemini CLI](https://docs.openclaw.ai/zh-CN/concepts/model-providers#google-vertex-%E5%92%8C-gemini-cli)
- [Z.AI (GLM)](https://docs.openclaw.ai/zh-CN/concepts/model-providers#z-ai-glm)
- [Vercel AI Gateway 网关](https://docs.openclaw.ai/zh-CN/concepts/model-providers#vercel-ai-gateway-%E7%BD%91%E5%85%B3)
- [Kilo Gateway 网关](https://docs.openclaw.ai/zh-CN/concepts/model-providers#kilo-gateway-%E7%BD%91%E5%85%B3)
- [其他内置提供商插件](https://docs.openclaw.ai/zh-CN/concepts/model-providers#%E5%85%B6%E4%BB%96%E5%86%85%E7%BD%AE%E6%8F%90%E4%BE%9B%E5%95%86%E6%8F%92%E4%BB%B6)
- [需要了解的特性](https://docs.openclaw.ai/zh-CN/concepts/model-providers#%E9%9C%80%E8%A6%81%E4%BA%86%E8%A7%A3%E7%9A%84%E7%89%B9%E6%80%A7)
- [通过 models.providers 使用提供商（自定义/基础 URL）](https://docs.openclaw.ai/zh-CN/concepts/model-providers#%E9%80%9A%E8%BF%87-models-providers-%E4%BD%BF%E7%94%A8%E6%8F%90%E4%BE%9B%E5%95%86%EF%BC%88%E8%87%AA%E5%AE%9A%E4%B9%89%2F%E5%9F%BA%E7%A1%80-url%EF%BC%89)
- [Moonshot AI（Kimi）](https://docs.openclaw.ai/zh-CN/concepts/model-providers#moonshot-ai%EF%BC%88kimi%EF%BC%89)
- [Kimi 编程](https://docs.openclaw.ai/zh-CN/concepts/model-providers#kimi-%E7%BC%96%E7%A8%8B)
- [Volcano Engine（Doubao）](https://docs.openclaw.ai/zh-CN/concepts/model-providers#volcano-engine%EF%BC%88doubao%EF%BC%89)
- [BytePlus（国际版）](https://docs.openclaw.ai/zh-CN/concepts/model-providers#byteplus%EF%BC%88%E5%9B%BD%E9%99%85%E7%89%88%EF%BC%89)
- [Synthetic](https://docs.openclaw.ai/zh-CN/concepts/model-providers#synthetic)
- [MiniMax](https://docs.openclaw.ai/zh-CN/concepts/model-providers#minimax)
- [LM Studio](https://docs.openclaw.ai/zh-CN/concepts/model-providers#lm-studio)
- [Ollama](https://docs.openclaw.ai/zh-CN/concepts/model-providers#ollama)
- [vLLM](https://docs.openclaw.ai/zh-CN/concepts/model-providers#vllm)
- [SGLang](https://docs.openclaw.ai/zh-CN/concepts/model-providers#sglang)
- [本地代理（LM Studio、vLLM、LiteLLM 等）](https://docs.openclaw.ai/zh-CN/concepts/model-providers#%E6%9C%AC%E5%9C%B0%E4%BB%A3%E7%90%86%EF%BC%88lm-studio%E3%80%81vllm%E3%80%81litellm-%E7%AD%89%EF%BC%89)
- [CLI 示例](https://docs.openclaw.ai/zh-CN/concepts/model-providers#cli-%E7%A4%BA%E4%BE%8B)
- [相关内容](https://docs.openclaw.ai/zh-CN/concepts/model-providers#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

模型选择规则请参阅 [Models](https://docs.openclaw.ai/zh-CN/concepts/models)。

## [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#%E5%BF%AB%E9%80%9F%E8%A7%84%E5%88%99)  快速规则

模型引用和 CLI 辅助命令

- 模型引用使用 `provider/model`（示例：`opencode/claude-opus-4-6`）。
- 设置 `agents.defaults.models` 后，它会作为允许列表。
- CLI 辅助命令：`openclaw onboard`、`openclaw models list`、`openclaw models set <provider/model>`。
- `models.providers.*.contextWindow` / `contextTokens` / `maxTokens` 设置提供商级默认值；`models.providers.*.models[].contextWindow` / `contextTokens` / `maxTokens` 会按模型覆盖它们。
- 回退规则、冷却探测和会话覆盖持久化： [模型故障转移](https://docs.openclaw.ai/zh-CN/concepts/model-failover)。

添加提供商凭证不会更改你的主模型

当你添加提供商或重新授权提供商时，`openclaw configure` 会保留现有的 `agents.defaults.model.primary`。提供商插件仍可能在其凭证配置补丁中返回推荐的默认模型，但如果主模型已经存在，configure 会将其视为“让此模型可用”，而不是“替换当前主模型”。若要有意切换默认模型，请使用 `openclaw models set <provider/model>` 或 `openclaw models auth login --provider <id> --set-default`。

OpenAI provider/运行时拆分

OpenAI 系列路由按前缀区分：

- `openai/<model>` 加上 `agents.defaults.agentRuntime.id: "codex"` 使用原生 Codex 应用服务器 harness。这是常见的 ChatGPT/Codex 订阅设置。
- `openai-codex/<model>` 在 PI 中使用 Codex OAuth。
- 没有 Codex 运行时覆盖的 `openai/<model>` 使用 PI 中的直接 OpenAI API key 提供商。

请参阅 [OpenAI](https://docs.openclaw.ai/zh-CN/providers/openai) 和 [Codex harness](https://docs.openclaw.ai/zh-CN/plugins/codex-harness)。如果 provider/运行时拆分令人困惑，请先阅读 [Agent Runtimes](https://docs.openclaw.ai/zh-CN/concepts/agent-runtimes)。插件自动启用遵循相同边界：`openai-codex/<model>` 属于 OpenAI 插件，而 Codex 插件由 `agentRuntime.id: "codex"` 或旧版 `codex/<model>` 引用启用。设置 `agentRuntime.id: "codex"` 时，GPT-5.5 可通过原生 Codex 应用服务器 harness 使用；对于 Codex OAuth，可通过 PI 中的 `openai-codex/gpt-5.5` 使用；当你的账号开放该模型时，对于直接 API-key 流量，可通过 PI 中的 `openai/gpt-5.5` 使用。

CLI 运行时

CLI 运行时使用相同拆分：选择规范模型引用，例如 `anthropic/claude-*`、`google/gemini-*` 或 `openai/gpt-*`，然后在需要本地 CLI 后端时，将 `agents.defaults.agentRuntime.id` 设置为 `claude-cli`、`google-gemini-cli` 或 `codex-cli`。旧版 `claude-cli/*`、`google-gemini-cli/*` 和 `codex-cli/*` 引用会迁移回规范提供商引用，并将运行时单独记录。

## [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#%E6%8F%92%E4%BB%B6%E6%8B%A5%E6%9C%89%E7%9A%84%E6%8F%90%E4%BE%9B%E5%95%86%E8%A1%8C%E4%B8%BA)  插件拥有的提供商行为

大多数提供商特定逻辑位于提供商插件（`registerProvider(...)`）中，而 OpenClaw 保留通用推理循环。插件拥有新手引导、模型目录、凭证环境变量映射、传输/配置规范化、工具 schema 清理、故障转移分类、OAuth 刷新、用量报告、thinking/reasoning 配置等。提供商 SDK 钩子和内置插件示例的完整列表位于 [提供商插件](https://docs.openclaw.ai/zh-CN/plugins/sdk-provider-plugins)。需要完全自定义请求执行器的提供商属于单独且更深层的扩展面。

提供商拥有的 runner 行为位于显式提供商钩子上，例如重放策略、工具 schema 规范化、流包装以及传输/请求辅助工具。旧版 `ProviderPlugin.capabilities` 静态包仅用于兼容性，不再由共享 runner 逻辑读取。

## [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#api-key-%E8%BD%AE%E6%8D%A2)  API key 轮换

密钥来源和优先级

通过以下方式配置多个密钥：

- `OPENCLAW_LIVE_<PROVIDER>_KEY`（单个实时覆盖，最高优先级）
- `<PROVIDER>_API_KEYS`（逗号或分号列表）
- `<PROVIDER>_API_KEY`（主密钥）
- `<PROVIDER>_API_KEY_*`（编号列表，例如 `<PROVIDER>_API_KEY_1`）

对于 Google 提供商，`GOOGLE_API_KEY` 也会作为回退包含在内。密钥选择顺序会保留优先级并对值去重。

何时触发轮换

- 仅在速率限制响应时，请求才会使用下一个密钥重试（例如 `429`、`rate_limit`、`quota`、`resource exhausted`、`Too many concurrent requests`、`ThrottlingException`、`concurrency limit reached`、`workers_ai ... quota limit exceeded` 或周期性用量限制消息）。
- 非速率限制失败会立即失败；不会尝试密钥轮换。
- 当所有候选密钥都失败时，将返回最后一次尝试的最终错误。

## [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#%E5%86%85%E7%BD%AE%E6%8F%90%E4%BE%9B%E5%95%86%EF%BC%88pi-ai-%E7%9B%AE%E5%BD%95%EF%BC%89)  内置提供商（pi-ai 目录）

OpenClaw 随附 pi-ai 目录。这些提供商 **不** 需要 `models.providers` 配置；只需设置凭证并选择模型。

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#openai)  OpenAI

- 提供商：`openai`
- 凭证：`OPENAI_API_KEY`
- 可选轮换：`OPENAI_API_KEYS`、`OPENAI_API_KEY_1`、`OPENAI_API_KEY_2`，以及 `OPENCLAW_LIVE_OPENAI_KEY`（单个覆盖）
- 示例模型：`openai/gpt-5.5`、`openai/gpt-5.4-mini`
- 如果特定安装或 API key 表现不同，请使用 `openclaw models list --provider openai` 验证账号/模型可用性。
- CLI：`openclaw onboard --auth-choice openai-api-key`
- 默认传输为 `auto`（WebSocket 优先，SSE 回退）
- 通过 `agents.defaults.models["openai/<model>"].params.transport` 按模型覆盖（`"sse"`、`"websocket"` 或 `"auto"`）
- OpenAI Responses WebSocket 预热默认通过 `params.openaiWsWarmup` 启用（`true`/`false`）
- OpenAI 优先级处理可通过 `agents.defaults.models["openai/<model>"].params.serviceTier` 启用
- `/fast` 和 `params.fastMode` 会将直接 `openai/*` Responses 请求映射到 `api.openai.com` 上的 `service_tier=priority`
- 当你想要显式层级而不是共享的 `/fast` 开关时，请使用 `params.serviceTier`
- 隐藏的 OpenClaw 归因标头（`originator`、`version`、`User-Agent`）仅应用于发往 `api.openai.com` 的原生 OpenAI 流量，不应用于通用 OpenAI 兼容代理
- 原生 OpenAI 路由还会保留 Responses `store`、prompt-cache 提示以及 OpenAI reasoning 兼容载荷整形；代理路由不会
- `openai/gpt-5.3-codex-spark` 在 OpenClaw 中被有意抑制，因为实时 OpenAI API 请求会拒绝它，且当前 Codex 目录未公开它

```
{
  agents: { defaults: { model: { primary: "openai/gpt-5.5" } } },
}
```

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#anthropic)  Anthropic

- 提供商：`anthropic`
- 凭证：`ANTHROPIC_API_KEY`
- 可选轮换：`ANTHROPIC_API_KEYS`、`ANTHROPIC_API_KEY_1`、`ANTHROPIC_API_KEY_2`，以及 `OPENCLAW_LIVE_ANTHROPIC_KEY`（单个覆盖）
- 示例模型：`anthropic/claude-opus-4-6`
- CLI：`openclaw onboard --auth-choice apiKey`
- 直接公共 Anthropic 请求支持共享的 `/fast` 开关和 `params.fastMode`，包括发送到 `api.anthropic.com` 的 API-key 和 OAuth 认证流量；OpenClaw 会将其映射到 Anthropic `service_tier`（`auto` 与 `standard_only`）
- 首选 Claude CLI 配置会保持模型引用规范，并单独选择 CLI
后端：`anthropic/claude-opus-4-7` 搭配
`agents.defaults.agentRuntime.id: "claude-cli"`。旧版
`claude-cli/claude-opus-4-7` 引用仍可用于兼容。

Anthropic 员工告诉我们，OpenClaw 风格的 Claude CLI 用法再次被允许，因此除非 Anthropic 发布新政策，否则 OpenClaw 会将此集成中的 Claude CLI 复用和 `claude -p` 用法视为已获准。Anthropic setup-token 仍作为受支持的 OpenClaw token 路径可用，但 OpenClaw 现在会在可用时优先使用 Claude CLI 复用和 `claude -p`。

```
{
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-6" } } },
}
```

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#openai-codex-oauth)  OpenAI Codex OAuth

- 提供商：`openai-codex`
- 凭证：OAuth (ChatGPT)
- PI 模型引用：`openai-codex/gpt-5.5`
- 原生 Codex 应用服务器 harness 引用：`openai/gpt-5.5` 搭配 `agents.defaults.agentRuntime.id: "codex"`
- 原生 Codex 应用服务器 harness 文档： [Codex harness](https://docs.openclaw.ai/zh-CN/plugins/codex-harness)
- 旧版模型引用：`codex/gpt-*`
- 插件边界：`openai-codex/*` 加载 OpenAI 插件；原生 Codex 应用服务器插件仅由 Codex harness 运行时或旧版 `codex/*` 引用选择。
- CLI：`openclaw onboard --auth-choice openai-codex` 或 `openclaw models auth login --provider openai-codex`
- 默认传输为 `auto`（WebSocket 优先，SSE 回退）
- 通过 `agents.defaults.models["openai-codex/<model>"].params.transport` 按 PI 模型覆盖（`"sse"`、`"websocket"` 或 `"auto"`）
- `params.serviceTier` 也会在原生 Codex Responses 请求（`chatgpt.com/backend-api`）上转发
- 隐藏的 OpenClaw 归因标头（`originator`、`version`、`User-Agent`）仅附加到发往 `chatgpt.com/backend-api` 的原生 Codex 流量，不附加到通用 OpenAI 兼容代理
- 与直接 `openai/*` 共享相同的 `/fast` 开关和 `params.fastMode` 配置；OpenClaw 会将其映射到 `service_tier=priority`
- `openai-codex/gpt-5.5` 使用 Codex 目录原生 `contextWindow = 400000` 和默认运行时 `contextTokens = 272000`；可通过 `models.providers.openai-codex.models[].contextTokens` 覆盖运行时上限
- 策略说明：OpenAI Codex OAuth 明确支持 OpenClaw 这类外部工具/工作流。
- 对于常见的订阅加原生 Codex 运行时路由，请使用 `openai-codex` 凭证登录，但配置 `openai/gpt-5.5` 加上 `agents.defaults.agentRuntime.id: "codex"`。
- 仅当你希望通过 PI 使用 Codex OAuth/订阅路由时，才使用 `openai-codex/gpt-5.5`；当你的 API-key 设置和本地目录公开公共 API 路由时，请使用不带 Codex 运行时覆盖的 `openai/gpt-5.5`。

```
{
  plugins: { entries: { codex: { enabled: true } } },
  agents: {
    defaults: {
      model: { primary: "openai/gpt-5.5" },
      agentRuntime: { id: "codex", fallback: "none" },
    },
  },
}
```

```
{
  models: {
    providers: {
      "openai-codex": {
        models: [{ id: "gpt-5.5", contextTokens: 160000 }],
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#%E5%85%B6%E4%BB%96%E8%AE%A2%E9%98%85%E5%BC%8F%E6%89%98%E7%AE%A1%E9%80%89%E9%A1%B9)  其他订阅式托管选项

[**GLM 模型** \\
\\
Z.AI Coding Plan 或通用 API 端点。](https://docs.openclaw.ai/zh-CN/providers/glm)

[**MiniMax** \\
\\
MiniMax Coding Plan OAuth 或 API key 访问。](https://docs.openclaw.ai/zh-CN/providers/minimax)

[**Qwen Cloud** \\
\\
Qwen Cloud 提供商表面，以及 Alibaba DashScope 和 Coding Plan 端点映射。](https://docs.openclaw.ai/zh-CN/providers/qwen)

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#opencode)  OpenCode

- 凭证：`OPENCODE_API_KEY`（或 `OPENCODE_ZEN_API_KEY`）
- Zen 运行时提供商：`opencode`
- Go 运行时提供商：`opencode-go`
- 示例模型：`opencode/claude-opus-4-6`、`opencode-go/kimi-k2.6`
- CLI：`openclaw onboard --auth-choice opencode-zen` 或 `openclaw onboard --auth-choice opencode-go`

```
{
  agents: { defaults: { model: { primary: "opencode/claude-opus-4-6" } } },
}
```

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#google-gemini%EF%BC%88api-key%EF%BC%89)  Google Gemini（API key）

- 提供商：`google`
- 凭证：`GEMINI_API_KEY`
- 可选轮换：`GEMINI_API_KEYS`、`GEMINI_API_KEY_1`、`GEMINI_API_KEY_2`、`GOOGLE_API_KEY` 回退，以及 `OPENCLAW_LIVE_GEMINI_KEY`（单一覆盖）
- 示例模型：`google/gemini-3.1-pro-preview`、`google/gemini-3-flash-preview`
- 兼容性：使用 `google/gemini-3.1-flash-preview` 的旧版 OpenClaw 配置会规范化为 `google/gemini-3-flash-preview`
- 别名：`google/gemini-3.1-pro` 可被接受，并规范化为 Google 实时 Gemini API ID：`google/gemini-3.1-pro-preview`
- CLI：`openclaw onboard --auth-choice gemini-api-key`
- 思考：`/think adaptive` 使用 Google 动态思考。Gemini 3/3.1 会省略固定的 `thinkingLevel`；Gemini 2.5 会发送 `thinkingBudget: -1`。
- 直接运行 Gemini 也接受 `agents.defaults.models["google/<model>"].params.cachedContent`（或旧版 `cached_content`），以转发提供商原生的 `cachedContents/...` 句柄；Gemini 缓存命中会显示为 OpenClaw `cacheRead`

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#google-vertex-%E5%92%8C-gemini-cli)  Google Vertex 和 Gemini CLI

- 提供商：`google-vertex`、`google-gemini-cli`
- 凭证：Vertex 使用 gcloud ADC；Gemini CLI 使用其 OAuth 流程

OpenClaw 中的 Gemini CLI OAuth 是非官方集成。一些用户报告称，在使用第三方客户端后遇到 Google 账号限制。如果你选择继续，请查看 Google 条款，并使用非关键账号。

Gemini CLI OAuth 作为内置 `google` 插件的一部分发布。

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/concepts/model-providers#)

Install Gemini CLI

- brew

- npm


```
brew install gemini-cli
```

```
npm install -g @google/gemini-cli
```

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/concepts/model-providers#)

Enable plugin

```
openclaw plugins enable google
```

3

[Navigate to header](https://docs.openclaw.ai/zh-CN/concepts/model-providers#)

Login

```
openclaw models auth login --provider google-gemini-cli --set-default
```

默认模型：`google-gemini-cli/gemini-3-flash-preview`。你 **不** 需要把客户端 ID 或密钥粘贴到 `openclaw.json`。CLI 登录流程会将令牌存储在 Gateway 网关主机上的认证配置文件中。

4

[Navigate to header](https://docs.openclaw.ai/zh-CN/concepts/model-providers#)

Set project (if needed)

如果登录后请求失败，请在 Gateway 网关主机上设置 `GOOGLE_CLOUD_PROJECT` 或 `GOOGLE_CLOUD_PROJECT_ID`。

Gemini CLI JSON 回复会从 `response` 解析；用量会回退到 `stats`，并将 `stats.cached` 规范化为 OpenClaw `cacheRead`。

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#z-ai-glm)  Z.AI (GLM)

- 提供商：`zai`
- 凭证：`ZAI_API_KEY`
- 示例模型：`zai/glm-5.1`
- CLI：`openclaw onboard --auth-choice zai-api-key`
  - 别名：`z.ai/*` 和 `z-ai/*` 会规范化为 `zai/*`
  - `zai-api-key` 会自动检测匹配的 Z.AI 端点；`zai-coding-global`、`zai-coding-cn`、`zai-global` 和 `zai-cn` 会强制使用特定接口

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#vercel-ai-gateway-%E7%BD%91%E5%85%B3)  Vercel AI Gateway 网关

- 提供商：`vercel-ai-gateway`
- 凭证：`AI_GATEWAY_API_KEY`
- 示例模型：`vercel-ai-gateway/anthropic/claude-opus-4.6`、`vercel-ai-gateway/moonshotai/kimi-k2.6`
- CLI：`openclaw onboard --auth-choice ai-gateway-api-key`

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#kilo-gateway-%E7%BD%91%E5%85%B3)  Kilo Gateway 网关

- 提供商：`kilocode`
- 凭证：`KILOCODE_API_KEY`
- 示例模型：`kilocode/kilo/auto`
- CLI：`openclaw onboard --auth-choice kilocode-api-key`
- 基础 URL：`https://api.kilo.ai/api/gateway/`
- 静态回退目录随 `kilocode/kilo/auto` 一起发布；实时 `https://api.kilo.ai/api/gateway/models` 设备发现可以进一步扩展运行时目录。
- `kilocode/kilo/auto` 背后的确切上游路由由 Kilo Gateway 网关负责，而不是在 OpenClaw 中硬编码。

有关设置详情，请参阅 [/providers/kilocode](https://docs.openclaw.ai/zh-CN/providers/kilocode)。

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#%E5%85%B6%E4%BB%96%E5%86%85%E7%BD%AE%E6%8F%90%E4%BE%9B%E5%95%86%E6%8F%92%E4%BB%B6)  其他内置提供商插件

| 提供商 | ID | 凭证环境变量 | 示例模型 |
| --- | --- | --- | --- |
| BytePlus | `byteplus` / `byteplus-plan` | `BYTEPLUS_API_KEY` | `byteplus-plan/ark-code-latest` |
| Cerebras | `cerebras` | `CEREBRAS_API_KEY` | `cerebras/zai-glm-4.7` |
| Cloudflare AI Gateway 网关 | `cloudflare-ai-gateway` | `CLOUDFLARE_AI_GATEWAY_API_KEY` | — |
| DeepInfra | `deepinfra` | `DEEPINFRA_API_KEY` | `deepinfra/deepseek-ai/DeepSeek-V3.2` |
| DeepSeek | `deepseek` | `DEEPSEEK_API_KEY` | `deepseek/deepseek-v4-flash` |
| GitHub Copilot | `github-copilot` | `COPILOT_GITHUB_TOKEN` / `GH_TOKEN` / `GITHUB_TOKEN` | — |
| Groq | `groq` | `GROQ_API_KEY` | — |
| Hugging Face Inference | `huggingface` | `HUGGINGFACE_HUB_TOKEN` 或 `HF_TOKEN` | `huggingface/deepseek-ai/DeepSeek-R1` |
| Kilo Gateway 网关 | `kilocode` | `KILOCODE_API_KEY` | `kilocode/kilo/auto` |
| Kimi Coding | `kimi` | `KIMI_API_KEY` 或 `KIMICODE_API_KEY` | `kimi/kimi-code` |
| MiniMax | `minimax` / `minimax-portal` | `MINIMAX_API_KEY` / `MINIMAX_OAUTH_TOKEN` | `minimax/MiniMax-M2.7` |
| Mistral | `mistral` | `MISTRAL_API_KEY` | `mistral/mistral-large-latest` |
| Moonshot | `moonshot` | `MOONSHOT_API_KEY` | `moonshot/kimi-k2.6` |
| NVIDIA | `nvidia` | `NVIDIA_API_KEY` | `nvidia/nvidia/nemotron-3-super-120b-a12b` |
| OpenRouter | `openrouter` | `OPENROUTER_API_KEY` | `openrouter/auto` |
| Qianfan | `qianfan` | `QIANFAN_API_KEY` | `qianfan/deepseek-v3.2` |
| Qwen Cloud | `qwen` | `QWEN_API_KEY` / `MODELSTUDIO_API_KEY` / `DASHSCOPE_API_KEY` | `qwen/qwen3.5-plus` |
| StepFun | `stepfun` / `stepfun-plan` | `STEPFUN_API_KEY` | `stepfun/step-3.5-flash` |
| Together | `together` | `TOGETHER_API_KEY` | `together/moonshotai/Kimi-K2.5` |
| Venice | `venice` | `VENICE_API_KEY` | — |
| Vercel AI Gateway 网关 | `vercel-ai-gateway` | `AI_GATEWAY_API_KEY` | `vercel-ai-gateway/anthropic/claude-opus-4.6` |
| Volcano Engine (Doubao) | `volcengine` / `volcengine-plan` | `VOLCANO_ENGINE_API_KEY` | `volcengine-plan/ark-code-latest` |
| xAI | `xai` | `XAI_API_KEY` | `xai/grok-4.3` |
| Xiaomi | `xiaomi` | `XIAOMI_API_KEY` | `xiaomi/mimo-v2-flash` |

#### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#%E9%9C%80%E8%A6%81%E4%BA%86%E8%A7%A3%E7%9A%84%E7%89%B9%E6%80%A7)  需要了解的特性

OpenRouter

仅在已验证的 `openrouter.ai` 路由上应用其应用归属标头和 Anthropic `cache_control` 标记。DeepSeek、Moonshot 和 ZAI 引用符合 OpenRouter 托管提示缓存的缓存 TTL 条件，但不会接收 Anthropic 缓存标记。作为代理式 OpenAI 兼容路径，它会跳过仅限原生 OpenAI 的形态调整（`serviceTier`、Responses `store`、提示缓存提示、OpenAI 推理兼容）。Gemini 支持的引用仅保留代理 Gemini 思维签名清理。

Kilo Gateway

Gemini 支持的引用遵循相同的代理 Gemini 清理路径；`kilocode/kilo/auto` 和其他不支持代理推理的引用会跳过代理推理注入。

MiniMax

API key 新手引导会写入显式的纯文本 M2.7 聊天模型定义；图像理解仍使用插件拥有的 `MiniMax-VL-01` 媒体提供商。

NVIDIA

模型 ID 使用 `nvidia/<vendor>/<model>` 命名空间（例如 `nvidia/nvidia/nemotron-...` 以及 `nvidia/moonshotai/kimi-k2.5`）；选择器会保留字面上的 `<provider>/<model-id>` 组合，而发送到 API 的规范键保持单前缀。

xAI

使用 xAI Responses 路径。`grok-4.3` 是内置的默认聊天模型。`/fast` 或 `params.fastMode: true` 会将 `grok-3`、`grok-3-mini`、`grok-4` 和 `grok-4-0709` 重写为其 `*-fast` 变体。`tool_stream` 默认开启；可通过 `agents.defaults.models["xai/<model>"].params.tool_stream=false` 禁用。

Cerebras

作为内置的 `cerebras` 提供商插件交付。GLM 使用 `zai-glm-4.7`；OpenAI 兼容的基础 URL 是 `https://api.cerebras.ai/v1`。

## [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#%E9%80%9A%E8%BF%87-models-providers-%E4%BD%BF%E7%94%A8%E6%8F%90%E4%BE%9B%E5%95%86%EF%BC%88%E8%87%AA%E5%AE%9A%E4%B9%89/%E5%9F%BA%E7%A1%80-url%EF%BC%89)  通过 `models.providers` 使用提供商（自定义/基础 URL）

使用 `models.providers`（或 `models.json`）添加 **自定义** 提供商或 OpenAI/Anthropic 兼容代理。下面许多内置提供商插件已经发布默认目录。仅当你想覆盖默认基础 URL、标头或模型列表时，才使用显式的 `models.providers.<id>` 条目。Gateway 网关模型能力检查也会读取显式的 `models.providers.<id>.models[]` 元数据。如果自定义或代理模型接受图像，请在该模型上设置 `input: ["text", "image"]`，以便 WebChat 和节点来源附件路径将图像作为原生模型输入传递，而不是作为纯文本媒体引用。

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#moonshot-ai%EF%BC%88kimi%EF%BC%89)  Moonshot AI（Kimi）

Moonshot 作为内置提供商插件交付。默认使用内置提供商，仅当需要覆盖基础 URL 或模型元数据时，才添加显式的 `models.providers.moonshot` 条目：

- 提供商：`moonshot`
- 凭证：`MOONSHOT_API_KEY`
- 示例模型：`moonshot/kimi-k2.6`
- CLI：`openclaw onboard --auth-choice moonshot-api-key` 或 `openclaw onboard --auth-choice moonshot-api-key-cn`

Kimi K2 模型 ID：

- `moonshot/kimi-k2.6`
- `moonshot/kimi-k2.5`
- `moonshot/kimi-k2-thinking`
- `moonshot/kimi-k2-thinking-turbo`
- `moonshot/kimi-k2-turbo`

```
{
  agents: {
    defaults: { model: { primary: "moonshot/kimi-k2.6" } },
  },
  models: {
    mode: "merge",
    providers: {
      moonshot: {
        baseUrl: "https://api.moonshot.ai/v1",
        apiKey: "${MOONSHOT_API_KEY}",
        api: "openai-completions",
        models: [{ id: "kimi-k2.6", name: "Kimi K2.6" }],
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#kimi-%E7%BC%96%E7%A8%8B)  Kimi 编程

Kimi Coding 使用 Moonshot AI 的 Anthropic 兼容端点：

- 提供商：`kimi`
- 凭证：`KIMI_API_KEY`
- 示例模型：`kimi/kimi-code`

```
{
  env: { KIMI_API_KEY: "sk-..." },
  agents: {
    defaults: { model: { primary: "kimi/kimi-code" } },
  },
}
```

旧版 `kimi/k2p5` 仍作为兼容模型 ID 被接受。

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#volcano-engine%EF%BC%88doubao%EF%BC%89)  Volcano Engine（Doubao）

Volcano Engine（火山引擎）在中国提供对 Doubao 和其他模型的访问。

- 提供商：`volcengine`（编码：`volcengine-plan`）
- 凭证：`VOLCANO_ENGINE_API_KEY`
- 示例模型：`volcengine-plan/ark-code-latest`
- CLI：`openclaw onboard --auth-choice volcengine-api-key`

```
{
  agents: {
    defaults: { model: { primary: "volcengine-plan/ark-code-latest" } },
  },
}
```

新手引导默认使用编码入口，但同时会注册通用的 `volcengine/*` 目录。在新手引导/配置模型选择器中，Volcengine 凭证选项会优先显示 `volcengine/*` 和 `volcengine-plan/*` 两类行。如果这些模型尚未加载，OpenClaw 会回退到未过滤的目录，而不是显示一个按提供商范围过滤后为空的选择器。

- 标准模型

- 编码模型（volcengine-plan）


- `volcengine/doubao-seed-1-8-251228`（Doubao Seed 1.8）
- `volcengine/doubao-seed-code-preview-251028`
- `volcengine/kimi-k2-5-260127`（Kimi K2.5）
- `volcengine/glm-4-7-251222`（GLM 4.7）
- `volcengine/deepseek-v3-2-251201`（DeepSeek V3.2 128K）

- `volcengine-plan/ark-code-latest`
- `volcengine-plan/doubao-seed-code`
- `volcengine-plan/kimi-k2.5`
- `volcengine-plan/kimi-k2-thinking`
- `volcengine-plan/glm-4.7`

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#byteplus%EF%BC%88%E5%9B%BD%E9%99%85%E7%89%88%EF%BC%89)  BytePlus（国际版）

BytePlus ARK 为国际用户提供与 Volcano Engine 相同模型的访问能力。

- 提供商：`byteplus`（编码：`byteplus-plan`）
- 凭证：`BYTEPLUS_API_KEY`
- 示例模型：`byteplus-plan/ark-code-latest`
- CLI：`openclaw onboard --auth-choice byteplus-api-key`

```
{
  agents: {
    defaults: { model: { primary: "byteplus-plan/ark-code-latest" } },
  },
}
```

新手引导默认使用编码入口，但同时会注册通用的 `byteplus/*` 目录。在新手引导/配置模型选择器中，BytePlus 凭证选项会优先显示 `byteplus/*` 和 `byteplus-plan/*` 两类行。如果这些模型尚未加载，OpenClaw 会回退到未过滤的目录，而不是显示一个按提供商范围过滤后为空的选择器。

- 标准模型

- 编码模型（byteplus-plan）


- `byteplus/seed-1-8-251228`（Seed 1.8）
- `byteplus/kimi-k2-5-260127`（Kimi K2.5）
- `byteplus/glm-4-7-251222`（GLM 4.7）

- `byteplus-plan/ark-code-latest`
- `byteplus-plan/doubao-seed-code`
- `byteplus-plan/kimi-k2.5`
- `byteplus-plan/kimi-k2-thinking`
- `byteplus-plan/glm-4.7`

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#synthetic)  Synthetic

Synthetic 通过 `synthetic` 提供商提供 Anthropic 兼容模型：

- 提供商：`synthetic`
- 凭证：`SYNTHETIC_API_KEY`
- 示例模型：`synthetic/hf:MiniMaxAI/MiniMax-M2.5`
- CLI：`openclaw onboard --auth-choice synthetic-api-key`

```
{
  agents: {
    defaults: { model: { primary: "synthetic/hf:MiniMaxAI/MiniMax-M2.5" } },
  },
  models: {
    mode: "merge",
    providers: {
      synthetic: {
        baseUrl: "https://api.synthetic.new/anthropic",
        apiKey: "${SYNTHETIC_API_KEY}",
        api: "anthropic-messages",
        models: [{ id: "hf:MiniMaxAI/MiniMax-M2.5", name: "MiniMax M2.5" }],
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#minimax)  MiniMax

MiniMax 通过 `models.providers` 配置，因为它使用自定义端点：

- MiniMax OAuth（全球）：`--auth-choice minimax-global-oauth`
- MiniMax OAuth（中国）：`--auth-choice minimax-cn-oauth`
- MiniMax API key（全球）：`--auth-choice minimax-global-api`
- MiniMax API key（中国）：`--auth-choice minimax-cn-api`
- 凭证：`minimax` 使用 `MINIMAX_API_KEY`；`minimax-portal` 使用 `MINIMAX_OAUTH_TOKEN` 或 `MINIMAX_API_KEY`

查看 [/providers/minimax](https://docs.openclaw.ai/zh-CN/providers/minimax) 了解设置详情、模型选项和配置片段。

在 MiniMax 的 Anthropic 兼容流式传输路径上，除非你显式设置，否则 OpenClaw 默认会禁用思考，并且 `/fast on` 会将 `MiniMax-M2.7` 重写为 `MiniMax-M2.7-highspeed`。

插件拥有的能力划分：

- 文本/聊天默认值保持在 `minimax/MiniMax-M2.7`
- 图像生成是 `minimax/image-01` 或 `minimax-portal/image-01`
- 图像理解是在两条 MiniMax 凭证路径上均由插件拥有的 `MiniMax-VL-01`
- Web 搜索保持在提供商 ID `minimax`

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#lm-studio)  LM Studio

LM Studio 作为内置提供商插件提供，并使用原生 API：

- 提供商：`lmstudio`
- 凭证：`LM_API_TOKEN`
- 默认推理基础 URL：`http://localhost:1234/v1`

然后设置一个模型（替换为 `http://localhost:1234/api/v1/models` 返回的任一 ID）：

```
{
  agents: {
    defaults: { model: { primary: "lmstudio/openai/gpt-oss-20b" } },
  },
}
```

OpenClaw 使用 LM Studio 的原生 `/api/v1/models` 和 `/api/v1/models/load` 进行设备发现 \+ 自动加载，并默认使用 `/v1/chat/completions` 进行推理。如果你希望由 LM Studio 的 JIT 加载、TTL 和自动驱逐来管理模型生命周期，请设置 `models.providers.lmstudio.params.preload: false`。查看 [/providers/lmstudio](https://docs.openclaw.ai/zh-CN/providers/lmstudio) 了解设置和故障排除。

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#ollama)  Ollama

Ollama 作为内置提供商插件提供，并使用 Ollama 的原生 API：

- 提供商：`ollama`
- 凭证：无需（本地服务器）
- 示例模型：`ollama/llama3.3`
- 安装： [https://ollama.com/download](https://ollama.com/download)

```
# Install Ollama, then pull a model:
ollama pull llama3.3
```

```
{
  agents: {
    defaults: { model: { primary: "ollama/llama3.3" } },
  },
}
```

当你通过 `OLLAMA_API_KEY` 选择启用时，OpenClaw 会在本地 `http://127.0.0.1:11434` 检测 Ollama，且内置提供商插件会将 Ollama 直接添加到 `openclaw onboard` 和模型选择器中。查看 [/providers/ollama](https://docs.openclaw.ai/zh-CN/providers/ollama) 了解新手引导、云端/本地模式和自定义配置。

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#vllm)  vLLM

vLLM 作为内置提供商插件提供，用于本地/自托管 OpenAI 兼容服务器：

- 提供商：`vllm`
- 凭证：可选（取决于你的服务器）
- 默认基础 URL：`http://127.0.0.1:8000/v1`

要在本地选择启用自动发现（如果你的服务器不强制凭证，任意值都可以）：

```
export VLLM_API_KEY="vllm-local"
```

然后设置一个模型（替换为 `/v1/models` 返回的任一 ID）：

```
{
  agents: {
    defaults: { model: { primary: "vllm/your-model-id" } },
  },
}
```

查看 [/providers/vllm](https://docs.openclaw.ai/zh-CN/providers/vllm) 了解详情。

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#sglang)  SGLang

SGLang 作为内置提供商插件提供，用于快速自托管 OpenAI 兼容服务器：

- 提供商：`sglang`
- 凭证：可选（取决于你的服务器）
- 默认基础 URL：`http://127.0.0.1:30000/v1`

要在本地选择启用自动发现（如果你的服务器不强制凭证，任意值都可以）：

```
export SGLANG_API_KEY="sglang-local"
```

然后设置一个模型（替换为 `/v1/models` 返回的任一 ID）：

```
{
  agents: {
    defaults: { model: { primary: "sglang/your-model-id" } },
  },
}
```

查看 [/providers/sglang](https://docs.openclaw.ai/zh-CN/providers/sglang) 了解详情。

### [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#%E6%9C%AC%E5%9C%B0%E4%BB%A3%E7%90%86%EF%BC%88lm-studio%E3%80%81vllm%E3%80%81litellm-%E7%AD%89%EF%BC%89)  本地代理（LM Studio、vLLM、LiteLLM 等）

示例（OpenAI 兼容）：

```
{
  agents: {
    defaults: {
      model: { primary: "lmstudio/my-local-model" },
      models: { "lmstudio/my-local-model": { alias: "Local" } },
    },
  },
  models: {
    providers: {
      lmstudio: {
        baseUrl: "http://localhost:1234/v1",
        apiKey: "${LM_API_TOKEN}",
        api: "openai-completions",
        timeoutSeconds: 300,
        models: [\
          {\
            id: "my-local-model",\
            name: "Local Model",\
            reasoning: false,\
            input: ["text"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 200000,\
            maxTokens: 8192,\
          },\
        ],
      },
    },
  },
}
```

默认可选字段

对于自定义提供商，`reasoning`、`input`、`cost`、`contextWindow` 和 `maxTokens` 都是可选的。省略时，OpenClaw 默认使用：

- `reasoning: false`
- `input: ["text"]`
- `cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 }`
- `contextWindow: 200000`
- `maxTokens: 8192`

建议：设置与你的代理/模型限制匹配的显式值。

代理路由塑形规则

- 对非原生端点上的 `api: "openai-completions"`（任何非空 `baseUrl` 且其主机不是 `api.openai.com`），OpenClaw 会强制设置 `compat.supportsDeveloperRole: false`，以避免提供商因不支持 `developer` 角色而返回 400 错误。
- 代理式 OpenAI 兼容路由还会跳过仅适用于原生 OpenAI 的请求塑形：没有 `service_tier`，没有 Responses `store`，没有 Completions `store`，没有提示缓存提示，没有 OpenAI 推理兼容载荷塑形，也没有隐藏的 OpenClaw 归因标头。
- 对于需要供应商特定字段的 OpenAI 兼容 Completions 代理，请设置 `agents.defaults.models["provider/model"].params.extra_body`（或 `extraBody`），以将额外 JSON 合并到出站请求体中。
- 对于 vLLM 聊天模板控制，请设置 `agents.defaults.models["provider/model"].params.chat_template_kwargs`。当会话思考级别关闭时，内置 vLLM 插件会自动为 `vllm/nemotron-3-*` 发送 `enable_thinking: false` 和 `force_nonempty_content: true`。
- 对于缓慢的本地模型或远程 LAN/tailnet 主机，请设置 `models.providers.<id>.timeoutSeconds`。这会扩展提供商模型 HTTP 请求处理，包括连接、标头、正文流式传输以及总的受保护 fetch 中止时间，而不会增加整个智能体运行时超时。
- 如果 `baseUrl` 为空/省略，OpenClaw 会保持默认的 OpenAI 行为（解析到 `api.openai.com`）。
- 为安全起见，在非原生 `openai-completions` 端点上，显式的 `compat.supportsDeveloperRole: true` 仍会被覆盖。
- 对非直连端点上的 `api: "anthropic-messages"`（canonical `anthropic` 以外的任何提供商，或自定义 `models.providers.anthropic.baseUrl` 且其主机不是公开 `api.anthropic.com` 端点），OpenClaw 会抑制隐式 Anthropic beta 标头，例如 `claude-code-20250219`、`interleaved-thinking-2025-05-14` 和 OAuth 标记，因此自定义 Anthropic 兼容代理不会拒绝不支持的 beta 标志。如果你的代理需要特定 beta 功能，请显式设置 `models.providers.<id>.headers["anthropic-beta"]`。

## [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#cli-%E7%A4%BA%E4%BE%8B)  CLI 示例

```
openclaw onboard --auth-choice opencode-zen
openclaw models set opencode/claude-opus-4-6
openclaw models list
```

另见： [配置](https://docs.openclaw.ai/zh-CN/gateway/configuration) 了解完整配置示例。

## [​](https://docs.openclaw.ai/zh-CN/concepts/model-providers\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

- [配置参考](https://docs.openclaw.ai/zh-CN/gateway/config-agents#agent-defaults) — 模型配置键
- [模型故障转移](https://docs.openclaw.ai/zh-CN/concepts/model-failover) — 回退链和重试行为
- [Models](https://docs.openclaw.ai/zh-CN/concepts/models) — 模型配置和别名
- [提供商](https://docs.openclaw.ai/zh-CN/providers) — 各提供商设置指南

[Models CLI](https://docs.openclaw.ai/zh-CN/concepts/models) [Model failover](https://docs.openclaw.ai/zh-CN/concepts/model-failover)

Ctrl+I