---
source_url: https://docs.openclaw.ai/zh-CN/providers/ollama
title: "Ollama - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/providers/ollama#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

提供商

Ollama

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [凭证规则](https://docs.openclaw.ai/zh-CN/providers/ollama#%E5%87%AD%E8%AF%81%E8%A7%84%E5%88%99)
- [入门指南](https://docs.openclaw.ai/zh-CN/providers/ollama#%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97)
- [非交互模式](https://docs.openclaw.ai/zh-CN/providers/ollama#%E9%9D%9E%E4%BA%A4%E4%BA%92%E6%A8%A1%E5%BC%8F)
- [云模型](https://docs.openclaw.ai/zh-CN/providers/ollama#%E4%BA%91%E6%A8%A1%E5%9E%8B)
- [模型发现（隐式 provider）](https://docs.openclaw.ai/zh-CN/providers/ollama#%E6%A8%A1%E5%9E%8B%E5%8F%91%E7%8E%B0%EF%BC%88%E9%9A%90%E5%BC%8F-provider%EF%BC%89)
- [视觉和图像描述](https://docs.openclaw.ai/zh-CN/providers/ollama#%E8%A7%86%E8%A7%89%E5%92%8C%E5%9B%BE%E5%83%8F%E6%8F%8F%E8%BF%B0)
- [配置](https://docs.openclaw.ai/zh-CN/providers/ollama#%E9%85%8D%E7%BD%AE)
- [常见配方](https://docs.openclaw.ai/zh-CN/providers/ollama#%E5%B8%B8%E8%A7%81%E9%85%8D%E6%96%B9)
- [模型选择](https://docs.openclaw.ai/zh-CN/providers/ollama#%E6%A8%A1%E5%9E%8B%E9%80%89%E6%8B%A9)
- [快速验证](https://docs.openclaw.ai/zh-CN/providers/ollama#%E5%BF%AB%E9%80%9F%E9%AA%8C%E8%AF%81)
- [Ollama Web 搜索](https://docs.openclaw.ai/zh-CN/providers/ollama#ollama-web-%E6%90%9C%E7%B4%A2)
- [高级配置](https://docs.openclaw.ai/zh-CN/providers/ollama#%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE)
- [故障排除](https://docs.openclaw.ai/zh-CN/providers/ollama#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4)
- [相关内容](https://docs.openclaw.ai/zh-CN/providers/ollama#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw 与 Ollama 的原生 API（`/api/chat`）集成，可用于托管云模型以及本地/自托管 Ollama 服务器。你可以通过三种模式使用 Ollama：通过可访问的 Ollama 主机使用 `Cloud + Local`，针对 `https://ollama.com` 使用 `Cloud only`，或针对可访问的 Ollama 主机使用 `Local only`。

**远程 Ollama 用户**：不要在 OpenClaw 中使用 `/v1` OpenAI 兼容 URL（`http://host:11434/v1`）。这会破坏工具调用，模型可能会把原始工具 JSON 当作纯文本输出。请改用原生 Ollama API URL：`baseUrl: "http://host:11434"`（不带 `/v1`）。

Ollama provider 配置使用 `baseUrl` 作为规范键名。为兼容 OpenAI SDK 风格的示例，OpenClaw 也接受 `baseURL`，但新配置应优先使用 `baseUrl`。

## [​](https://docs.openclaw.ai/zh-CN/providers/ollama\#%E5%87%AD%E8%AF%81%E8%A7%84%E5%88%99)  凭证规则

本地和 LAN 主机

本地和 LAN Ollama 主机不需要真实的 bearer token。OpenClaw 仅对 loopback、私有网络、`.local` 和裸主机名 Ollama base URL 使用本地 `ollama-local` 标记。

远程和 Ollama Cloud 主机

远程公共主机和 Ollama Cloud（`https://ollama.com`）需要通过 `OLLAMA_API_KEY`、凭证配置档或 provider 的 `apiKey` 提供真实凭证。

自定义 provider id

设置了 `api: "ollama"` 的自定义 provider id 遵循相同规则。例如，指向私有 LAN Ollama 主机的 `ollama-remote` provider 可以使用 `apiKey: "ollama-local"`，子智能体会通过 Ollama provider 钩子解析该标记，而不是将其视为缺失凭证。记忆搜索也可以将 `agents.defaults.memorySearch.provider` 设置为该自定义 provider id，以便嵌入使用匹配的 Ollama 端点。

凭证配置档

`auth-profiles.json` 存储 provider id 的凭证。将端点设置（`baseUrl`、`api`、model id、headers、timeouts）放在 `models.providers.<id>` 中。较旧的扁平凭证配置档文件，例如 `{ "ollama-windows": { "apiKey": "ollama-local" } }`，不是运行时格式；运行 `openclaw doctor --fix` 可将其重写为规范的 `ollama-windows:default` API-key 配置档并创建备份。该文件中的 `baseUrl` 是兼容性噪音，应移到 provider 配置中。

记忆嵌入范围

当 Ollama 用于记忆嵌入时，bearer 身份验证的作用域限定为声明它的主机：

- provider 级密钥只会发送到该 provider 的 Ollama 主机。
- `agents.*.memorySearch.remote.apiKey` 只会发送到其远程嵌入主机。
- 纯 `OLLAMA_API_KEY` 环境变量值会被视为 Ollama Cloud 约定，默认不会发送到本地或自托管主机。

## [​](https://docs.openclaw.ai/zh-CN/providers/ollama\#%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97)  入门指南

选择你偏好的设置方法和模式。

- 新手引导（推荐）

- 手动设置


\*\*最适合：\*\*最快完成可用的 Ollama 云端或本地设置。

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/ollama#)

运行新手引导

```
openclaw onboard
```

从 provider 列表中选择 **Ollama**。

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/ollama#)

选择你的模式

- **Cloud + Local** — 本地 Ollama 主机加上通过该主机路由的云模型
- **Cloud only** — 通过 `https://ollama.com` 使用托管 Ollama 模型
- **Local only** — 仅使用本地模型

3

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/ollama#)

选择模型

`Cloud only` 会提示输入 `OLLAMA_API_KEY` 并建议托管云端默认值。`Cloud + Local` 和 `Local only` 会要求提供 Ollama base URL，发现可用模型，并在所选本地模型尚不可用时自动拉取。若 Ollama 报告了已安装的 `:latest` 标签，例如 `gemma4:latest`，设置过程只会显示该已安装模型一次，而不会同时显示 `gemma4` 和 `gemma4:latest`，也不会再次拉取裸别名。`Cloud + Local` 还会检查该 Ollama 主机是否已登录以获取云端访问权限。

4

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/ollama#)

验证模型可用

```
openclaw models list --provider ollama
```

### [​](https://docs.openclaw.ai/zh-CN/providers/ollama\#%E9%9D%9E%E4%BA%A4%E4%BA%92%E6%A8%A1%E5%BC%8F)  非交互模式

```
openclaw onboard --non-interactive \
  --auth-choice ollama \
  --accept-risk
```

也可以指定自定义 base URL 或模型：

```
openclaw onboard --non-interactive \
  --auth-choice ollama \
  --custom-base-url "http://ollama-host:11434" \
  --custom-model-id "qwen3.5:27b" \
  --accept-risk
```

\*\*最适合：\*\*完全控制云端或本地设置。

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/ollama#)

选择云端或本地

- **Cloud + Local**：安装 Ollama，使用 `ollama signin` 登录，并通过该主机路由云端请求
- **Cloud only**：使用带有 `OLLAMA_API_KEY` 的 `https://ollama.com`
- **Local only**：从 [ollama.com/download](https://ollama.com/download) 安装 Ollama

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/ollama#)

拉取本地模型（仅本地）

```
ollama pull gemma4
# or
ollama pull gpt-oss:20b
# or
ollama pull llama3.3
```

3

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/ollama#)

为 OpenClaw 启用 Ollama

对于 `Cloud only`，使用你的真实 `OLLAMA_API_KEY`。对于由主机支持的设置，任何占位符值都可用：

```
# Cloud
export OLLAMA_API_KEY="your-ollama-api-key"

# Local-only
export OLLAMA_API_KEY="ollama-local"

# Or configure in your config file
openclaw config set models.providers.ollama.apiKey "OLLAMA_API_KEY"
```

4

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/ollama#)

检查并设置你的模型

```
openclaw models list
openclaw models set ollama/gemma4
```

或在配置中设置默认值：

```
{
  agents: {
    defaults: {
      model: { primary: "ollama/gemma4" },
    },
  },
}
```

## [​](https://docs.openclaw.ai/zh-CN/providers/ollama\#%E4%BA%91%E6%A8%A1%E5%9E%8B)  云模型

- Cloud + Local

- Cloud only

- Local only


`Cloud + Local` 使用可访问的 Ollama 主机作为本地模型和云模型的控制点。这是 Ollama 偏好的混合流程。在设置期间使用 **Cloud + Local**。OpenClaw 会提示输入 Ollama base URL，从该主机发现本地模型，并检查该主机是否已通过 `ollama signin` 登录以获取云端访问权限。当主机已登录时，OpenClaw 还会建议托管云端默认值，例如 `kimi-k2.5:cloud`、`minimax-m2.7:cloud` 和 `glm-5.1:cloud`。如果主机尚未登录，OpenClaw 会让设置保持为仅本地，直到你运行 `ollama signin`。

`Cloud only` 针对 Ollama 托管 API `https://ollama.com` 运行。在设置期间使用 **Cloud only**。OpenClaw 会提示输入 `OLLAMA_API_KEY`，设置 `baseUrl: "https://ollama.com"`，并填充托管云模型列表。此路径 **不** 需要本地 Ollama 服务器或 `ollama signin`。`openclaw onboard` 期间显示的云模型列表会从 `https://ollama.com/api/tags` 实时填充，上限为 500 条，因此选择器反映的是当前托管目录，而不是静态种子。如果 `ollama.com` 在设置时无法访问或未返回模型，OpenClaw 会回退到之前的硬编码建议，确保新手引导仍能完成。

在仅本地模式下，OpenClaw 会从已配置的 Ollama 实例发现模型。此路径适用于本地或自托管 Ollama 服务器。OpenClaw 当前建议 `gemma4` 作为本地默认值。

## [​](https://docs.openclaw.ai/zh-CN/providers/ollama\#%E6%A8%A1%E5%9E%8B%E5%8F%91%E7%8E%B0%EF%BC%88%E9%9A%90%E5%BC%8F-provider%EF%BC%89)  模型发现（隐式 provider）

当你设置 `OLLAMA_API_KEY`（或凭证配置档）并且 **没有** 定义 `models.providers.ollama` 或另一个带有 `api: "ollama"` 的自定义远程 provider 时，OpenClaw 会从 `http://127.0.0.1:11434` 的本地 Ollama 实例发现模型。

| 行为 | 详情 |
| --- | --- |
| 目录查询 | 查询 `/api/tags` |
| 能力检测 | 尽力使用 `/api/show` 查询来读取 `contextWindow`、展开后的 `num_ctx` Modelfile 参数，以及包括视觉/工具在内的能力 |
| 视觉模型 | `/api/show` 报告具有 `vision` 能力的模型会被标记为支持图像（`input: ["text", "image"]`），因此 OpenClaw 会自动将图像注入到提示词中 |
| 推理检测 | 可用时使用 `/api/show` 能力，包括 `thinking`；当 Ollama 省略能力时，回退到模型名称启发式规则（`r1`、`reasoning`、`think`） |
| Token 限制 | 将 `maxTokens` 设置为 OpenClaw 使用的默认 Ollama 最大 token 上限 |
| 成本 | 将所有成本设置为 `0` |

这样可以避免手动模型条目，同时让目录与本地 Ollama 实例保持一致。你可以在本地 `infer model run` 中使用完整引用，例如 `ollama/<pulled-model>:latest`；OpenClaw 会从 Ollama 的实时目录解析该已安装模型，而无需手写 `models.json` 条目。对于已登录的 Ollama 主机，一些 `:cloud` 模型可能在出现在 `/api/tags` 之前，就已经可以通过 `/api/chat`
和 `/api/show` 使用。当你显式选择完整的 `ollama/<model>:cloud` 引用时，OpenClaw 会用
`/api/show` 验证该精确缺失模型，并且只有在 Ollama 确认模型
元数据时，才将其加入运行时目录。拼写错误仍会作为未知模型失败，而不会被自动创建。

```
# See what models are available
ollama list
openclaw models list
```

对于避开完整智能体工具表面的窄文本生成冒烟测试，
请使用带有完整 Ollama 模型引用的本地 `infer model run`：

```
OLLAMA_API_KEY=ollama-local \
  openclaw infer model run \
    --local \
    --model ollama/llama3.2:latest \
    --prompt "Reply with exactly: pong" \
    --json
```

该路径仍使用 OpenClaw 已配置的 provider、凭证和原生 Ollama
传输协议，但不会启动聊天智能体轮次，也不会加载 MCP/工具上下文。如果
此路径成功而普通智能体回复失败，接下来请排查该模型的智能体
提示词/工具容量。对于同一精简路径上的窄视觉模型冒烟测试，请向
`infer model run` 添加一个或多个图像文件。这会将提示词和图像直接发送到
所选 Ollama 视觉模型，而不会加载聊天工具、记忆或先前
会话上下文：

```
OLLAMA_API_KEY=ollama-local \
  openclaw infer model run \
    --local \
    --model ollama/qwen2.5vl:7b \
    --prompt "Describe this image in one sentence." \
    --file ./photo.jpg \
    --json
```

`model run --file` 接受检测为 `image/*` 的文件，包括常见 PNG、
JPEG 和 WebP 输入。非图像文件会在调用 Ollama 之前被拒绝。
对于语音识别，请改用 `openclaw infer audio transcribe`。当你使用 `/model ollama/<model>` 切换会话时，OpenClaw 会将
其视为精确的用户选择。如果已配置的 Ollama `baseUrl`
不可访问，下一条回复会因 provider 错误而失败，而不是静默
从另一个已配置的回退模型回答。隔离的 cron 作业在启动智能体轮次前会额外执行一次本地安全检查。如果选定的模型解析到 local、私有网络或 `.local` Ollama 提供商，并且 `/api/tags` 无法访问，OpenClaw 会将该 cron 运行记录为 `skipped`，并在错误文本中包含选定的 `ollama/<model>`。端点预检会缓存 5 分钟，因此指向同一个已停止 Ollama 守护进程的多个 cron 作业不会全部发起失败的模型请求。使用以下命令针对本地 Ollama 实时验证本地文本路径、原生流路径和嵌入：

```
OPENCLAW_LIVE_TEST=1 OPENCLAW_LIVE_OLLAMA=1 OPENCLAW_LIVE_OLLAMA_WEB_SEARCH=0 \
  pnpm test:live -- extensions/ollama/ollama.live.test.ts
```

要添加新模型，只需用 Ollama 拉取它：

```
ollama pull mistral
```

新模型会被自动发现并可供使用。

如果你显式设置 `models.providers.ollama`，或配置自定义远程提供商，例如带有 `api: "ollama"` 的 `models.providers.ollama-cloud`，则会跳过自动发现，你必须手动定义模型。像 `http://127.0.0.2:11434` 这样的回环自定义提供商仍会被视为本地。请参阅下面的显式配置部分。

## [​](https://docs.openclaw.ai/zh-CN/providers/ollama\#%E8%A7%86%E8%A7%89%E5%92%8C%E5%9B%BE%E5%83%8F%E6%8F%8F%E8%BF%B0)  视觉和图像描述

内置的 Ollama 插件会将 Ollama 注册为支持图像的媒体理解提供商。这让 OpenClaw 可以通过本地或托管的 Ollama 视觉模型来路由显式图像描述请求和已配置的图像模型默认值。对于本地视觉，请拉取一个支持图像的模型：

```
ollama pull qwen2.5vl:7b
export OLLAMA_API_KEY="ollama-local"
```

然后使用 infer CLI 验证：

```
openclaw infer image describe \
  --file ./photo.jpg \
  --model ollama/qwen2.5vl:7b \
  --json
```

`--model` 必须是完整的 `<provider/model>` 引用。设置后，`openclaw infer image describe` 会直接运行该模型，而不是因为模型支持原生视觉而跳过描述。当你需要 OpenClaw 的图像理解提供商流程、已配置的 `agents.defaults.imageModel` 以及图像描述输出形状时，请使用 `infer image describe`。当你需要使用自定义提示词和一张或多张图像来原始探测多模态模型时，请使用 `infer model run --file`。要让 Ollama 成为入站媒体的默认图像理解模型，请配置 `agents.defaults.imageModel`：

```
{
  agents: {
    defaults: {
      imageModel: {
        primary: "ollama/qwen2.5vl:7b",
      },
    },
  },
}
```

优先使用完整的 `ollama/<model>` 引用。如果同一模型列在 `models.providers.ollama.models` 下，且带有 `input: ["text", "image"]`，并且没有其他已配置的图像提供商暴露相同的裸模型 ID，OpenClaw 也会把像 `qwen2.5vl:7b` 这样的裸 `imageModel` 引用规范化为 `ollama/qwen2.5vl:7b`。如果多个已配置的图像提供商拥有相同的裸 ID，请显式使用提供商前缀。慢速本地视觉模型可能需要比云模型更长的图像理解超时时间。当 Ollama 尝试在受限硬件上分配完整声明的视觉上下文时，它们也可能崩溃或停止。设置能力超时，并在你只需要普通图像描述轮次时，在模型条目上限制 `num_ctx`：

```
{
  models: {
    providers: {
      ollama: {
        models: [\
          {\
            id: "qwen2.5vl:7b",\
            name: "qwen2.5vl:7b",\
            input: ["text", "image"],\
            params: { num_ctx: 2048, keep_alive: "1m" },\
          },\
        ],
      },
    },
  },
  tools: {
    media: {
      image: {
        timeoutSeconds: 180,
        models: [{ provider: "ollama", model: "qwen2.5vl:7b", timeoutSeconds: 300 }],
      },
    },
  },
}
```

此超时适用于入站图像理解，也适用于智能体在轮次中可调用的显式 `image` 工具。提供商级别的 `models.providers.ollama.timeoutSeconds` 仍控制普通模型调用底层 Ollama HTTP 请求的保护超时。使用以下命令针对本地 Ollama 实时验证显式图像工具：

```
OPENCLAW_LIVE_TEST=1 OPENCLAW_LIVE_OLLAMA_IMAGE=1 \
  pnpm test:live -- src/agents/tools/image-tool.ollama.live.test.ts
```

如果你手动定义 `models.providers.ollama.models`，请将视觉模型标记为支持图像输入：

```
{
  id: "qwen2.5vl:7b",
  name: "qwen2.5vl:7b",
  input: ["text", "image"],
  contextWindow: 128000,
  maxTokens: 8192,
}
```

OpenClaw 会拒绝未标记为支持图像能力的模型的图像描述请求。使用隐式发现时，当 `/api/show` 报告视觉能力时，OpenClaw 会从 Ollama 读取此信息。

## [​](https://docs.openclaw.ai/zh-CN/providers/ollama\#%E9%85%8D%E7%BD%AE)  配置

- Basic (implicit discovery)

- Explicit (manual models)

- Custom base URL


最简单的仅本地启用路径是通过环境变量：

```
export OLLAMA_API_KEY="ollama-local"
```

如果设置了 `OLLAMA_API_KEY`，你可以在提供商条目中省略 `apiKey`，OpenClaw 会为可用性检查填充它。

当你需要托管云设置、Ollama 运行在另一台主机或端口上、想强制指定特定上下文窗口或模型列表，或想完全手动定义模型时，请使用显式配置。

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "https://ollama.com",
        apiKey: "OLLAMA_API_KEY",
        api: "ollama",
        models: [\
          {\
            id: "kimi-k2.5:cloud",\
            name: "kimi-k2.5:cloud",\
            reasoning: false,\
            input: ["text", "image"],\
            cost: { input: 0, output: 0, cacheRead: 0, cacheWrite: 0 },\
            contextWindow: 128000,\
            maxTokens: 8192\
          }\
        ]
      }
    }
  }
}
```

如果 Ollama 在不同主机或端口上运行（显式配置会禁用自动发现，因此需要手动定义模型）：

```
{
  models: {
    providers: {
      ollama: {
        apiKey: "ollama-local",
        baseUrl: "http://ollama-host:11434", // No /v1 - use native Ollama API URL
        api: "ollama", // Set explicitly to guarantee native tool-calling behavior
        timeoutSeconds: 300, // Optional: give cold local models longer to connect and stream
        models: [\
          {\
            id: "qwen3:32b",\
            name: "qwen3:32b",\
            params: {\
              keep_alive: "15m", // Optional: keep the model loaded between turns\
            },\
          },\
        ],
      },
    },
  },
}
```

不要向 URL 添加 `/v1`。`/v1` 路径使用 OpenAI 兼容模式，在该模式下工具调用不可靠。请使用不带路径后缀的基础 Ollama URL。

## [​](https://docs.openclaw.ai/zh-CN/providers/ollama\#%E5%B8%B8%E8%A7%81%E9%85%8D%E6%96%B9)  常见配方

将这些作为起点，并将模型 ID 替换为 `ollama list` 或 `openclaw models list --provider ollama` 中的准确名称。

Local model with auto-discovery

当 Ollama 与 Gateway 网关运行在同一台机器上，并且你希望 OpenClaw 自动发现已安装的模型时，请使用此配置。

```
ollama serve
ollama pull gemma4
export OLLAMA_API_KEY="ollama-local"
openclaw models list --provider ollama
openclaw models set ollama/gemma4
```

此路径会让配置保持最小化。除非你想手动定义模型，否则不要添加 `models.providers.ollama` 块。

LAN Ollama host with manual models

对于 LAN 主机，请使用原生 Ollama URL。不要添加 `/v1`。

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "http://gpu-box.local:11434",
        apiKey: "ollama-local",
        api: "ollama",
        timeoutSeconds: 300,
        contextWindow: 32768,
        maxTokens: 8192,
        models: [\
          {\
            id: "qwen3.5:9b",\
            name: "qwen3.5:9b",\
            reasoning: true,\
            input: ["text"],\
            params: {\
              num_ctx: 32768,\
              thinking: false,\
              keep_alive: "15m",\
            },\
          },\
        ],
      },
    },
  },
  agents: {
    defaults: {
      model: { primary: "ollama/qwen3.5:9b" },
    },
  },
}
```

`contextWindow` 是 OpenClaw 侧的上下文预算。`params.num_ctx` 会随请求发送给 Ollama。当你的硬件无法运行模型完整声明的上下文时，请保持二者一致。

Ollama Cloud only

当你不运行本地守护进程并希望直接使用托管的 Ollama 模型时，请使用此配置。

```
export OLLAMA_API_KEY="your-ollama-api-key"
```

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "https://ollama.com",
        apiKey: "OLLAMA_API_KEY",
        api: "ollama",
        models: [\
          {\
            id: "kimi-k2.5:cloud",\
            name: "kimi-k2.5:cloud",\
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
      model: { primary: "ollama/kimi-k2.5:cloud" },
    },
  },
}
```

Cloud plus local through a signed-in daemon

当本地或 LAN Ollama 守护进程已通过 `ollama signin` 登录，并且应同时服务本地模型和 `:cloud` 模型时，请使用此配置。

```
ollama signin
ollama pull gemma4
```

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "http://127.0.0.1:11434",
        apiKey: "ollama-local",
        api: "ollama",
        timeoutSeconds: 300,
        models: [\
          { id: "gemma4", name: "gemma4", input: ["text"] },\
          { id: "kimi-k2.5:cloud", name: "kimi-k2.5:cloud", input: ["text", "image"] },\
        ],
      },
    },
  },
  agents: {
    defaults: {
      model: {
        primary: "ollama/gemma4",
        fallbacks: ["ollama/kimi-k2.5:cloud"],
      },
    },
  },
}
```

Multiple Ollama hosts

当你有多个 Ollama 服务器时，请使用自定义提供商 ID。每个提供商都有自己的主机、模型、认证、超时和模型引用。

```
{
  models: {
    providers: {
      "ollama-fast": {
        baseUrl: "http://mini.local:11434",
        apiKey: "ollama-local",
        api: "ollama",
        contextWindow: 32768,
        models: [{ id: "gemma4", name: "gemma4", input: ["text"] }],
      },
      "ollama-large": {
        baseUrl: "http://gpu-box.local:11434",
        apiKey: "ollama-local",
        api: "ollama",
        timeoutSeconds: 420,
        contextWindow: 131072,
        maxTokens: 16384,
        models: [{ id: "qwen3.5:27b", name: "qwen3.5:27b", input: ["text"] }],
      },
    },
  },
  agents: {
    defaults: {
      model: {
        primary: "ollama-fast/gemma4",
        fallbacks: ["ollama-large/qwen3.5:27b"],
      },
    },
  },
}
```

当 OpenClaw 发送请求时，会剥离活动提供商前缀，因此 `ollama-large/qwen3.5:27b` 会以 `qwen3.5:27b` 到达 Ollama。

Lean local model profile

有些本地模型可以回答简单提示词，但难以处理完整的智能体工具表面。请先限制工具和上下文，再更改全局运行时设置。

```
{
  agents: {
    defaults: {
      experimental: {
        localModelLean: true,
      },
      model: { primary: "ollama/gemma4" },
    },
  },
  models: {
    providers: {
      ollama: {
        baseUrl: "http://127.0.0.1:11434",
        apiKey: "ollama-local",
        api: "ollama",
        contextWindow: 32768,
        models: [\
          {\
            id: "gemma4",\
            name: "gemma4",\
            input: ["text"],\
            params: { num_ctx: 32768 },\
            compat: { supportsTools: false },\
          },\
        ],
      },
    },
  },
}
```

仅当模型或服务器在工具 schema 上会可靠失败时，才使用 `compat.supportsTools: false`。它会用智能体能力换取稳定性。
`localModelLean` 会从智能体表面移除浏览器、cron 和消息工具，但它不会改变 Ollama 的运行时上下文或思考模式。对于会循环或把响应预算花在隐藏推理上的小型 Qwen 风格思考模型，请将它与显式的 `params.num_ctx` 和 `params.thinking: false` 搭配使用。

### [​](https://docs.openclaw.ai/zh-CN/providers/ollama\#%E6%A8%A1%E5%9E%8B%E9%80%89%E6%8B%A9)  模型选择

配置完成后，你的所有 Ollama 模型都可用：

```
{
  agents: {
    defaults: {
      model: {
        primary: "ollama/gpt-oss:20b",
        fallbacks: ["ollama/llama3.3", "ollama/qwen2.5-coder:32b"],
      },
    },
  },
}
```

也支持自定义 Ollama 提供商 ID。当模型引用使用活动提供商前缀时，例如 `ollama-spark/qwen3:32b`，OpenClaw 只会在调用 Ollama 前剥离该前缀，因此服务器会收到 `qwen3:32b`。对于较慢的本地模型，优先使用提供商范围的请求调优，然后再提高整个智能体运行时超时时间：

```
{
  models: {
    providers: {
      ollama: {
        timeoutSeconds: 300,
        models: [\
          {\
            id: "gemma4:26b",\
            name: "gemma4:26b",\
            params: { keep_alive: "15m" },\
          },\
        ],
      },
    },
  },
}
```

`timeoutSeconds` 适用于模型 HTTP 请求，包括连接建立、标头、正文流式传输，以及受保护抓取的总中止时间。`params.keep_alive` 会在原生 `/api/chat` 请求中作为顶层 `keep_alive` 转发给 Ollama；当首轮加载时间是瓶颈时，请按模型设置它。

### [​](https://docs.openclaw.ai/zh-CN/providers/ollama\#%E5%BF%AB%E9%80%9F%E9%AA%8C%E8%AF%81)  快速验证

```
# Ollama daemon visible to this machine
curl http://127.0.0.1:11434/api/tags

# OpenClaw catalog and selected model
openclaw models list --provider ollama
openclaw models status

# Direct model smoke
openclaw infer model run \
  --model ollama/gemma4 \
  --prompt "Reply with exactly: ok"
```

对于远程主机，将 `127.0.0.1` 替换为 `baseUrl` 中使用的主机。如果 `curl` 正常但 OpenClaw 不正常，请检查 Gateway 网关是否运行在另一台机器、容器或服务账号下。

## [​](https://docs.openclaw.ai/zh-CN/providers/ollama\#ollama-web-%E6%90%9C%E7%B4%A2)  Ollama Web 搜索

OpenClaw 支持将 **Ollama Web 搜索** 作为内置 `web_search` 提供商。

| 属性 | 详情 |
| --- | --- |
| 主机 | 使用你配置的 Ollama 主机（设置了 `models.providers.ollama.baseUrl` 时使用它，否则使用 `http://127.0.0.1:11434`）；`https://ollama.com` 会直接使用托管 API |
| 凭证 | 对已登录的本地 Ollama 主机无需密钥；对直接 `https://ollama.com` 搜索或受凭证保护的主机，使用 `OLLAMA_API_KEY` 或已配置的提供商凭证 |
| 要求 | 本地/自托管主机必须正在运行并已通过 `ollama signin` 登录；直接托管搜索需要 `baseUrl: "https://ollama.com"` 加真实的 Ollama API 密钥 |

在 `openclaw onboard` 或 `openclaw configure --section web` 期间选择 **Ollama Web 搜索**，或设置：

```
{
  tools: {
    web: {
      search: {
        provider: "ollama",
      },
    },
  },
}
```

要通过 Ollama Cloud 进行直接托管搜索：

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "https://ollama.com",
        apiKey: "OLLAMA_API_KEY",
        api: "ollama",
        models: [{ id: "kimi-k2.5:cloud", name: "kimi-k2.5:cloud", input: ["text"] }],
      },
    },
  },
  tools: {
    web: {
      search: { provider: "ollama" },
    },
  },
}
```

对于已登录的本地守护进程，OpenClaw 会使用该守护进程的 `/api/experimental/web_search` 代理。对于 `https://ollama.com`，它会直接调用托管的 `/api/web_search` 端点。

有关完整设置和行为详情，请参阅 [Ollama Web 搜索](https://docs.openclaw.ai/zh-CN/tools/ollama-search)。

## [​](https://docs.openclaw.ai/zh-CN/providers/ollama\#%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE)  高级配置

旧版 OpenAI 兼容模式

**在 OpenAI 兼容模式下，工具调用并不可靠。** 仅当你需要为代理使用 OpenAI 格式，且不依赖原生工具调用行为时，才使用此模式。

如果你需要改用 OpenAI 兼容端点（例如在只支持 OpenAI 格式的代理后面），请显式设置 `api: "openai-completions"`：

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "http://ollama-host:11434/v1",
        api: "openai-completions",
        injectNumCtxForOpenAICompat: true, // default: true
        apiKey: "ollama-local",
        models: [...]
      }
    }
  }
}
```

此模式可能无法同时支持流式传输和工具调用。你可能需要在模型配置中用 `params: { streaming: false }` 禁用流式传输。当 Ollama 使用 `api: "openai-completions"` 时，OpenClaw 默认会注入 `options.num_ctx`，这样 Ollama 就不会静默回退到 4096 上下文窗口。如果你的代理/上游拒绝未知的 `options` 字段，请禁用此行为：

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "http://ollama-host:11434/v1",
        api: "openai-completions",
        injectNumCtxForOpenAICompat: false,
        apiKey: "ollama-local",
        models: [...]
      }
    }
  }
}
```

上下文窗口

对于自动发现的模型，OpenClaw 会在可用时使用 Ollama 报告的上下文窗口，包括来自自定义 Modelfile 的更大 `PARAMETER num_ctx` 值。否则，它会回退到 OpenClaw 使用的默认 Ollama 上下文窗口。你可以为该 Ollama 提供商下的每个模型设置提供商级别的 `contextWindow`、`contextTokens` 和 `maxTokens` 默认值，然后在需要时按模型覆盖它们。`contextWindow` 是 OpenClaw 的提示词和压缩预算。原生 Ollama 请求会保持 `options.num_ctx` 未设置，除非你显式配置 `params.num_ctx`，因此 Ollama 可以应用自己的模型、`OLLAMA_CONTEXT_LENGTH` 或基于 VRAM 的默认值。要在不重建 Modelfile 的情况下限制或强制 Ollama 的每请求运行时上下文，请设置 `params.num_ctx`；无效、零、负数和非有限值会被忽略。OpenAI 兼容的 Ollama 适配器仍会默认根据已配置的 `params.num_ctx` 或 `contextWindow` 注入 `options.num_ctx`；如果你的上游拒绝 `options`，请用 `injectNumCtxForOpenAICompat: false` 禁用它。原生 Ollama 模型条目还接受 `params` 下的常见 Ollama 运行时选项，包括 `temperature`、`top_p`、`top_k`、`min_p`、`num_predict`、`stop`、`repeat_penalty`、`num_batch`、`num_thread` 和 `use_mmap`。OpenClaw 只转发 Ollama 请求键，因此不会把 OpenClaw 运行时参数（如 `streaming`）泄漏给 Ollama。使用 `params.think` 或 `params.thinking` 发送顶层 Ollama `think`；`false` 会为 Qwen 风格的思考模型禁用 API 级别思考。

```
{
  models: {
    providers: {
      ollama: {
        contextWindow: 32768,
        models: [\
          {\
            id: "llama3.3",\
            contextWindow: 131072,\
            maxTokens: 65536,\
            params: {\
              num_ctx: 32768,\
              temperature: 0.7,\
              top_p: 0.9,\
              thinking: false,\
            },\
          }\
        ]
      }
    }
  }
}
```

按模型设置的 `agents.defaults.models["ollama/<model>"].params.num_ctx` 也可用。如果两者都已配置，显式提供商模型条目优先于智能体默认值。

思考控制

对于原生 Ollama 模型，OpenClaw 会按 Ollama 期望的方式转发思考控制：顶层 `think`，而不是 `options.think`。如果自动发现模型的 `/api/show` 响应包含 `thinking` 能力，则会暴露 `/think low`、`/think medium`、`/think high` 和 `/think max`；非思考模型只会暴露 `/think off`。

```
openclaw agent --model ollama/gemma4 --thinking off
openclaw agent --model ollama/gemma4 --thinking low
```

你也可以设置模型默认值：

```
{
  agents: {
    defaults: {
      models: {
        "ollama/gemma4": {
          thinking: "low",
        },
      },
    },
  },
}
```

按模型设置的 `params.think` 或 `params.thinking` 可以为特定已配置模型禁用或强制 Ollama API 思考。当活动运行只有隐式默认 `off` 时，OpenClaw 会保留这些显式模型参数；非 off 运行时命令（如 `/think medium`）仍会覆盖活动运行。

推理模型

OpenClaw 默认会将名称中包含 `deepseek-r1`、`reasoning` 或 `think` 等内容的模型视为具备推理能力。

```
ollama pull deepseek-r1:32b
```

不需要额外配置。OpenClaw 会自动标记它们。

模型成本

Ollama 免费并在本地运行，因此所有模型成本都设置为 $0。这同时适用于自动发现和手动定义的模型。

记忆嵌入

内置 Ollama 插件会为 [memory search](https://docs.openclaw.ai/zh-CN/concepts/memory) 注册一个记忆嵌入提供商。它使用已配置的 Ollama 基础 URL 和 API 密钥，调用 Ollama 当前的 `/api/embed` 端点，并在可能时将多个记忆块批处理到一个 `input` 请求中。

| 属性 | 值 |
| --- | --- |
| 默认模型 | `nomic-embed-text` |
| 自动拉取 | 是 — 如果本地不存在该嵌入模型，会自动拉取 |

查询时嵌入会为需要或推荐检索前缀的模型使用检索前缀，包括 `nomic-embed-text`、`qwen3-embedding` 和 `mxbai-embed-large`。记忆文档批次保持原始格式，因此现有索引不需要格式迁移。要选择 Ollama 作为记忆搜索嵌入提供商：

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "ollama",
        remote: {
          // Default for Ollama. Raise on larger hosts if reindexing is too slow.
          nonBatchConcurrency: 1,
        },
      },
    },
  },
}
```

对于远程嵌入主机，请将凭证限定在该主机范围内：

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "ollama",
        model: "nomic-embed-text",
        remote: {
          baseUrl: "http://gpu-box.local:11434",
          apiKey: "ollama-local",
          nonBatchConcurrency: 2,
        },
      },
    },
  },
}
```

流式传输配置

OpenClaw 的 Ollama 集成默认使用 **原生 Ollama API**（`/api/chat`），它完全支持同时进行流式传输和工具调用。不需要特殊配置。对于原生 `/api/chat` 请求，OpenClaw 还会将思考控制直接转发给 Ollama：`/think off` 和 `openclaw agent --thinking off` 会发送顶层 `think: false`，除非配置了显式的模型 `params.think`/`params.thinking` 值；而 `/think low|medium|high` 会发送匹配的顶层 `think` 强度字符串。`/think max` 会映射到 Ollama 的最高原生强度，即 `think: "high"`。

如果你需要使用 OpenAI 兼容端点，请参见上面的“旧版 OpenAI 兼容模式”部分。在该模式下，流式传输和工具调用可能无法同时工作。

## [​](https://docs.openclaw.ai/zh-CN/providers/ollama\#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4)  故障排除

WSL2 崩溃循环（反复重启）

在带有 NVIDIA/CUDA 的 WSL2 上，官方 Ollama Linux 安装程序会创建一个带有 `Restart=always` 的 `ollama.service` systemd 单元。如果该服务自动启动，并在 WSL2 启动期间加载由 GPU 支持的模型，Ollama 可能会在模型加载时固定主机内存。Hyper-V 内存回收并不总是能回收这些固定页面，因此 Windows 可能会终止 WSL2 VM，systemd 又会再次启动 Ollama，循环随之重复。常见证据：

- Windows 侧反复出现 WSL2 重启或终止
- WSL2 启动后不久，`app.slice` 或 `ollama.service` 中 CPU 占用较高
- 来自 systemd 的 SIGTERM，而不是 Linux OOM-killer 事件

当 OpenClaw 检测到 WSL2、启用了带有 `Restart=always` 的 `ollama.service`，并且可见 CUDA 标记时，会记录启动警告。缓解措施：

```
sudo systemctl disable ollama
```

在 Windows 侧将以下内容添加到 `%USERPROFILE%\.wslconfig`，然后运行 `wsl --shutdown`：

```
[experimental]
autoMemoryReclaim=disabled
```

在 Ollama 服务环境中设置更短的 keep-alive，或者只在需要时手动启动 Ollama：

```
export OLLAMA_KEEP_ALIVE=5m
ollama serve
```

参见 [ollama/ollama#11317](https://github.com/ollama/ollama/issues/11317)。

未检测到 Ollama

确保 Ollama 正在运行，并且你设置了 `OLLAMA_API_KEY`（或凭证配置文件），且 **没有** 定义显式的 `models.providers.ollama` 条目：

```
ollama serve
```

验证 API 是否可访问：

```
curl http://localhost:11434/api/tags
```

没有可用模型

如果你的模型未列出，请在本地拉取该模型，或在 `models.providers.ollama` 中显式定义它。

```
ollama list  # 查看已安装内容
ollama pull gemma4
ollama pull gpt-oss:20b
ollama pull llama3.3     # 或另一个模型
```

连接被拒绝

检查 Ollama 是否在正确端口上运行：

```
# 检查 Ollama 是否正在运行
ps aux | grep ollama

# 或重启 Ollama
ollama serve
```

远程主机可通过 curl 使用，但 OpenClaw 不行

从运行 Gateway 网关的同一台机器和运行时进行验证：

```
openclaw gateway status --deep
curl http://ollama-host:11434/api/tags
```

常见原因：

- `baseUrl` 指向 `localhost`，但 Gateway 网关在 Docker 中或另一台主机上运行。
- URL 使用 `/v1`，这会选择 OpenAI 兼容行为，而不是原生 Ollama。
- 远程主机需要在 Ollama 侧更改防火墙或 LAN 绑定。
- 模型存在于你的笔记本电脑守护进程上，但不在远程守护进程上。

模型将工具 JSON 输出为文本

这通常意味着提供商正在使用 OpenAI 兼容模式，或者模型无法处理工具架构。优先使用原生 Ollama 模式：

```
{
  models: {
    providers: {
      ollama: {
        baseUrl: "http://ollama-host:11434",
        api: "ollama",
      },
    },
  },
}
```

如果小型本地模型在工具架构上仍然失败，请在该模型条目上设置 `compat.supportsTools: false`，然后重新测试。

Kimi 或 GLM 返回乱码符号

Hosted Kimi/GLM 响应如果是很长的非语言符号串，会被视为失败的提供商输出，而不是成功的助手回答。这样正常的重试、回退或错误处理就能接管，而不会把损坏文本持久化到会话中。如果反复发生，请捕获原始模型名称、当前会话文件，以及本次运行使用的是 `Cloud + Local` 还是 `Cloud only`，然后尝试一个新的会话和一个回退模型：

```
openclaw infer model run --model ollama/kimi-k2.5:cloud --prompt "Reply with exactly: ok" --json
openclaw models set ollama/gemma4
```

冷启动本地模型超时

大型本地模型在流式传输开始前可能需要很长的首次加载时间。将超时范围限定到 Ollama 提供商，并且可以选择让 Ollama 在轮次之间保持模型已加载：

```
{
  models: {
    providers: {
      ollama: {
        timeoutSeconds: 300,
        models: [\
          {\
            id: "gemma4:26b",\
            name: "gemma4:26b",\
            params: { keep_alive: "15m" },\
          },\
        ],
      },
    },
  },
}
```

如果主机本身接受连接很慢，`timeoutSeconds` 也会延长此提供商受保护的 Undici 连接超时。

大上下文模型太慢或内存不足

许多 Ollama 模型声明的上下文大于你的硬件可以舒适运行的范围。原生 Ollama 会使用 Ollama 自身的运行时上下文默认值，除非你设置 `params.num_ctx`。当你想要可预测的首个 token 延迟时，请同时限制 OpenClaw 的预算和 Ollama 的请求上下文：

```
{
  models: {
    providers: {
      ollama: {
        contextWindow: 32768,
        maxTokens: 8192,
        models: [\
          {\
            id: "qwen3.5:9b",\
            name: "qwen3.5:9b",\
            params: { num_ctx: 32768, thinking: false },\
          },\
        ],
      },
    },
  },
}
```

如果 OpenClaw 发送了过多提示词，请先降低 `contextWindow`。如果 Ollama 正在加载对这台机器来说过大的运行时上下文，请降低 `params.num_ctx`。如果生成运行时间过长，请降低 `maxTokens`。

更多帮助： [故障排除](https://docs.openclaw.ai/zh-CN/help/troubleshooting) 和 [常见问题](https://docs.openclaw.ai/zh-CN/help/faq)。

## [​](https://docs.openclaw.ai/zh-CN/providers/ollama\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

[**模型提供商** \\
\\
所有提供商、模型引用和故障转移行为的概览。](https://docs.openclaw.ai/zh-CN/concepts/model-providers)

[**模型选择** \\
\\
如何选择和配置模型。](https://docs.openclaw.ai/zh-CN/concepts/models)

[**Ollama Web 搜索** \\
\\
Ollama 驱动的 Web 搜索的完整设置和行为详情。](https://docs.openclaw.ai/zh-CN/tools/ollama-search)

[**配置** \\
\\
完整配置参考。](https://docs.openclaw.ai/zh-CN/gateway/configuration)

[OpenCode](https://docs.openclaw.ai/zh-CN/providers/opencode) [OpenAI](https://docs.openclaw.ai/zh-CN/providers/openai)

Ctrl+I