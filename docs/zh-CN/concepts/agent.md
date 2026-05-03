---
source_url: https://docs.openclaw.ai/zh-CN/concepts/agent
title: "\u667a\u80fd\u4f53\u8fd0\u884c\u65f6 - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/concepts/agent#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

基础

智能体运行时

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [工作区（必需）](https://docs.openclaw.ai/zh-CN/concepts/agent#%E5%B7%A5%E4%BD%9C%E5%8C%BA%EF%BC%88%E5%BF%85%E9%9C%80%EF%BC%89)
- [引导文件（已注入）](https://docs.openclaw.ai/zh-CN/concepts/agent#%E5%BC%95%E5%AF%BC%E6%96%87%E4%BB%B6%EF%BC%88%E5%B7%B2%E6%B3%A8%E5%85%A5%EF%BC%89)
- [内置工具](https://docs.openclaw.ai/zh-CN/concepts/agent#%E5%86%85%E7%BD%AE%E5%B7%A5%E5%85%B7)
- [Skills](https://docs.openclaw.ai/zh-CN/concepts/agent#skills)
- [运行时边界](https://docs.openclaw.ai/zh-CN/concepts/agent#%E8%BF%90%E8%A1%8C%E6%97%B6%E8%BE%B9%E7%95%8C)
- [会话](https://docs.openclaw.ai/zh-CN/concepts/agent#%E4%BC%9A%E8%AF%9D)
- [流式传输时进行引导](https://docs.openclaw.ai/zh-CN/concepts/agent#%E6%B5%81%E5%BC%8F%E4%BC%A0%E8%BE%93%E6%97%B6%E8%BF%9B%E8%A1%8C%E5%BC%95%E5%AF%BC)
- [模型引用](https://docs.openclaw.ai/zh-CN/concepts/agent#%E6%A8%A1%E5%9E%8B%E5%BC%95%E7%94%A8)
- [配置（最小）](https://docs.openclaw.ai/zh-CN/concepts/agent#%E9%85%8D%E7%BD%AE%EF%BC%88%E6%9C%80%E5%B0%8F%EF%BC%89)
- [相关内容](https://docs.openclaw.ai/zh-CN/concepts/agent#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw 运行一个 **单个嵌入式智能体运行时**——每个 Gateway 网关一个智能体进程，并拥有自己的工作区、引导文件和会话存储。本页介绍该运行时契约：工作区必须包含什么、哪些文件会被注入，以及会话如何基于它进行引导。

## [​](https://docs.openclaw.ai/zh-CN/concepts/agent\#%E5%B7%A5%E4%BD%9C%E5%8C%BA%EF%BC%88%E5%BF%85%E9%9C%80%EF%BC%89)  工作区（必需）

OpenClaw 使用单个智能体工作区目录（`agents.defaults.workspace`）作为智能体在工具和上下文中的 **唯一** 工作目录（`cwd`）。推荐：使用 `openclaw setup` 在缺失时创建 `~/.openclaw/openclaw.json`，并初始化工作区文件。完整工作区布局 \+ 备份指南： [Agent 工作区](https://docs.openclaw.ai/zh-CN/concepts/agent-workspace)如果启用了 `agents.defaults.sandbox`，非主会话可以使用 `agents.defaults.sandbox.workspaceRoot` 下的逐会话工作区来覆盖此设置（见 [Gateway 网关配置](https://docs.openclaw.ai/zh-CN/gateway/configuration)）。

## [​](https://docs.openclaw.ai/zh-CN/concepts/agent\#%E5%BC%95%E5%AF%BC%E6%96%87%E4%BB%B6%EF%BC%88%E5%B7%B2%E6%B3%A8%E5%85%A5%EF%BC%89)  引导文件（已注入）

在 `agents.defaults.workspace` 内，OpenClaw 需要这些可由用户编辑的文件：

- `AGENTS.md` — 操作指令 \+ “记忆”
- `SOUL.md` — 人设、边界、语气
- `TOOLS.md` — 用户维护的工具说明（例如 `imsg`、`sag`、约定）
- `BOOTSTRAP.md` — 一次性的首次运行仪式（完成后删除）
- `IDENTITY.md` — 智能体名称/氛围/emoji
- `USER.md` — 用户档案 \+ 偏好的称呼方式

在新会话的第一轮中，OpenClaw 会将这些文件的内容直接注入到智能体上下文中。空白文件会被跳过。大型文件会带标记地精简并截断，以保持提示词轻量（阅读文件可查看完整内容）。如果缺少某个文件，OpenClaw 会注入一行“缺失文件”标记（并且 `openclaw setup` 会创建一个安全的默认模板）。`BOOTSTRAP.md` 只会为 **全新工作区** 创建（不存在其他引导文件）。如果你在完成仪式后删除它，后续重启时不应重新创建它。若要完全禁用引导文件创建（适用于预置内容的工作区），请设置：

```
{ agents: { defaults: { skipBootstrap: true } } }
```

## [​](https://docs.openclaw.ai/zh-CN/concepts/agent\#%E5%86%85%E7%BD%AE%E5%B7%A5%E5%85%B7)  内置工具

核心工具（读取/执行/编辑/写入以及相关系统工具）始终可用，但受工具策略约束。`apply_patch` 是可选的，并由 `tools.exec.applyPatch` 控制。`TOOLS.md` **不** 控制存在哪些工具；它是说明 _你_ 希望如何使用这些工具的指南。

## [​](https://docs.openclaw.ai/zh-CN/concepts/agent\#skills)  Skills

OpenClaw 从这些位置加载 Skills（优先级从高到低）：

- 工作区：`<workspace>/skills`
- 项目智能体 Skills：`<workspace>/.agents/skills`
- 个人智能体 Skills：`~/.agents/skills`
- 托管/本地：`~/.openclaw/skills`
- 内置（随安装包提供）
- 额外 Skills 文件夹：`skills.load.extraDirs`

Skills 可以由配置/环境变量控制（见 [Gateway 网关配置](https://docs.openclaw.ai/zh-CN/gateway/configuration) 中的 `skills`）。

## [​](https://docs.openclaw.ai/zh-CN/concepts/agent\#%E8%BF%90%E8%A1%8C%E6%97%B6%E8%BE%B9%E7%95%8C)  运行时边界

嵌入式智能体运行时基于 Pi 智能体核心构建（模型、工具和提示词流水线）。会话管理、设备发现、工具接线和渠道投递是 OpenClaw 在该核心之上拥有的层。

## [​](https://docs.openclaw.ai/zh-CN/concepts/agent\#%E4%BC%9A%E8%AF%9D)  会话

会话转录以 JSONL 形式存储在：

- `~/.openclaw/agents/<agentId>/sessions/<SessionId>.jsonl`

会话 ID 是稳定的，并由 OpenClaw 选择。
不会读取来自其他工具的旧版会话文件夹。

## [​](https://docs.openclaw.ai/zh-CN/concepts/agent\#%E6%B5%81%E5%BC%8F%E4%BC%A0%E8%BE%93%E6%97%B6%E8%BF%9B%E8%A1%8C%E5%BC%95%E5%AF%BC)  流式传输时进行引导

当队列模式为 `steer` 时，入站消息会被注入当前运行中。已排队的引导会在 **当前助手轮次完成其工具调用执行后**、下一次 LLM 调用前投递。Pi 会为 `steer` 一次性排空所有待处理的引导消息；旧版 `queue` 会在每个模型边界排空一条消息。引导不再跳过当前助手消息中剩余的工具调用。当队列模式为 `followup` 或 `collect` 时，入站消息会一直保留到当前轮次结束，然后新的智能体轮次会带着已排队的载荷启动。有关模式和边界行为，请参阅 [Queue](https://docs.openclaw.ai/zh-CN/concepts/queue) 和 [Steering queue](https://docs.openclaw.ai/zh-CN/concepts/queue-steering)。分块流式传输会在助手块完成后立即发送；它 **默认关闭**（`agents.defaults.blockStreamingDefault: "off"`）。
通过 `agents.defaults.blockStreamingBreak` 调整边界（`text_end` 与 `message_end`；默认为 text\_end）。
使用 `agents.defaults.blockStreamingChunk` 控制软分块切分（默认为 800–1200 个字符；优先段落分隔，其次换行，最后句子）。
使用 `agents.defaults.blockStreamingCoalesce` 合并流式块以减少单行刷屏（发送前基于空闲时间合并）。非 Telegram 渠道需要显式设置 `*.blockStreaming: true` 才能启用分块回复。
详细工具摘要会在工具启动时发出（无防抖）；Control UI 会在可用时通过智能体事件流式传输工具输出。
更多详情： [流式传输 \+ 分块](https://docs.openclaw.ai/zh-CN/concepts/streaming)。

## [​](https://docs.openclaw.ai/zh-CN/concepts/agent\#%E6%A8%A1%E5%9E%8B%E5%BC%95%E7%94%A8)  模型引用

配置中的模型引用（例如 `agents.defaults.model` 和 `agents.defaults.models`）会通过按 **第一个**`/` 拆分来解析。

- 配置模型时使用 `provider/model`。
- 如果模型 ID 本身包含 `/`（OpenRouter 风格），请包含提供商前缀（示例：`openrouter/moonshotai/kimi-k2`）。
- 如果你省略提供商，OpenClaw 会先尝试别名，然后为该精确模型 ID 尝试唯一的已配置提供商匹配，最后才回退到已配置的默认提供商。如果该提供商不再公开已配置的默认模型，OpenClaw 会回退到第一个已配置的提供商/模型，而不是暴露一个已过期的已移除提供商默认值。

## [​](https://docs.openclaw.ai/zh-CN/concepts/agent\#%E9%85%8D%E7%BD%AE%EF%BC%88%E6%9C%80%E5%B0%8F%EF%BC%89)  配置（最小）

至少设置：

- `agents.defaults.workspace`
- `channels.whatsapp.allowFrom`（强烈推荐）

* * *

_下一篇： [群聊](https://docs.openclaw.ai/zh-CN/channels/group-messages)_ 🦞

## [​](https://docs.openclaw.ai/zh-CN/concepts/agent\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

- [Agent 工作区](https://docs.openclaw.ai/zh-CN/concepts/agent-workspace)
- [多智能体路由](https://docs.openclaw.ai/zh-CN/concepts/multi-agent)
- [会话管理](https://docs.openclaw.ai/zh-CN/concepts/session)

[Gateway 网关架构](https://docs.openclaw.ai/zh-CN/concepts/architecture) [Agent loop](https://docs.openclaw.ai/zh-CN/concepts/agent-loop)

Ctrl+I