---
source_url: https://docs.openclaw.ai/zh-CN/providers/qwen
title: "Qwen - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/providers/qwen#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

提供商

Qwen

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [入门指南](https://docs.openclaw.ai/zh-CN/providers/qwen#%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97)
- [计划类型和端点](https://docs.openclaw.ai/zh-CN/providers/qwen#%E8%AE%A1%E5%88%92%E7%B1%BB%E5%9E%8B%E5%92%8C%E7%AB%AF%E7%82%B9)
- [内置目录](https://docs.openclaw.ai/zh-CN/providers/qwen#%E5%86%85%E7%BD%AE%E7%9B%AE%E5%BD%95)
- [思考控制](https://docs.openclaw.ai/zh-CN/providers/qwen#%E6%80%9D%E8%80%83%E6%8E%A7%E5%88%B6)
- [多模态附加功能](https://docs.openclaw.ai/zh-CN/providers/qwen#%E5%A4%9A%E6%A8%A1%E6%80%81%E9%99%84%E5%8A%A0%E5%8A%9F%E8%83%BD)
- [高级配置](https://docs.openclaw.ai/zh-CN/providers/qwen#%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE)
- [相关](https://docs.openclaw.ai/zh-CN/providers/qwen#%E7%9B%B8%E5%85%B3)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

**Qwen OAuth 已移除。** 使用 `portal.qwen.ai` 端点的免费层 OAuth 集成
（`qwen-portal`）已不再可用。
背景请参见 [Issue #49557](https://github.com/openclaw/openclaw/issues/49557)。

OpenClaw 现在将 Qwen 作为一等内置提供商处理，其规范 ID 为
`qwen`。该内置提供商面向 Qwen Cloud / Alibaba DashScope 和
Coding Plan 端点，并保留旧版 `modelstudio` ID 作为兼容别名继续可用。

- 提供商：`qwen`
- 首选环境变量：`QWEN_API_KEY`
- 兼容性也接受：`MODELSTUDIO_API_KEY`、`DASHSCOPE_API_KEY`
- API 风格：OpenAI 兼容

如果你想使用 `qwen3.6-plus`，优先选择\*\*标准（按量付费）\*\*端点。
Coding Plan 支持可能落后于公开目录。

## [​](https://docs.openclaw.ai/zh-CN/providers/qwen\#%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97)  入门指南

选择你的计划类型并按照设置步骤操作。

- Coding Plan（订阅）

- 标准（按量付费）


**最适合：** 通过 Qwen Coding Plan 获得基于订阅的访问。

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/qwen#)

获取你的 API key

从 [home.qwencloud.com/api-keys](https://home.qwencloud.com/api-keys) 创建或复制 API key。

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/qwen#)

运行新手引导

对于 **Global** 端点：

```
openclaw onboard --auth-choice qwen-api-key
```

对于 **China** 端点：

```
openclaw onboard --auth-choice qwen-api-key-cn
```

3

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/qwen#)

设置默认模型

```
{
  agents: {
    defaults: {
      model: { primary: "qwen/qwen3.5-plus" },
    },
  },
}
```

4

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/qwen#)

验证模型可用

```
openclaw models list --provider qwen
```

旧版 `modelstudio-*` auth-choice ID 和 `modelstudio/...` 模型引用仍可作为兼容别名使用，
但新的设置流程应优先使用规范的 `qwen-*` auth-choice ID 和 `qwen/...` 模型引用。
如果你定义了一个精确的自定义 `models.providers.modelstudio` 条目，并带有另一个 `api`
值，则该自定义提供商会拥有 `modelstudio/...` 引用，而不是 Qwen 兼容别名。

**最适合：** 通过标准 Model Studio 端点获得按量付费访问，包括可能无法在 Coding Plan 上使用的 `qwen3.6-plus` 等模型。

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/qwen#)

获取你的 API key

从 [home.qwencloud.com/api-keys](https://home.qwencloud.com/api-keys) 创建或复制 API key。

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/qwen#)

运行新手引导

对于 **Global** 端点：

```
openclaw onboard --auth-choice qwen-standard-api-key
```

对于 **China** 端点：

```
openclaw onboard --auth-choice qwen-standard-api-key-cn
```

3

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/qwen#)

设置默认模型

```
{
  agents: {
    defaults: {
      model: { primary: "qwen/qwen3.5-plus" },
    },
  },
}
```

4

[Navigate to header](https://docs.openclaw.ai/zh-CN/providers/qwen#)

验证模型可用

```
openclaw models list --provider qwen
```

旧版 `modelstudio-*` auth-choice ID 和 `modelstudio/...` 模型引用仍可作为兼容别名使用，
但新的设置流程应优先使用规范的 `qwen-*` auth-choice ID 和 `qwen/...` 模型引用。
如果你定义了一个精确的自定义 `models.providers.modelstudio` 条目，并带有另一个 `api`
值，则该自定义提供商会拥有 `modelstudio/...` 引用，而不是 Qwen 兼容别名。

## [​](https://docs.openclaw.ai/zh-CN/providers/qwen\#%E8%AE%A1%E5%88%92%E7%B1%BB%E5%9E%8B%E5%92%8C%E7%AB%AF%E7%82%B9)  计划类型和端点

| 计划 | 区域 | Auth choice | 端点 |
| --- | --- | --- | --- |
| 标准（按量付费） | China | `qwen-standard-api-key-cn` | `dashscope.aliyuncs.com/compatible-mode/v1` |
| 标准（按量付费） | Global | `qwen-standard-api-key` | `dashscope-intl.aliyuncs.com/compatible-mode/v1` |
| Coding Plan（订阅） | China | `qwen-api-key-cn` | `coding.dashscope.aliyuncs.com/v1` |
| Coding Plan（订阅） | Global | `qwen-api-key` | `coding-intl.dashscope.aliyuncs.com/v1` |

该提供商会根据你的 auth choice 自动选择端点。规范选择使用 `qwen-*`
系列；`modelstudio-*` 仅保留用于兼容。
你可以在配置中使用自定义 `baseUrl` 覆盖。

**管理 key：** [home.qwencloud.com/api-keys](https://home.qwencloud.com/api-keys) \|
**文档：** [docs.qwencloud.com](https://docs.qwencloud.com/developer-guides/getting-started/introduction)

## [​](https://docs.openclaw.ai/zh-CN/providers/qwen\#%E5%86%85%E7%BD%AE%E7%9B%AE%E5%BD%95)  内置目录

OpenClaw 目前随附此内置 Qwen 目录。配置的目录会感知端点：Coding Plan
配置会省略仅已知可在标准端点上工作的模型。

| 模型引用 | 输入 | 上下文 | 备注 |
| --- | --- | --- | --- |
| `qwen/qwen3.5-plus` | 文本，图像 | 1,000,000 | 默认模型 |
| `qwen/qwen3.6-plus` | 文本，图像 | 1,000,000 | 需要此模型时优先使用标准端点 |
| `qwen/qwen3-max-2026-01-23` | 文本 | 262,144 | Qwen Max 系列 |
| `qwen/qwen3-coder-next` | 文本 | 262,144 | 编码 |
| `qwen/qwen3-coder-plus` | 文本 | 1,000,000 | 编码 |
| `qwen/MiniMax-M2.5` | 文本 | 1,000,000 | 已启用推理 |
| `qwen/glm-5` | 文本 | 202,752 | GLM |
| `qwen/glm-4.7` | 文本 | 202,752 | GLM |
| `qwen/kimi-k2.5` | 文本，图像 | 262,144 | 通过 Alibaba 提供的 Moonshot AI |

即使某个模型存在于内置目录中，可用性仍可能因端点和计费计划而异。

## [​](https://docs.openclaw.ai/zh-CN/providers/qwen\#%E6%80%9D%E8%80%83%E6%8E%A7%E5%88%B6)  思考控制

对于启用推理的 Qwen Cloud 模型，该内置提供商会将 OpenClaw
思考级别映射到 DashScope 顶层的 `enable_thinking` 请求标志。禁用思考时发送
`enable_thinking: false`；其他思考级别发送
`enable_thinking: true`。

## [​](https://docs.openclaw.ai/zh-CN/providers/qwen\#%E5%A4%9A%E6%A8%A1%E6%80%81%E9%99%84%E5%8A%A0%E5%8A%9F%E8%83%BD)  多模态附加功能

`qwen` 插件还会在 **标准** DashScope 端点（而不是 Coding Plan 端点）上公开多模态能力：

- 通过 `qwen-vl-max-latest` 实现 **视频理解**
- 通过 `wan2.6-t2v`（默认）、`wan2.6-i2v`、`wan2.6-r2v`、`wan2.6-r2v-flash`、`wan2.7-r2v` 实现 **Wan 视频生成**

要将 Qwen 用作默认视频提供商：

```
{
  agents: {
    defaults: {
      videoGenerationModel: { primary: "qwen/wan2.6-t2v" },
    },
  },
}
```

有关共享工具参数、提供商选择和故障转移行为，请参见 [视频生成](https://docs.openclaw.ai/zh-CN/tools/video-generation)。

## [​](https://docs.openclaw.ai/zh-CN/providers/qwen\#%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE)  高级配置

图像和视频理解

内置 Qwen 插件会在 **标准** DashScope 端点（而不是 Coding Plan 端点）上为图像和视频注册媒体理解。

| 属性 | 值 |
| --- | --- |
| 模型 | `qwen-vl-max-latest` |
| 支持的输入 | 图像，视频 |

媒体理解会根据配置的 Qwen 凭证自动解析，不需要额外配置。
请确保你使用的是标准（按量付费）端点，以获得媒体理解支持。

Qwen 3.6 Plus 可用性

`qwen3.6-plus` 可在标准（按量付费）Model Studio
端点上使用：

- China：`dashscope.aliyuncs.com/compatible-mode/v1`
- Global：`dashscope-intl.aliyuncs.com/compatible-mode/v1`

如果 Coding Plan 端点对 `qwen3.6-plus` 返回 “unsupported model” 错误，
请改用标准（按量付费）端点/key 对，而不是 Coding Plan 端点/key 对。OpenClaw 的内置 Qwen 目录不会在 Coding Plan
端点上公布 `qwen3.6-plus`，但会尊重 `models.providers.qwen.models`
下显式配置的 `qwen/qwen3.6-plus` 条目，并在 Coding Plan baseUrl
上使用；因此，如果 Aliyun 在你的订阅中启用了该模型，你可以选择加入。
上游 API 仍会决定调用是否成功。

能力计划

`qwen` 插件正被定位为完整 Qwen Cloud
能力面的厂商归属，而不只是编码/文本模型。

- **文本/聊天模型：** 现已内置
- **工具调用、结构化输出、思考：** 继承自 OpenAI 兼容传输
- **图像生成：** 计划在提供商插件层实现
- **图像/视频理解：** 现已在标准端点上内置
- **语音/音频：** 计划在提供商插件层实现
- **Memory 嵌入/重排序：** 计划通过嵌入适配器能力面实现
- **视频生成：** 现已通过共享视频生成能力内置

视频生成详情

对于视频生成，OpenClaw 会先将配置的 Qwen 区域映射到匹配的
DashScope AIGC 主机，然后再提交任务：

- Global/Intl：`https://dashscope-intl.aliyuncs.com`
- China：`https://dashscope.aliyuncs.com`

这意味着，指向 Coding Plan 或标准 Qwen 主机的普通
`models.providers.qwen.baseUrl` 仍会让视频生成使用正确的区域
DashScope 视频端点。当前内置 Qwen 视频生成限制：

- 每个请求最多 **1** 个输出视频
- 最多 **1** 张输入图像
- 最多 **4** 个输入视频
- 最长 **10 秒** 时长
- 支持 `size`、`aspectRatio`、`resolution`、`audio` 和 `watermark`
- 参考图像/视频模式目前要求使用 **远程 http(s) URL**。本地文件路径会被预先拒绝，
因为 DashScope 视频端点不接受为这些引用上传本地 buffer。

流式用量兼容性

原生 Model Studio 端点会在共享 `openai-completions` 传输上公布流式用量兼容性。
OpenClaw 现在会根据端点能力启用该行为，因此指向同一原生主机的
DashScope 兼容自定义提供商 ID 会继承相同的流式用量行为，而不再要求必须使用内置
`qwen` 提供商 ID。原生流式用量兼容性适用于 Coding Plan 主机和标准 DashScope 兼容主机：

- `https://coding.dashscope.aliyuncs.com/v1`
- `https://coding-intl.dashscope.aliyuncs.com/v1`
- `https://dashscope.aliyuncs.com/compatible-mode/v1`
- `https://dashscope-intl.aliyuncs.com/compatible-mode/v1`

多模态端点区域

多模态能力面（视频理解和 Wan 视频生成）使用 **标准** DashScope
端点，而不是 Coding Plan 端点：

- Global/Intl 标准 base URL：`https://dashscope-intl.aliyuncs.com/compatible-mode/v1`
- China 标准 base URL：`https://dashscope.aliyuncs.com/compatible-mode/v1`

环境和守护进程设置

如果 Gateway 网关作为守护进程（launchd/systemd）运行，请确保 `QWEN_API_KEY`
可供该进程使用（例如，在 `~/.openclaw/.env` 中，或通过
`env.shellEnv`）。

## [​](https://docs.openclaw.ai/zh-CN/providers/qwen\#%E7%9B%B8%E5%85%B3)  相关

[**模型选择** \\
\\
选择提供商、模型引用和故障转移行为。](https://docs.openclaw.ai/zh-CN/concepts/model-providers)

[**视频生成** \\
\\
共享视频工具参数和提供商选择。](https://docs.openclaw.ai/zh-CN/tools/video-generation)

[**Alibaba (ModelStudio)** \\
\\
旧版 ModelStudio 提供商和迁移说明。](https://docs.openclaw.ai/zh-CN/providers/alibaba)

[**故障排除** \\
\\
常规故障排除和常见问题。](https://docs.openclaw.ai/zh-CN/help/troubleshooting)

[千帆](https://docs.openclaw.ai/zh-CN/providers/qianfan) [Synthetic](https://docs.openclaw.ai/zh-CN/providers/synthetic)

Ctrl+I