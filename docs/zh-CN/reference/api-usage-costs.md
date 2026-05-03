---
source_url: https://docs.openclaw.ai/zh-CN/reference/api-usage-costs
title: "API \u4f7f\u7528\u60c5\u51b5\u548c\u8d39\u7528 - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

技术参考

API 使用情况和费用

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [API 使用情况和费用](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#api-%E4%BD%BF%E7%94%A8%E6%83%85%E5%86%B5%E5%92%8C%E8%B4%B9%E7%94%A8)
- [费用显示在哪里（聊天 \+ CLI）](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#%E8%B4%B9%E7%94%A8%E6%98%BE%E7%A4%BA%E5%9C%A8%E5%93%AA%E9%87%8C%EF%BC%88%E8%81%8A%E5%A4%A9-%2B-cli%EF%BC%89)
- [如何发现密钥](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#%E5%A6%82%E4%BD%95%E5%8F%91%E7%8E%B0%E5%AF%86%E9%92%A5)
- [哪些功能会消耗密钥](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#%E5%93%AA%E4%BA%9B%E5%8A%9F%E8%83%BD%E4%BC%9A%E6%B6%88%E8%80%97%E5%AF%86%E9%92%A5)
- [1) 核心模型响应（聊天 + 工具）](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#1-%E6%A0%B8%E5%BF%83%E6%A8%A1%E5%9E%8B%E5%93%8D%E5%BA%94%EF%BC%88%E8%81%8A%E5%A4%A9-%2B-%E5%B7%A5%E5%85%B7%EF%BC%89)
- [2) 媒体理解（音频/图像/视频）](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#2-%E5%AA%92%E4%BD%93%E7%90%86%E8%A7%A3%EF%BC%88%E9%9F%B3%E9%A2%91%2F%E5%9B%BE%E5%83%8F%2F%E8%A7%86%E9%A2%91%EF%BC%89)
- [3) 图像和视频生成](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#3-%E5%9B%BE%E5%83%8F%E5%92%8C%E8%A7%86%E9%A2%91%E7%94%9F%E6%88%90)
- [4) 记忆嵌入 + 语义搜索](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#4-%E8%AE%B0%E5%BF%86%E5%B5%8C%E5%85%A5-%2B-%E8%AF%AD%E4%B9%89%E6%90%9C%E7%B4%A2)
- [5) Web 搜索工具](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#5-web-%E6%90%9C%E7%B4%A2%E5%B7%A5%E5%85%B7)
- [5) Web 抓取工具（Firecrawl）](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#5-web-%E6%8A%93%E5%8F%96%E5%B7%A5%E5%85%B7%EF%BC%88firecrawl%EF%BC%89)
- [6) Provider 使用情况快照（status/health）](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#6-provider-%E4%BD%BF%E7%94%A8%E6%83%85%E5%86%B5%E5%BF%AB%E7%85%A7%EF%BC%88status%2Fhealth%EF%BC%89)
- [7) 压缩保护总结](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#7-%E5%8E%8B%E7%BC%A9%E4%BF%9D%E6%8A%A4%E6%80%BB%E7%BB%93)
- [8) 模型扫描 / 探测](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#8-%E6%A8%A1%E5%9E%8B%E6%89%AB%E6%8F%8F-%2F-%E6%8E%A2%E6%B5%8B)
- [9) Talk（语音）](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#9-talk%EF%BC%88%E8%AF%AD%E9%9F%B3%EF%BC%89)
- [10) Skills（第三方 API）](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#10-skills%EF%BC%88%E7%AC%AC%E4%B8%89%E6%96%B9-api%EF%BC%89)
- [相关内容](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs\#api-%E4%BD%BF%E7%94%A8%E6%83%85%E5%86%B5%E5%92%8C%E8%B4%B9%E7%94%A8)  API 使用情况和费用

本文档列出了 **哪些功能会调用 API 密钥** 以及这些费用会显示在哪里。重点介绍 OpenClaw 中可能产生提供商使用量或付费 API 调用的功能。

## [​](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs\#%E8%B4%B9%E7%94%A8%E6%98%BE%E7%A4%BA%E5%9C%A8%E5%93%AA%E9%87%8C%EF%BC%88%E8%81%8A%E5%A4%A9-+-cli%EF%BC%89)  费用显示在哪里（聊天 \+ CLI）

**每个会话的费用快照**

- `/status` 会显示当前会话模型、上下文使用情况以及上一条响应的 token。
- 如果模型使用的是 **API 密钥身份验证**，`/status` 还会显示上一条回复的 **预估费用**。
- 如果实时会话元数据较少，`/status` 可以从最新的转录使用记录中恢复 token/缓存计数器以及当前活动运行时模型标签。现有的非零实时值仍然优先；当存储的总计缺失或更小时，基于提示词大小的转录总计也可以优先生效。

**每条消息的费用页脚**

- `/usage full` 会在每条回复后附加使用情况页脚，其中包括 **预估费用**（仅限 API 密钥）。
- `/usage tokens` 仅显示 token；订阅式 OAuth/token 和 CLI 流程会隐藏美元费用。
- Gemini CLI 说明：当 CLI 返回 JSON 输出时，OpenClaw 会从 `stats` 读取使用情况，将 `stats.cached` 规范化为 `cacheRead`，并在需要时根据 `stats.input_tokens - stats.cached` 推导输入 token。

Anthropic 说明：Anthropic 员工告诉我们，再次允许 OpenClaw 风格的 Claude CLI 使用，因此 OpenClaw 将 Claude CLI 复用和 `claude -p` 用法视为此集成中被认可的方式，除非 Anthropic 发布新的策略。Anthropic 仍未提供 OpenClaw 可在 `/usage full` 中显示的逐条消息美元费用估算。**CLI 使用窗口（provider 配额）**

- `openclaw status --usage` 和 `openclaw channels list` 会显示 provider 的 **使用窗口**（配额快照，而非逐条消息费用）。
- 面向用户的输出会在不同提供商之间统一为 `X% left`。
- 当前支持使用窗口的提供商有：Anthropic、GitHub Copilot、Gemini CLI、OpenAI Codex、MiniMax、小米和 z.ai。
- MiniMax 说明：其原始 `usage_percent` / `usagePercent` 字段表示剩余配额，因此 OpenClaw 会在显示前对其取反。如果存在基于计数的字段，则这些字段仍然优先。如果 provider 返回 `model_remains`，OpenClaw 会优先使用聊天模型条目，并在需要时根据时间戳推导窗口标签，同时在套餐标签中包含模型名称。
- 这些配额窗口的使用情况身份验证会在可用时来自 provider 专用钩子；否则，OpenClaw 会回退为从 auth 配置文件、环境变量或配置中匹配 OAuth/API 密钥凭证。

详情和示例请参阅 [Token 使用情况和费用](https://docs.openclaw.ai/zh-CN/reference/token-use)。

## [​](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs\#%E5%A6%82%E4%BD%95%E5%8F%91%E7%8E%B0%E5%AF%86%E9%92%A5)  如何发现密钥

OpenClaw 可以从以下位置获取凭证：

- **Auth 配置文件**（按智能体区分，存储在 `auth-profiles.json` 中）。
- **环境变量**（例如 `OPENAI_API_KEY`、`BRAVE_API_KEY`、`FIRECRAWL_API_KEY`）。
- **配置**（`models.providers.*.apiKey`、`plugins.entries.*.config.webSearch.apiKey`、`plugins.entries.firecrawl.config.webFetch.apiKey`、`memorySearch.*`、`talk.providers.*.apiKey`）。
- **Skills**（`skills.entries.<name>.apiKey`），它们可能会将密钥导出到 skill 进程环境中。

## [​](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs\#%E5%93%AA%E4%BA%9B%E5%8A%9F%E8%83%BD%E4%BC%9A%E6%B6%88%E8%80%97%E5%AF%86%E9%92%A5)  哪些功能会消耗密钥

### [​](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs\#1-%E6%A0%B8%E5%BF%83%E6%A8%A1%E5%9E%8B%E5%93%8D%E5%BA%94%EF%BC%88%E8%81%8A%E5%A4%A9-+-%E5%B7%A5%E5%85%B7%EF%BC%89)  1) 核心模型响应（聊天 + 工具）

每条回复或工具调用都会使用 **当前模型提供商**（OpenAI、Anthropic 等）。这是使用量和费用的主要来源。这也包括订阅式托管提供商，它们仍会在 OpenClaw 的本地 UI 之外计费，例如 **OpenAI Codex**、 **Alibaba Cloud Model Studio Coding Plan**、 **MiniMax Coding Plan**、 **Z.AI / GLM Coding Plan**，以及启用了 **Extra Usage** 的 Anthropic OpenClaw Claude 登录路径。有关定价配置，请参阅 [Models](https://docs.openclaw.ai/zh-CN/providers/models)；有关显示方式，请参阅 [Token 使用情况和费用](https://docs.openclaw.ai/zh-CN/reference/token-use)。

### [​](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs\#2-%E5%AA%92%E4%BD%93%E7%90%86%E8%A7%A3%EF%BC%88%E9%9F%B3%E9%A2%91/%E5%9B%BE%E5%83%8F/%E8%A7%86%E9%A2%91%EF%BC%89)  2) 媒体理解（音频/图像/视频）

在回复运行之前，入站媒体可能会被总结或转录。这会使用模型/提供商 API。

- 音频：OpenAI / Groq / Deepgram / DeepInfra / Google / Mistral。
- 图像：OpenAI / OpenRouter / Anthropic / DeepInfra / Google / MiniMax / Moonshot AI / Qwen / Z.AI。
- 视频：Google / Qwen / Moonshot AI。

请参阅 [媒体理解](https://docs.openclaw.ai/zh-CN/nodes/media-understanding)。

### [​](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs\#3-%E5%9B%BE%E5%83%8F%E5%92%8C%E8%A7%86%E9%A2%91%E7%94%9F%E6%88%90)  3) 图像和视频生成

共享生成能力也可能会消耗提供商密钥：

- 图像生成：OpenAI / Google / DeepInfra / fal / MiniMax
- 视频生成：DeepInfra / Qwen

当 `agents.defaults.imageGenerationModel` 未设置时，图像生成可以推断使用具备身份验证的 provider 默认值。视频生成当前需要显式设置 `agents.defaults.videoGenerationModel`，例如 `qwen/wan2.6-t2v`。请参阅 [图像生成](https://docs.openclaw.ai/zh-CN/tools/image-generation)、 [Qwen Cloud](https://docs.openclaw.ai/zh-CN/providers/qwen) 和 [Models](https://docs.openclaw.ai/zh-CN/concepts/models)。

### [​](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs\#4-%E8%AE%B0%E5%BF%86%E5%B5%8C%E5%85%A5-+-%E8%AF%AD%E4%B9%89%E6%90%9C%E7%B4%A2)  4) 记忆嵌入 + 语义搜索

当配置为远程提供商时，语义记忆搜索会使用 **嵌入 API**：

- `memorySearch.provider = "openai"` → OpenAI embeddings
- `memorySearch.provider = "gemini"` → Gemini embeddings
- `memorySearch.provider = "voyage"` → Voyage embeddings
- `memorySearch.provider = "mistral"` → Mistral embeddings
- `memorySearch.provider = "deepinfra"` → DeepInfra embeddings
- `memorySearch.provider = "lmstudio"` → LM Studio embeddings（本地/自托管）
- `memorySearch.provider = "ollama"` → Ollama embeddings（本地/自托管；通常不会产生托管 API 计费）
- 如果本地嵌入失败，可选回退到远程提供商

你可以通过设置 `memorySearch.provider = "local"` 保持本地运行（无 API 使用）。请参阅 [内存](https://docs.openclaw.ai/zh-CN/concepts/memory)。

### [​](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs\#5-web-%E6%90%9C%E7%B4%A2%E5%B7%A5%E5%85%B7)  5) Web 搜索工具

`web_search` 可能会根据你的提供商产生使用费用：

- **Brave Search API**：`BRAVE_API_KEY` 或 `plugins.entries.brave.config.webSearch.apiKey`
- **Exa**：`EXA_API_KEY` 或 `plugins.entries.exa.config.webSearch.apiKey`
- **Firecrawl**：`FIRECRAWL_API_KEY` 或 `plugins.entries.firecrawl.config.webSearch.apiKey`
- **Gemini（Google Search）**：`GEMINI_API_KEY` 或 `plugins.entries.google.config.webSearch.apiKey`
- **Grok（xAI）**：`XAI_API_KEY` 或 `plugins.entries.xai.config.webSearch.apiKey`
- **Kimi（Moonshot AI）**：`KIMI_API_KEY`、`MOONSHOT_API_KEY` 或 `plugins.entries.moonshot.config.webSearch.apiKey`
- **MiniMax Search**：`MINIMAX_CODE_PLAN_KEY`、`MINIMAX_CODING_API_KEY`、`MINIMAX_API_KEY` 或 `plugins.entries.minimax.config.webSearch.apiKey`
- **Ollama Web 搜索**：对于可访问且已登录的本地 Ollama host 无需密钥；直接 `https://ollama.com` 搜索使用 `OLLAMA_API_KEY`，而受身份验证保护的 host 可以复用普通 Ollama provider bearer auth
- **Perplexity Search API**：`PERPLEXITY_API_KEY`、`OPENROUTER_API_KEY` 或 `plugins.entries.perplexity.config.webSearch.apiKey`
- **Tavily**：`TAVILY_API_KEY` 或 `plugins.entries.tavily.config.webSearch.apiKey`
- **DuckDuckGo**：无密钥回退方案（无 API 计费，但非官方且基于 HTML）
- **SearXNG**：`SEARXNG_BASE_URL` 或 `plugins.entries.searxng.config.webSearch.baseUrl`（无密钥/自托管；无托管 API 计费）

旧版 `tools.web.search.*` provider 路径仍会通过临时兼容层加载，但它们已不再是推荐的配置界面。**Brave Search 免费额度：** 每个 Brave 套餐都包含每月可续期的 5 美元免费额度。Search 套餐价格为每 1,000 次请求 5 美元，因此该额度可覆盖每月 1,000 次免费请求。请在 Brave 控制台中设置使用上限，以避免意外费用。请参阅 [Web 工具](https://docs.openclaw.ai/zh-CN/tools/web)。

### [​](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs\#5-web-%E6%8A%93%E5%8F%96%E5%B7%A5%E5%85%B7%EF%BC%88firecrawl%EF%BC%89)  5) Web 抓取工具（Firecrawl）

当存在 API 密钥时，`web_fetch` 可以调用 **Firecrawl**：

- `FIRECRAWL_API_KEY` 或 `plugins.entries.firecrawl.config.webFetch.apiKey`

如果未配置 Firecrawl，该工具会回退为直接抓取加内置 `web-readability` 插件（无付费 API）。禁用 `plugins.entries.web-readability.enabled` 可跳过本地 Readability 提取。请参阅 [Web 工具](https://docs.openclaw.ai/zh-CN/tools/web)。

### [​](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs\#6-provider-%E4%BD%BF%E7%94%A8%E6%83%85%E5%86%B5%E5%BF%AB%E7%85%A7%EF%BC%88status/health%EF%BC%89)  6) Provider 使用情况快照（status/health）

某些状态命令会调用 **provider 使用情况端点**，以显示配额窗口或身份验证健康状态。
这些通常是低频调用，但仍会命中 provider API：

- `openclaw status --usage`
- `openclaw models status --json`

请参阅 [Models CLI](https://docs.openclaw.ai/zh-CN/cli/models)。

### [​](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs\#7-%E5%8E%8B%E7%BC%A9%E4%BF%9D%E6%8A%A4%E6%80%BB%E7%BB%93)  7) 压缩保护总结

压缩保护机制可以使用 **当前模型** 对会话历史进行总结，这在运行时会调用提供商 API。请参阅 [会话管理 \+ 压缩](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction)。

### [​](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs\#8-%E6%A8%A1%E5%9E%8B%E6%89%AB%E6%8F%8F-/-%E6%8E%A2%E6%B5%8B)  8) 模型扫描 / 探测

`openclaw models scan` 可以探测 OpenRouter 模型，并在启用探测时使用 `OPENROUTER_API_KEY`。请参阅 [Models CLI](https://docs.openclaw.ai/zh-CN/cli/models)。

### [​](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs\#9-talk%EF%BC%88%E8%AF%AD%E9%9F%B3%EF%BC%89)  9) Talk（语音）

配置后，Talk 模式可以调用 **ElevenLabs**：

- `ELEVENLABS_API_KEY` 或 `talk.providers.elevenlabs.apiKey`

请参阅 [Talk 模式](https://docs.openclaw.ai/zh-CN/nodes/talk)。

### [​](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs\#10-skills%EF%BC%88%E7%AC%AC%E4%B8%89%E6%96%B9-api%EF%BC%89)  10) Skills（第三方 API）

Skills 可以在 `skills.entries.<name>.apiKey` 中存储 `apiKey`。如果某个 skill 使用该密钥调用外部 API，则可能会根据该 skill 的提供商产生费用。请参阅 [Skills](https://docs.openclaw.ai/zh-CN/tools/skills)。

## [​](https://docs.openclaw.ai/zh-CN/reference/api-usage-costs\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

- [Token 使用情况和费用](https://docs.openclaw.ai/zh-CN/reference/token-use)
- [提示词缓存](https://docs.openclaw.ai/zh-CN/reference/prompt-caching)
- [使用情况跟踪](https://docs.openclaw.ai/zh-CN/concepts/usage-tracking)

[Token 使用量和费用](https://docs.openclaw.ai/zh-CN/reference/token-use) [会话记录整理](https://docs.openclaw.ai/zh-CN/reference/transcript-hygiene)

Ctrl+I