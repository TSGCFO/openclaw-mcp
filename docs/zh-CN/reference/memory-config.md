---
source_url: https://docs.openclaw.ai/zh-CN/reference/memory-config
title: "\u8bb0\u5fc6\u914d\u7f6e\u53c2\u8003 - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/zh-CN/reference/memory-config#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

记忆配置参考

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [提供商选择](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E6%8F%90%E4%BE%9B%E5%95%86%E9%80%89%E6%8B%A9)
- [自动检测顺序](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E8%87%AA%E5%8A%A8%E6%A3%80%E6%B5%8B%E9%A1%BA%E5%BA%8F)
- [自定义提供商 ID](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E8%87%AA%E5%AE%9A%E4%B9%89%E6%8F%90%E4%BE%9B%E5%95%86-id)
- [API 密钥解析](https://docs.openclaw.ai/zh-CN/reference/memory-config#api-%E5%AF%86%E9%92%A5%E8%A7%A3%E6%9E%90)
- [远程端点配置](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E8%BF%9C%E7%A8%8B%E7%AB%AF%E7%82%B9%E9%85%8D%E7%BD%AE)
- [提供商特定配置](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E6%8F%90%E4%BE%9B%E5%95%86%E7%89%B9%E5%AE%9A%E9%85%8D%E7%BD%AE)
- [内联嵌入超时](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E5%86%85%E8%81%94%E5%B5%8C%E5%85%A5%E8%B6%85%E6%97%B6)
- [混合搜索配置](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E6%B7%B7%E5%90%88%E6%90%9C%E7%B4%A2%E9%85%8D%E7%BD%AE)
- [完整示例](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E5%AE%8C%E6%95%B4%E7%A4%BA%E4%BE%8B)
- [其他记忆路径](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E5%85%B6%E4%BB%96%E8%AE%B0%E5%BF%86%E8%B7%AF%E5%BE%84)
- [多模态记忆（Gemini）](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E5%A4%9A%E6%A8%A1%E6%80%81%E8%AE%B0%E5%BF%86%EF%BC%88gemini%EF%BC%89)
- [嵌入缓存](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E5%B5%8C%E5%85%A5%E7%BC%93%E5%AD%98)
- [批量索引](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E6%89%B9%E9%87%8F%E7%B4%A2%E5%BC%95)
- [会话记忆搜索（实验性）](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E4%BC%9A%E8%AF%9D%E8%AE%B0%E5%BF%86%E6%90%9C%E7%B4%A2%EF%BC%88%E5%AE%9E%E9%AA%8C%E6%80%A7%EF%BC%89)
- [SQLite 向量加速（sqlite-vec）](https://docs.openclaw.ai/zh-CN/reference/memory-config#sqlite-%E5%90%91%E9%87%8F%E5%8A%A0%E9%80%9F%EF%BC%88sqlite-vec%EF%BC%89)
- [索引存储](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E7%B4%A2%E5%BC%95%E5%AD%98%E5%82%A8)
- [QMD 后端配置](https://docs.openclaw.ai/zh-CN/reference/memory-config#qmd-%E5%90%8E%E7%AB%AF%E9%85%8D%E7%BD%AE)
- [完整 QMD 示例](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E5%AE%8C%E6%95%B4-qmd-%E7%A4%BA%E4%BE%8B)
- [Dreaming](https://docs.openclaw.ai/zh-CN/reference/memory-config#dreaming)
- [用户设置](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E7%94%A8%E6%88%B7%E8%AE%BE%E7%BD%AE)
- [示例](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E7%A4%BA%E4%BE%8B)
- [相关](https://docs.openclaw.ai/zh-CN/reference/memory-config#%E7%9B%B8%E5%85%B3)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

本页列出 OpenClaw 记忆搜索的每一个配置开关。概念概览请参阅：

[**记忆概览** \\
\\
记忆的工作方式。](https://docs.openclaw.ai/zh-CN/concepts/memory)

[**内置引擎** \\
\\
默认 SQLite 后端。](https://docs.openclaw.ai/zh-CN/concepts/memory-builtin)

[**QMD 引擎** \\
\\
本地优先的 sidecar。](https://docs.openclaw.ai/zh-CN/concepts/memory-qmd)

[**记忆搜索** \\
\\
搜索流水线与调优。](https://docs.openclaw.ai/zh-CN/concepts/memory-search)

[**主动记忆** \\
\\
用于交互式会话的记忆子智能体。](https://docs.openclaw.ai/zh-CN/concepts/active-memory)

除非另有说明，所有记忆搜索设置都位于 `openclaw.json` 的 `agents.defaults.memorySearch` 下。

如果你要查找 **主动记忆** 功能开关和子智能体配置，它位于 `plugins.entries.active-memory` 下，而不是 `memorySearch`。主动记忆使用双门控模型：

1. 插件必须已启用并以当前智能体 ID 为目标
2. 请求必须是符合条件的交互式持久聊天会话

有关激活模型、插件自有配置、转录持久化和安全发布模式，请参阅 [主动记忆](https://docs.openclaw.ai/zh-CN/concepts/active-memory)。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E6%8F%90%E4%BE%9B%E5%95%86%E9%80%89%E6%8B%A9)  提供商选择

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `provider` | `string` | 自动检测 | 嵌入适配器 ID，例如 `bedrock`、`deepinfra`、`gemini`、`github-copilot`、`local`、`mistral`、`ollama`、`openai` 或 `voyage`；也可以是已配置的 `models.providers.<id>`，其 `api` 指向这些适配器之一 |
| `model` | `string` | 提供商默认值 | 嵌入模型名称 |
| `fallback` | `string` | `"none"` | 主适配器失败时使用的后备适配器 ID |
| `enabled` | `boolean` | `true` | 启用或禁用记忆搜索 |

### [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E8%87%AA%E5%8A%A8%E6%A3%80%E6%B5%8B%E9%A1%BA%E5%BA%8F)  自动检测顺序

未设置 `provider` 时，OpenClaw 会选择第一个可用项：

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/reference/memory-config#)

local

如果已配置 `memorySearch.local.modelPath` 且文件存在，则选择。

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/reference/memory-config#)

github-copilot

如果可以解析 GitHub Copilot 令牌（环境变量或认证配置文件），则选择。

3

[Navigate to header](https://docs.openclaw.ai/zh-CN/reference/memory-config#)

openai

如果可以解析 OpenAI 密钥，则选择。

4

[Navigate to header](https://docs.openclaw.ai/zh-CN/reference/memory-config#)

gemini

如果可以解析 Gemini 密钥，则选择。

5

[Navigate to header](https://docs.openclaw.ai/zh-CN/reference/memory-config#)

voyage

如果可以解析 Voyage 密钥，则选择。

6

[Navigate to header](https://docs.openclaw.ai/zh-CN/reference/memory-config#)

mistral

如果可以解析 Mistral 密钥，则选择。

7

[Navigate to header](https://docs.openclaw.ai/zh-CN/reference/memory-config#)

deepinfra

如果可以解析 DeepInfra 密钥，则选择。

8

[Navigate to header](https://docs.openclaw.ai/zh-CN/reference/memory-config#)

bedrock

如果 AWS SDK 凭证链可以解析（实例角色、访问密钥、配置文件、SSO、Web 身份或共享配置），则选择。

支持 `ollama`，但不会自动检测（请显式设置）。

### [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E8%87%AA%E5%AE%9A%E4%B9%89%E6%8F%90%E4%BE%9B%E5%95%86-id)  自定义提供商 ID

`memorySearch.provider` 可以指向自定义 `models.providers.<id>` 条目。OpenClaw 会解析该提供商的 `api` 所有者来确定嵌入适配器，同时保留自定义提供商 ID 用于端点、认证和模型前缀处理。这让多 GPU 或多主机设置可以把记忆嵌入专门分配给特定本地端点：

```
{
  models: {
    providers: {
      "ollama-5080": {
        api: "ollama",
        baseUrl: "http://gpu-box.local:11435",
        apiKey: "ollama-local",
        models: [{ id: "qwen3-embedding:0.6b" }],
      },
    },
  },
  agents: {
    defaults: {
      memorySearch: {
        provider: "ollama-5080",
        model: "qwen3-embedding:0.6b",
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#api-%E5%AF%86%E9%92%A5%E8%A7%A3%E6%9E%90)  API 密钥解析

远程嵌入需要 API 密钥。Bedrock 改用 AWS SDK 默认凭证链（实例角色、SSO、访问密钥）。

| 提供商 | 环境变量 | 配置键 |
| --- | --- | --- |
| Bedrock | AWS 凭证链 | 不需要 API 密钥 |
| DeepInfra | `DEEPINFRA_API_KEY` | `models.providers.deepinfra.apiKey` |
| Gemini | `GEMINI_API_KEY` | `models.providers.google.apiKey` |
| GitHub Copilot | `COPILOT_GITHUB_TOKEN`, `GH_TOKEN`, `GITHUB_TOKEN` | 通过设备登录使用认证配置文件 |
| Mistral | `MISTRAL_API_KEY` | `models.providers.mistral.apiKey` |
| Ollama | `OLLAMA_API_KEY`（占位符） | — |
| OpenAI | `OPENAI_API_KEY` | `models.providers.openai.apiKey` |
| Voyage | `VOYAGE_API_KEY` | `models.providers.voyage.apiKey` |

Codex OAuth 仅覆盖聊天/补全，不满足嵌入请求。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E8%BF%9C%E7%A8%8B%E7%AB%AF%E7%82%B9%E9%85%8D%E7%BD%AE)  远程端点配置

用于自定义 OpenAI 兼容端点或覆盖提供商默认值：

[​](https://docs.openclaw.ai/zh-CN/reference/memory-config#param-remote-base-url)

remote.baseUrl

string

自定义 API 基础 URL。

[​](https://docs.openclaw.ai/zh-CN/reference/memory-config#param-remote-api-key)

remote.apiKey

string

覆盖 API 密钥。

[​](https://docs.openclaw.ai/zh-CN/reference/memory-config#param-remote-headers)

remote.headers

object

额外 HTTP 标头（与提供商默认值合并）。

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "openai",
        model: "text-embedding-3-small",
        remote: {
          baseUrl: "https://api.example.com/v1/",
          apiKey: "YOUR_KEY",
        },
      },
    },
  },
}
```

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E6%8F%90%E4%BE%9B%E5%95%86%E7%89%B9%E5%AE%9A%E9%85%8D%E7%BD%AE)  提供商特定配置

Gemini

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `model` | `string` | `gemini-embedding-001` | 也支持 `gemini-embedding-2-preview` |
| `outputDimensionality` | `number` | `3072` | 对于 Embedding 2：768、1536 或 3072 |

更改模型或 `outputDimensionality` 会触发自动完整重建索引。

OpenAI 兼容输入类型

OpenAI 兼容嵌入端点可以选择使用提供商特定的 `input_type` 请求字段。这对于需要为查询和文档嵌入使用不同标签的非对称嵌入模型很有用。

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `inputType` | `string` | 未设置 | 查询和文档嵌入共享的 `input_type` |
| `queryInputType` | `string` | 未设置 | 查询时的 `input_type`；覆盖 `inputType` |
| `documentInputType` | `string` | 未设置 | 索引/文档 `input_type`；覆盖 `inputType` |

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "openai",
        remote: {
          baseUrl: "https://embeddings.example/v1",
          apiKey: "env:EMBEDDINGS_API_KEY",
        },
        model: "asymmetric-embedder",
        queryInputType: "query",
        documentInputType: "passage",
      },
    },
  },
}
```

更改这些值会影响提供商批量索引的嵌入缓存身份；当上游模型以不同方式处理这些标签时，应随后执行记忆重建索引。

Bedrock

### [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#bedrock-%E5%B5%8C%E5%85%A5%E9%85%8D%E7%BD%AE)  Bedrock 嵌入配置

Bedrock 使用 AWS SDK 默认凭证链，不需要 API 密钥。如果 OpenClaw 运行在带有已启用 Bedrock 的实例角色的 EC2 上，只需设置提供商和模型：

```
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "bedrock",
        model: "amazon.titan-embed-text-v2:0",
      },
    },
  },
}
```

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `model` | `string` | `amazon.titan-embed-text-v2:0` | 任意 Bedrock 嵌入模型 ID |
| `outputDimensionality` | `number` | 模型默认值 | 对于 Titan V2：256、512 或 1024 |

**支持的模型**（包含系列检测和维度默认值）：

| 模型 ID | 提供商 | 默认维度 | 可配置维度 |
| --- | --- | --- | --- |
| `amazon.titan-embed-text-v2:0` | Amazon | 1024 | 256、512、1024 |
| `amazon.titan-embed-text-v1` | Amazon | 1536 | — |
| `amazon.titan-embed-g1-text-02` | Amazon | 1536 | — |
| `amazon.titan-embed-image-v1` | Amazon | 1024 | — |
| `amazon.nova-2-multimodal-embeddings-v1:0` | Amazon | 1024 | 256、384、1024、3072 |
| `cohere.embed-english-v3` | Cohere | 1024 | — |
| `cohere.embed-multilingual-v3` | Cohere | 1024 | — |
| `cohere.embed-v4:0` | Cohere | 1536 | 256-1536 |
| `twelvelabs.marengo-embed-3-0-v1:0` | TwelveLabs | 512 | — |
| `twelvelabs.marengo-embed-2-7-v1:0` | TwelveLabs | 1024 | — |

带吞吐量后缀的变体（例如 `amazon.titan-embed-text-v1:2:8k`）继承基础模型的配置。**认证：** Bedrock 认证使用标准 AWS SDK 凭证解析顺序：

1. 环境变量（`AWS_ACCESS_KEY_ID` \+ `AWS_SECRET_ACCESS_KEY`）
2. SSO 令牌缓存
3. Web 身份令牌凭证
4. 共享凭证和配置文件
5. ECS 或 EC2 元数据凭证

区域从 `AWS_REGION`、`AWS_DEFAULT_REGION`、`amazon-bedrock` 提供商 `baseUrl` 中解析，或默认为 `us-east-1`。**IAM 权限：** IAM 角色或用户需要：

```
{
  "Effect": "Allow",
  "Action": "bedrock:InvokeModel",
  "Resource": "*"
}
```

为实现最小权限，请将 `InvokeModel` 作用域限定到特定模型：

```
arn:aws:bedrock:*::foundation-model/amazon.titan-embed-text-v2:0
```

本地（GGUF + node-llama-cpp）

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `local.modelPath` | `string` | 自动下载 | GGUF 模型文件路径 |
| `local.modelCacheDir` | `string` | node-llama-cpp 默认值 | 下载模型的缓存目录 |
| `local.contextSize` | `number | "auto"` | `4096` | 嵌入上下文的上下文窗口大小。4096 可覆盖典型分块（128–512 个 token），同时限制非权重 VRAM。在资源受限的主机上可降低到 1024–2048。`"auto"` 使用模型训练时的最大值，不建议用于 8B+ 模型（Qwen3-Embedding-8B：40 960 个 token → 约 32 GB VRAM，而 4096 时约 8.8 GB）。 |

默认模型：`embeddinggemma-300m-qat-Q8_0.gguf`（约 0.6 GB，自动下载）。源码检出仍然需要原生构建批准：先运行 `pnpm approve-builds`，再运行 `pnpm rebuild node-llama-cpp`。使用独立 CLI 验证 Gateway 网关使用的同一提供商路径：

```
openclaw memory status --deep --agent main
openclaw memory index --force --agent main
```

如果 `provider` 为 `auto`，只有当 `local.modelPath` 指向现有本地文件时才会选择 `local`。`hf:` 和 HTTP(S) 模型引用仍然可以通过 `provider: "local"` 显式使用，但在模型可在磁盘上使用之前，它们不会让 `auto` 选择本地。

### [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E5%86%85%E8%81%94%E5%B5%8C%E5%85%A5%E8%B6%85%E6%97%B6)  内联嵌入超时

[​](https://docs.openclaw.ai/zh-CN/reference/memory-config#param-sync-embedding-batch-timeout-seconds)

sync.embeddingBatchTimeoutSeconds

number

覆盖记忆索引期间内联嵌入批次的超时时间。未设置时使用提供商默认值：对于 `local`、`ollama` 和 `lmstudio` 等本地/自托管提供商为 600 秒，对于托管提供商为 120 秒。当本地 CPU 密集型嵌入批次运行正常但速度较慢时，可以增大此值。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E6%B7%B7%E5%90%88%E6%90%9C%E7%B4%A2%E9%85%8D%E7%BD%AE)  混合搜索配置

全部位于 `memorySearch.query.hybrid` 下：

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `enabled` | `boolean` | `true` | 启用混合 BM25 + 向量搜索 |
| `vectorWeight` | `number` | `0.7` | 向量分数的权重（0-1） |
| `textWeight` | `number` | `0.3` | BM25 分数的权重（0-1） |
| `candidateMultiplier` | `number` | `4` | 候选池大小倍数 |

- MMR（多样性）

- 时间衰减（新近度）


| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `mmr.enabled` | `boolean` | `false` | 启用 MMR 重排序 |
| `mmr.lambda` | `number` | `0.7` | 0 = 最大多样性，1 = 最大相关性 |

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `temporalDecay.enabled` | `boolean` | `false` | 启用新近度加权 |
| `temporalDecay.halfLifeDays` | `number` | `30` | 每 N 天分数减半 |

常青文件（`MEMORY.md`、`memory/` 中无日期的文件）永不衰减。

### [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E5%AE%8C%E6%95%B4%E7%A4%BA%E4%BE%8B)  完整示例

```
{
  agents: {
    defaults: {
      memorySearch: {
        query: {
          hybrid: {
            vectorWeight: 0.7,
            textWeight: 0.3,
            mmr: { enabled: true, lambda: 0.7 },
            temporalDecay: { enabled: true, halfLifeDays: 30 },
          },
        },
      },
    },
  },
}
```

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E5%85%B6%E4%BB%96%E8%AE%B0%E5%BF%86%E8%B7%AF%E5%BE%84)  其他记忆路径

| 键 | 类型 | 描述 |
| --- | --- | --- |
| `extraPaths` | `string[]` | 要索引的其他目录或文件 |

```
{
  agents: {
    defaults: {
      memorySearch: {
        extraPaths: ["../team-docs", "/srv/shared-notes"],
      },
    },
  },
}
```

路径可以是绝对路径，也可以是相对于工作区的路径。目录会被递归扫描以查找 `.md` 文件。符号链接处理取决于当前后端：内置引擎会忽略符号链接，而 QMD 遵循底层 QMD 扫描器行为。对于按智能体限定的跨智能体 transcript 搜索，请使用 `agents.list[].memorySearch.qmd.extraCollections`，而不是 `memory.qmd.paths`。这些额外集合遵循相同的 `{ path, name, pattern? }` 结构，但它们会按智能体合并，并且当路径指向当前工作区外部时，可以保留显式共享名称。如果同一个已解析路径同时出现在 `memory.qmd.paths` 和 `memorySearch.qmd.extraCollections` 中，QMD 会保留第一个条目并跳过重复项。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E5%A4%9A%E6%A8%A1%E6%80%81%E8%AE%B0%E5%BF%86%EF%BC%88gemini%EF%BC%89)  多模态记忆（Gemini）

使用 Gemini Embedding 2 将图片和音频与 Markdown 一起索引：

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `multimodal.enabled` | `boolean` | `false` | 启用多模态索引 |
| `multimodal.modalities` | `string[]` | — | `["image"]`、`["audio"]` 或 `["all"]` |
| `multimodal.maxFileBytes` | `number` | `10000000` | 索引的最大文件大小 |

仅适用于 `extraPaths` 中的文件。默认记忆根目录保持仅限 Markdown。需要 `gemini-embedding-2-preview`。`fallback` 必须为 `"none"`。

支持的格式：`.jpg`、`.jpeg`、`.png`、`.webp`、`.gif`、`.heic`、`.heif`（图像）；`.mp3`、`.wav`、`.ogg`、`.opus`、`.m4a`、`.aac`、`.flac`（音频）。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E5%B5%8C%E5%85%A5%E7%BC%93%E5%AD%98)  嵌入缓存

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `cache.enabled` | `boolean` | `false` | 在 SQLite 中缓存分块嵌入 |
| `cache.maxEntries` | `number` | `50000` | 最大缓存嵌入数量 |

在重新索引或转录更新期间，防止对未更改的文本重新生成嵌入。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E6%89%B9%E9%87%8F%E7%B4%A2%E5%BC%95)  批量索引

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `remote.nonBatchConcurrency` | `number` | `4` | 并行内联嵌入 |
| `remote.batch.enabled` | `boolean` | `false` | 启用批量嵌入 API |
| `remote.batch.concurrency` | `number` | `2` | 并行批处理作业 |
| `remote.batch.wait` | `boolean` | `true` | 等待批处理完成 |
| `remote.batch.pollIntervalMs` | `number` | — | 轮询间隔 |
| `remote.batch.timeoutMinutes` | `number` | — | 批处理超时 |

适用于 `openai`、`gemini` 和 `voyage`。对于大规模回填，OpenAI 批处理通常最快且成本最低。`remote.nonBatchConcurrency` 控制本地/自托管提供商使用的内联嵌入调用，以及提供商批处理 API 未启用时托管提供商使用的内联嵌入调用。Ollama 在非批量索引时默认值为 `1`，以避免压垮较小的本地主机；在更大的机器上可设置更高的值。这与 `sync.embeddingBatchTimeoutSeconds` 分开，后者控制内联嵌入调用的超时时间。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E4%BC%9A%E8%AF%9D%E8%AE%B0%E5%BF%86%E6%90%9C%E7%B4%A2%EF%BC%88%E5%AE%9E%E9%AA%8C%E6%80%A7%EF%BC%89)  会话记忆搜索（实验性）

索引会话转录，并通过 `memory_search` 展示：

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `experimental.sessionMemory` | `boolean` | `false` | 启用会话索引 |
| `sources` | `string[]` | `["memory"]` | 添加 `"sessions"` 以包含转录 |
| `sync.sessions.deltaBytes` | `number` | `100000` | 重新索引的字节阈值 |
| `sync.sessions.deltaMessages` | `number` | `50` | 重新索引的消息阈值 |

会话索引是选择启用的，并且会异步运行。结果可能略微过时。会话日志存储在磁盘上，因此应将文件系统访问视为信任边界。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#sqlite-%E5%90%91%E9%87%8F%E5%8A%A0%E9%80%9F%EF%BC%88sqlite-vec%EF%BC%89)  SQLite 向量加速（sqlite-vec）

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `store.vector.enabled` | `boolean` | `true` | 使用 sqlite-vec 进行向量查询 |
| `store.vector.extensionPath` | `string` | 内置 | 覆盖 sqlite-vec 路径 |

当 sqlite-vec 不可用时，OpenClaw 会自动回退到进程内余弦相似度。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E7%B4%A2%E5%BC%95%E5%AD%98%E5%82%A8)  索引存储

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `store.path` | `string` | `~/.openclaw/memory/{agentId}.sqlite` | 索引位置（支持 `{agentId}` 令牌） |
| `store.fts.tokenizer` | `string` | `unicode61` | FTS5 分词器（`unicode61` 或 `trigram`） |

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#qmd-%E5%90%8E%E7%AB%AF%E9%85%8D%E7%BD%AE)  QMD 后端配置

设置 `memory.backend = "qmd"` 以启用。所有 QMD 设置都位于 `memory.qmd` 下：

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `command` | `string` | `qmd` | QMD 可执行文件路径；当服务 `PATH` 与你的 shell 不同时，设置为绝对路径 |
| `searchMode` | `string` | `search` | 搜索命令：`search`、`vsearch`、`query` |
| `includeDefaultMemory` | `boolean` | `true` | 自动索引 `MEMORY.md` \+ `memory/**/*.md` |
| `paths[]` | `array` | — | 额外路径：`{ name, path, pattern? }` |
| `sessions.enabled` | `boolean` | `false` | 索引会话转录 |
| `sessions.retentionDays` | `number` | — | 转录保留时间 |
| `sessions.exportDir` | `string` | — | 导出目录 |

`searchMode: "search"` 仅使用词法/BM25。OpenClaw 不会为该模式运行语义向量就绪探测或 QMD embedding 维护，包括执行 `memory status --deep` 时；`vsearch` 和 `query` 仍然要求 QMD 向量就绪和 embeddings。OpenClaw 优先使用当前的 QMD collection 和 MCP 查询形态，但会在需要时尝试兼容的 collection pattern 标志和较旧的 MCP 工具名称，以保持较旧的 QMD 版本可用。当 QMD 声明支持多个 collection 过滤器时，同源 collection 会使用一个 QMD 进程进行搜索；较旧的 QMD 构建仍使用逐 collection 的兼容路径。同源意味着持久化记忆 collection 会分组在一起，而会话 transcript collection 仍保持为单独分组，因此来源多样化仍然同时包含两类输入。

QMD 模型覆盖保留在 QMD 侧，而不是 OpenClaw 配置中。如果你需要全局覆盖 QMD 的模型，请在 Gateway 网关运行时环境中设置环境变量，例如 `QMD_EMBED_MODEL`、`QMD_RERANK_MODEL` 和 `QMD_GENERATE_MODEL`。

更新计划

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `update.interval` | `string` | `5m` | 刷新间隔 |
| `update.debounceMs` | `number` | `15000` | 对文件更改进行 debounce |
| `update.onBoot` | `boolean` | `true` | 长驻 QMD 管理器打开时刷新；同时控制选择启用的启动刷新 |
| `update.startup` | `string` | `off` | 可选的 Gateway 网关启动刷新：`off`、`idle` 或 `immediate` |
| `update.startupDelayMs` | `number` | `120000` | 在 `startup: "idle"` 刷新运行前延迟 |
| `update.waitForBootSync` | `boolean` | `false` | 阻塞管理器打开，直到其初始刷新完成 |
| `update.embedInterval` | `string` | — | 单独的 embedding 节奏 |
| `update.commandTimeoutMs` | `number` | — | QMD 命令的超时时间 |
| `update.updateTimeoutMs` | `number` | — | QMD 更新操作的超时时间 |
| `update.embedTimeoutMs` | `number` | — | QMD embed 操作的超时时间 |

限制

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `limits.maxResults` | `number` | `6` | 最大搜索结果数 |
| `limits.maxSnippetChars` | `number` | — | 限制 snippet 长度 |
| `limits.maxInjectedChars` | `number` | — | 限制注入字符总数 |
| `limits.timeoutMs` | `number` | `4000` | 搜索超时时间 |

范围

控制哪些会话可以接收 QMD 搜索结果。架构与 [`session.sendPolicy`](https://docs.openclaw.ai/zh-CN/gateway/config-agents#session) 相同：

```
{
  memory: {
    qmd: {
      scope: {
        default: "deny",
        rules: [{ action: "allow", match: { chatType: "direct" } }],
      },
    },
  },
}
```

随附默认值允许 direct 和 channel 会话，同时仍拒绝 groups。默认仅限私信。`match.keyPrefix` 匹配规范化的会话键；`match.rawKeyPrefix` 匹配包含 `agent:<id>:` 的原始键。

引用

`memory.citations` 适用于所有后端：

| 值 | 行为 |
| --- | --- |
| `auto`（默认） | 在 snippets 中包含 `Source: <path#line>` 页脚 |
| `on` | 始终包含页脚 |
| `off` | 省略页脚（路径仍会在内部传递给智能体） |

QMD 启动刷新会在 Gateway 网关启动期间使用一次性子进程路径。当记忆搜索为交互式使用而打开时，长驻 QMD 管理器仍负责常规文件 watcher 和间隔 timer。

### [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E5%AE%8C%E6%95%B4-qmd-%E7%A4%BA%E4%BE%8B)  完整 QMD 示例

```
{
  memory: {
    backend: "qmd",
    citations: "auto",
    qmd: {
      includeDefaultMemory: true,
      update: { interval: "5m", debounceMs: 15000 },
      limits: { maxResults: 6, timeoutMs: 4000 },
      scope: {
        default: "deny",
        rules: [{ action: "allow", match: { chatType: "direct" } }],
      },
      paths: [{ name: "docs", path: "~/notes", pattern: "**/*.md" }],
    },
  },
}
```

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#dreaming)  Dreaming

Dreaming 配置在 `plugins.entries.memory-core.config.dreaming` 下，而不是 `agents.defaults.memorySearch` 下。Dreaming 作为一次定时 sweep 运行，并使用内部 light/deep/REM 阶段作为实现细节。关于概念行为和 slash commands，请参阅 [Dreaming](https://docs.openclaw.ai/zh-CN/concepts/dreaming)。

### [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E7%94%A8%E6%88%B7%E8%AE%BE%E7%BD%AE)  用户设置

| 键 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `enabled` | `boolean` | `false` | 完全启用或禁用 dreaming |
| `frequency` | `string` | `0 3 * * *` | 完整 dreaming sweep 的可选 cron 节奏 |
| `model` | `string` | 默认模型 | 可选的 Dream Diary 子智能体模型覆盖 |

### [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E7%A4%BA%E4%BE%8B)  示例

```
{
  plugins: {
    entries: {
      "memory-core": {
        subagent: {
          allowModelOverride: true,
          allowedModels: ["anthropic/claude-sonnet-4-6"],
        },
        config: {
          dreaming: {
            enabled: true,
            frequency: "0 3 * * *",
            model: "anthropic/claude-sonnet-4-6",
          },
        },
      },
    },
  },
}
```

- Dreaming 将机器状态写入 `memory/.dreams/`。
- Dreaming 将人类可读的叙事输出写入 `DREAMS.md`（或现有的 `dreams.md`）。
- `dreaming.model` 使用现有的插件子智能体信任门控；启用前请设置 `plugins.entries.memory-core.subagent.allowModelOverride: true`。
- 当配置的模型不可用时，Dream Diary 会使用会话默认模型重试一次。信任或 allowlist 失败会被记录，且不会静默重试。
- light/deep/REM 阶段策略和阈值属于内部行为，不是面向用户的配置。

## [​](https://docs.openclaw.ai/zh-CN/reference/memory-config\#%E7%9B%B8%E5%85%B3)  相关

- [配置参考](https://docs.openclaw.ai/zh-CN/gateway/configuration-reference)
- [记忆概览](https://docs.openclaw.ai/zh-CN/concepts/memory)
- [记忆搜索](https://docs.openclaw.ai/zh-CN/concepts/memory-search)

Ctrl+I