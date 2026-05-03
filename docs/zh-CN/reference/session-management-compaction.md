---
source_url: https://docs.openclaw.ai/zh-CN/reference/session-management-compaction
title: "\u4f1a\u8bdd\u7ba1\u7406\u6df1\u5165\u89e3\u6790 - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

压缩机制内部参考

会话管理深入解析

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [权威来源：Gateway 网关](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E6%9D%83%E5%A8%81%E6%9D%A5%E6%BA%90%EF%BC%9Agateway-%E7%BD%91%E5%85%B3)
- [两个持久化层](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E4%B8%A4%E4%B8%AA%E6%8C%81%E4%B9%85%E5%8C%96%E5%B1%82)
- [磁盘位置](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E7%A3%81%E7%9B%98%E4%BD%8D%E7%BD%AE)
- [存储维护和磁盘控制](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E5%AD%98%E5%82%A8%E7%BB%B4%E6%8A%A4%E5%92%8C%E7%A3%81%E7%9B%98%E6%8E%A7%E5%88%B6)
- [Cron 会话和运行日志](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#cron-%E4%BC%9A%E8%AF%9D%E5%92%8C%E8%BF%90%E8%A1%8C%E6%97%A5%E5%BF%97)
- [会话键（sessionKey）](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E4%BC%9A%E8%AF%9D%E9%94%AE%EF%BC%88sessionkey%EF%BC%89)
- [会话 id（sessionId）](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E4%BC%9A%E8%AF%9D-id%EF%BC%88sessionid%EF%BC%89)
- [会话存储架构（sessions.json）](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E4%BC%9A%E8%AF%9D%E5%AD%98%E5%82%A8%E6%9E%B6%E6%9E%84%EF%BC%88sessions-json%EF%BC%89)
- [会话记录结构（\*.jsonl）](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E4%BC%9A%E8%AF%9D%E8%AE%B0%E5%BD%95%E7%BB%93%E6%9E%84%EF%BC%88-jsonl%EF%BC%89)
- [上下文窗口与跟踪 token](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E4%B8%8A%E4%B8%8B%E6%96%87%E7%AA%97%E5%8F%A3%E4%B8%8E%E8%B7%9F%E8%B8%AA-token)
- [压缩：它是什么](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E5%8E%8B%E7%BC%A9%EF%BC%9A%E5%AE%83%E6%98%AF%E4%BB%80%E4%B9%88)
- [压缩分块边界和工具配对](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E5%8E%8B%E7%BC%A9%E5%88%86%E5%9D%97%E8%BE%B9%E7%95%8C%E5%92%8C%E5%B7%A5%E5%85%B7%E9%85%8D%E5%AF%B9)
- [自动压缩何时发生（Pi 运行时）](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E8%87%AA%E5%8A%A8%E5%8E%8B%E7%BC%A9%E4%BD%95%E6%97%B6%E5%8F%91%E7%94%9F%EF%BC%88pi-%E8%BF%90%E8%A1%8C%E6%97%B6%EF%BC%89)
- [压缩设置（reserveTokens、keepRecentTokens）](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E5%8E%8B%E7%BC%A9%E8%AE%BE%E7%BD%AE%EF%BC%88reservetokens%E3%80%81keeprecenttokens%EF%BC%89)
- [可插拔压缩提供商](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E5%8F%AF%E6%8F%92%E6%8B%94%E5%8E%8B%E7%BC%A9%E6%8F%90%E4%BE%9B%E5%95%86)
- [用户可见表面](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E7%94%A8%E6%88%B7%E5%8F%AF%E8%A7%81%E8%A1%A8%E9%9D%A2)
- [静默内务处理（NO\_REPLY）](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E9%9D%99%E9%BB%98%E5%86%85%E5%8A%A1%E5%A4%84%E7%90%86%EF%BC%88no_reply%EF%BC%89)
- [压缩前“记忆刷新”（已实现）](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E5%8E%8B%E7%BC%A9%E5%89%8D%E2%80%9C%E8%AE%B0%E5%BF%86%E5%88%B7%E6%96%B0%E2%80%9D%EF%BC%88%E5%B7%B2%E5%AE%9E%E7%8E%B0%EF%BC%89)
- [故障排除清单](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4%E6%B8%85%E5%8D%95)
- [相关](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction#%E7%9B%B8%E5%85%B3)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw 端到端管理以下领域的会话：

- **会话路由**（入站消息如何映射到 `sessionKey`）
- **会话存储**（`sessions.json`）及其跟踪的内容
- **会话记录持久化**（`*.jsonl`）及其结构
- **会话记录整理**（运行前的提供商特定修正）
- **上下文限制**（上下文窗口与跟踪的 token）
- **压缩**（手动压缩和自动压缩）以及在何处挂接压缩前工作
- **静默维护**（不应产生用户可见输出的记忆写入）

如果你想先了解更高层次的概览，请从这里开始：

- [会话管理](https://docs.openclaw.ai/zh-CN/concepts/session)
- [压缩](https://docs.openclaw.ai/zh-CN/concepts/compaction)
- [记忆概览](https://docs.openclaw.ai/zh-CN/concepts/memory)
- [记忆搜索](https://docs.openclaw.ai/zh-CN/concepts/memory-search)
- [会话清理](https://docs.openclaw.ai/zh-CN/concepts/session-pruning)
- [会话记录整理](https://docs.openclaw.ai/zh-CN/reference/transcript-hygiene)

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E6%9D%83%E5%A8%81%E6%9D%A5%E6%BA%90%EF%BC%9Agateway-%E7%BD%91%E5%85%B3)  权威来源：Gateway 网关

OpenClaw 的设计围绕一个拥有会话状态的单一 **Gateway 网关进程**。

- UI（macOS 应用、Web 控制 UI、TUI）应向 Gateway 网关查询会话列表和 token 计数。
- 在远程模式下，会话文件位于远程主机上；“检查你的本地 Mac 文件”不会反映 Gateway 网关正在使用的内容。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E4%B8%A4%E4%B8%AA%E6%8C%81%E4%B9%85%E5%8C%96%E5%B1%82)  两个持久化层

OpenClaw 在两个层中持久化会话：

1. **会话存储（`sessions.json`）**   - 键/值映射：`sessionKey -> SessionEntry`
   - 小型、可变、可安全编辑（或删除条目）
   - 跟踪会话元数据（当前会话 id、最后活动时间、开关、token 计数器等）
2. **会话记录（`<sessionId>.jsonl`）**   - 仅追加的树结构会话记录（条目包含 `id` \+ `parentId`）
   - 存储实际对话 \+ 工具调用 \+ 压缩摘要
   - 用于为未来轮次重建模型上下文
   - 一旦活跃会话记录超过检查点大小上限，就会跳过大型压缩前调试检查点，避免再生成一份巨大的 `.checkpoint.*.jsonl` 副本。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E7%A3%81%E7%9B%98%E4%BD%8D%E7%BD%AE)  磁盘位置

每个智能体，在 Gateway 网关主机上：

- 存储：`~/.openclaw/agents/<agentId>/sessions/sessions.json`
- 会话记录：`~/.openclaw/agents/<agentId>/sessions/<sessionId>.jsonl`
  - Telegram 话题会话：`.../<sessionId>-topic-<threadId>.jsonl`

OpenClaw 通过 `src/config/sessions.ts` 解析这些位置。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E5%AD%98%E5%82%A8%E7%BB%B4%E6%8A%A4%E5%92%8C%E7%A3%81%E7%9B%98%E6%8E%A7%E5%88%B6)  存储维护和磁盘控制

会话持久化为 `sessions.json`、会话记录产物和轨迹旁车文件提供自动维护控制（`session.maintenance`）：

- `mode`：`warn`（默认）或 `enforce`
- `pruneAfter`：过时条目的年龄截止值（默认 `30d`）
- `maxEntries`：`sessions.json` 中的条目上限（默认 `500`）
- `resetArchiveRetention`：`*.reset.<timestamp>` 会话记录归档的保留时间（默认：与 `pruneAfter` 相同；`false` 禁用清理）
- `maxDiskBytes`：可选的会话目录预算
- `highWaterBytes`：清理后的可选目标值（默认是 `maxDiskBytes` 的 `80%`）

正常的 Gateway 网关写入会按生产规模上限批量执行 `maxEntries` 清理，因此在下一次高水位清理把存储重写回上限以下之前，存储可能会短暂超过配置的上限。会话存储读取不会在 Gateway 网关启动期间清理或限制条目数量；使用写入或 `openclaw sessions cleanup --enforce` 进行清理。`openclaw sessions cleanup --enforce` 仍会立即应用配置的上限。维护会保留持久的外部对话指针，例如群组会话和按线程限定的聊天会话，但 cron、钩子、Heartbeat、ACP 和子智能体的合成运行时条目在超过配置的年龄、数量或磁盘预算时仍可被移除。OpenClaw 不再在 Gateway 网关写入期间自动创建 `sessions.json.bak.*` 轮转备份。旧版 `session.maintenance.rotateBytes` 键会被忽略，`openclaw doctor --fix` 会从旧配置中移除它。磁盘预算清理的执行顺序（`mode: "enforce"`）：

1. 首先移除最旧的归档产物、孤立会话记录或孤立轨迹产物。
2. 如果仍高于目标值，则逐出最旧的会话条目及其会话记录/轨迹文件。
3. 持续执行，直到用量处于或低于 `highWaterBytes`。

在 `mode: "warn"` 中，OpenClaw 会报告潜在逐出项，但不会修改存储/文件。按需运行维护：

```
openclaw sessions cleanup --dry-run
openclaw sessions cleanup --enforce
```

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#cron-%E4%BC%9A%E8%AF%9D%E5%92%8C%E8%BF%90%E8%A1%8C%E6%97%A5%E5%BF%97)  Cron 会话和运行日志

隔离的 cron 运行也会创建会话条目/会话记录，并且有专用的保留控制：

- `cron.sessionRetention`（默认 `24h`）会从会话存储中清理旧的隔离 cron 运行会话（`false` 禁用）。
- `cron.runLog.maxBytes` \+ `cron.runLog.keepLines` 会清理 `~/.openclaw/cron/runs/<jobId>.jsonl` 文件（默认值：`2_000_000` 字节和 `2000` 行）。

当 cron 强制创建新的隔离运行会话时，它会在写入新行之前清理之前的 `cron:<jobId>` 会话条目。它会携带安全偏好，例如思考/快速/详细设置、标签，以及显式的用户所选模型/身份验证覆盖项。它会丢弃环境对话上下文，例如渠道/群组路由、发送或队列策略、提权、来源，以及 ACP 运行时绑定，因此新的隔离运行不会从旧运行继承过期的交付或运行时权限。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E4%BC%9A%E8%AF%9D%E9%94%AE%EF%BC%88sessionkey%EF%BC%89)  会话键（`sessionKey`）

`sessionKey` 标识你所在的\_对话桶\_（路由 + 隔离）。常见模式：

- 主/直接聊天（每个智能体）：`agent:<agentId>:<mainKey>`（默认 `main`）
- 群组：`agent:<agentId>:<channel>:group:<id>`
- 房间/渠道（Discord/Slack）：`agent:<agentId>:<channel>:channel:<id>` 或 `...:room:<id>`
- Cron：`cron:<job.id>`
- Webhook：`hook:<uuid>`（除非被覆盖）

规范规则记录在 [/concepts/session](https://docs.openclaw.ai/zh-CN/concepts/session)。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E4%BC%9A%E8%AF%9D-id%EF%BC%88sessionid%EF%BC%89)  会话 id（`sessionId`）

每个 `sessionKey` 都指向一个当前 `sessionId`（继续对话的会话记录文件）。经验规则：

- **重置**（`/new`、`/reset`）会为该 `sessionKey` 创建新的 `sessionId`。
- **每日重置**（默认是 Gateway 网关主机本地时间凌晨 4:00）会在重置边界后的下一条消息到达时创建新的 `sessionId`。
- **空闲过期**（`session.reset.idleMinutes` 或旧版 `session.idleMinutes`）会在空闲窗口之后收到消息时创建新的 `sessionId`。当每日重置和空闲过期都已配置时，先过期者生效。
- **系统事件**（Heartbeat、cron 唤醒、exec 通知、Gateway 网关簿记）可能会修改会话行，但不会延长每日/空闲重置的新鲜度。重置滚动会在构建新提示前丢弃上一会话排队的系统事件通知。
- **线程父级分叉保护**（`session.parentForkMaxTokens`，默认 `100000`）会在父会话已经过大时跳过父会话记录分叉；新线程会从空白状态开始。设置为 `0` 可禁用。

实现细节：该决策发生在 `src/auto-reply/reply/session.ts` 中的 `initSessionState()`。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E4%BC%9A%E8%AF%9D%E5%AD%98%E5%82%A8%E6%9E%B6%E6%9E%84%EF%BC%88sessions-json%EF%BC%89)  会话存储架构（`sessions.json`）

存储的值类型是 `src/config/sessions.ts` 中的 `SessionEntry`。关键字段（并非详尽）：

- `sessionId`：当前会话记录 id（除非设置了 `sessionFile`，否则文件名由此派生）
- `sessionStartedAt`：当前 `sessionId` 的开始时间戳；每日重置新鲜度使用此值。旧版行可从 JSONL 会话头派生该值。
- `lastInteractionAt`：最后一次真实用户/渠道交互时间戳；空闲重置新鲜度使用此值，因此 Heartbeat、cron 和 exec 事件不会让会话保持存活。没有该字段的旧版行会回退到恢复出的会话开始时间来计算空闲新鲜度。
- `updatedAt`：最后一次存储行修改时间戳，用于列表、清理和簿记。它不是每日/空闲重置新鲜度的权威依据。
- `sessionFile`：可选的显式会话记录路径覆盖
- `chatType`：`direct | group | room`（帮助 UI 和发送策略）
- `provider`、`subject`、`room`、`space`、`displayName`：用于群组/渠道标签的元数据
- 开关：
  - `thinkingLevel`、`verboseLevel`、`reasoningLevel`、`elevatedLevel`
  - `sendPolicy`（按会话覆盖）
- 模型选择：
  - `providerOverride`、`modelOverride`、`authProfileOverride`
- Token 计数器（尽力而为/依赖提供商）：
  - `inputTokens`、`outputTokens`、`totalTokens`、`contextTokens`
- `compactionCount`：此会话键自动压缩完成的次数
- `memoryFlushAt`：最后一次压缩前记忆刷新的时间戳
- `memoryFlushCompactionCount`：上次刷新运行时的压缩计数

存储可以安全编辑，但 Gateway 网关是权威来源：随着会话运行，它可能会重写或重新水化条目。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E4%BC%9A%E8%AF%9D%E8%AE%B0%E5%BD%95%E7%BB%93%E6%9E%84%EF%BC%88-jsonl%EF%BC%89)  会话记录结构（`*.jsonl`）

会话记录由 `@mariozechner/pi-coding-agent` 的 `SessionManager` 管理。文件为 JSONL：

- 第一行：会话头（`type: "session"`，包含 `id`、`cwd`、`timestamp`，可选 `parentSession`）
- 随后：带有 `id` \+ `parentId` 的会话条目（树）

值得注意的条目类型：

- `message`：用户/助手/toolResult 消息
- `custom_message`：扩展注入的消息，\_会\_进入模型上下文（可对 UI 隐藏）
- `custom`：\_不会\_进入模型上下文的扩展状态
- `compaction`：带有 `firstKeptEntryId` 和 `tokensBefore` 的持久化压缩摘要
- `branch_summary`：导航树分支时的持久化摘要

OpenClaw 有意 **不会**“修正”会话记录；Gateway 网关使用 `SessionManager` 读写它们。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E4%B8%8A%E4%B8%8B%E6%96%87%E7%AA%97%E5%8F%A3%E4%B8%8E%E8%B7%9F%E8%B8%AA-token)  上下文窗口与跟踪 token

有两个不同概念很重要：

1. **模型上下文窗口**：每个模型的硬上限（模型可见的 token）
2. **会话存储计数器**：写入 `sessions.json` 的滚动统计（用于 /status 和仪表板）

如果你在调整限制：

- 上下文窗口来自模型目录（也可以通过配置覆盖）。
- 存储中的 `contextTokens` 是运行时估算/报告值；不要把它当作严格保证。

更多信息请参阅 [/token-use](https://docs.openclaw.ai/zh-CN/reference/token-use)。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E5%8E%8B%E7%BC%A9%EF%BC%9A%E5%AE%83%E6%98%AF%E4%BB%80%E4%B9%88)  压缩：它是什么

压缩会把较早的对话摘要为会话记录中的持久化 `compaction` 条目，并保留最近消息不变。压缩后，未来轮次会看到：

- 压缩摘要
- `firstKeptEntryId` 之后的消息

压缩是 **持久的**（不同于会话清理）。请参阅 [/concepts/session-pruning](https://docs.openclaw.ai/zh-CN/concepts/session-pruning)。

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E5%8E%8B%E7%BC%A9%E5%88%86%E5%9D%97%E8%BE%B9%E7%95%8C%E5%92%8C%E5%B7%A5%E5%85%B7%E9%85%8D%E5%AF%B9)  压缩分块边界和工具配对

当 OpenClaw 将长会话记录拆分为压缩分块时，它会保持助手工具调用与对应的 `toolResult` 条目配对。

- 如果按 token 占比拆分的位置落在工具调用和其结果之间，OpenClaw 会把边界移动到助手工具调用消息，而不是分离这对条目。
- 如果尾随的工具结果块本来会让分块超过目标大小，OpenClaw 会保留该待处理工具块，并保持未摘要的尾部不变。
- 已中止/错误的工具调用块不会让待处理拆分保持打开状态。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E8%87%AA%E5%8A%A8%E5%8E%8B%E7%BC%A9%E4%BD%95%E6%97%B6%E5%8F%91%E7%94%9F%EF%BC%88pi-%E8%BF%90%E8%A1%8C%E6%97%B6%EF%BC%89)  自动压缩何时发生（Pi 运行时）

在嵌入式 Pi 智能体中，自动压缩会在两种情况下触发：

1. **溢出恢复**：模型返回上下文溢出错误（`request_too_large`、`context length exceeded`、`input exceeds the maximum number of tokens`、`input token count exceeds the maximum number of input tokens`、`input is too long for the model`、`ollama error: context length exceeded`，以及类似的提供商形态变体）→ 压缩 → 重试。
2. **阈值维护**：成功轮次之后，当：

`contextTokens > contextWindow - reserveTokens`其中：

- `contextWindow` 是模型的上下文窗口
- `reserveTokens` 是为提示 \+ 下一次模型输出预留的余量

这些是 Pi 运行时语义（OpenClaw 会消费事件，但由 Pi 决定何时压缩）。OpenClaw 还可以在打开下一次运行前触发一次预检本地压缩，前提是设置了 `agents.defaults.compaction.maxActiveTranscriptBytes`，且活跃会话记录文件达到该大小。这是针对本地重新打开成本的文件大小保护，并非原始归档：OpenClaw 仍会运行常规语义压缩，并且它要求启用 `truncateAfterCompaction`，这样压缩后的摘要才能成为新的后继会话记录。对于嵌入式 Pi 运行，`agents.defaults.compaction.midTurnPrecheck.enabled: true` 会添加一个可选启用的工具循环保护。在追加工具结果之后、下一次模型调用之前，OpenClaw 会使用与回合开始时相同的预检预算逻辑来估算提示压力。如果上下文不再适配，该保护不会在 Pi 的 `transformContext` 钩子内执行压缩。它会抛出一个结构化的回合中预检信号，停止当前提示提交，并让外层运行循环使用现有恢复路径：在足够时截断过大的工具结果，或触发已配置的压缩模式并重试。该选项默认禁用，并且可与 `default` 和 `safeguard` 两种压缩模式配合使用，包括由提供商支持的 safeguard 压缩。
这独立于 `maxActiveTranscriptBytes`：字节大小保护会在打开回合之前运行，而回合中预检则稍后在嵌入式 Pi 工具循环中、追加新的工具结果之后运行。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E5%8E%8B%E7%BC%A9%E8%AE%BE%E7%BD%AE%EF%BC%88reservetokens%E3%80%81keeprecenttokens%EF%BC%89)  压缩设置（`reserveTokens`、`keepRecentTokens`）

Pi 的压缩设置位于 Pi 设置中：

```
{
  compaction: {
    enabled: true,
    reserveTokens: 16384,
    keepRecentTokens: 20000,
  },
}
```

OpenClaw 还会为嵌入式运行强制实施安全下限：

- 如果 `compaction.reserveTokens < reserveTokensFloor`，OpenClaw 会将其提高。
- 默认下限为 `20000` 个 token。
- 将 `agents.defaults.compaction.reserveTokensFloor: 0` 设置为禁用下限。
- 如果它已经更高，OpenClaw 会保持不变。
- 手动 `/compact` 会遵循显式的 `agents.defaults.compaction.keepRecentTokens`，并保留 Pi 的最近尾部切点。如果没有显式保留预算，手动压缩仍是硬检查点，重建后的上下文会从新的摘要开始。
- 将 `agents.defaults.compaction.midTurnPrecheck.enabled: true` 设置为在新的工具结果之后、下一次模型调用之前运行可选的工具循环预检。这只是触发器；摘要生成仍使用已配置的压缩路径。它独立于 `maxActiveTranscriptBytes`，后者是回合开始时的活跃会话记录字节大小保护。
- 将 `agents.defaults.compaction.maxActiveTranscriptBytes` 设置为字节值或 `"20mb"` 这样的字符串，以便在活跃会话记录变大时，在回合开始前运行本地压缩。此保护仅在同时启用 `truncateAfterCompaction` 时生效。保持未设置或设置为 `0` 可禁用。
- 启用 `agents.defaults.compaction.truncateAfterCompaction` 时，OpenClaw 会在压缩后将活跃会话记录轮转为压缩后的后继 JSONL。旧的完整会话记录会保持归档状态，并从压缩检查点链接，而不是原地重写。

原因：在压缩变得不可避免之前，为多回合“内务处理”（如记忆写入）留出足够的余量。实现：`src/agents/pi-settings.ts` 中的 `ensurePiCompactionReserveTokens()`
（从 `src/agents/pi-embedded-runner.ts` 调用）。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E5%8F%AF%E6%8F%92%E6%8B%94%E5%8E%8B%E7%BC%A9%E6%8F%90%E4%BE%9B%E5%95%86)  可插拔压缩提供商

插件可以通过插件 API 上的 `registerCompactionProvider()` 注册压缩提供商。当 `agents.defaults.compaction.provider` 设置为已注册的提供商 ID 时，safeguard 扩展会将摘要生成委托给该提供商，而不是使用内置的 `summarizeInStages` 流水线。

- `provider`：已注册压缩提供商插件的 ID。保持未设置则使用默认 LLM 摘要生成。
- 设置 `provider` 会强制 `mode: "safeguard"`。
- 提供商会收到与内置路径相同的压缩指令和标识符保留策略。
- safeguard 仍会在提供商输出后保留最近回合和拆分回合的后缀上下文。
- 内置 safeguard 摘要生成会使用新消息重新提炼既有摘要，而不是逐字保留完整的先前摘要。
- safeguard 模式默认启用摘要质量审计；设置 `qualityGuard.enabled: false` 可跳过格式异常输出的重试行为。
- 如果提供商失败或返回空结果，OpenClaw 会自动回退到内置 LLM 摘要生成。
- 中止/超时信号会重新抛出（不会被吞掉），以尊重调用方取消。

来源：`src/plugins/compaction-provider.ts`、`src/agents/pi-hooks/compaction-safeguard.ts`。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E7%94%A8%E6%88%B7%E5%8F%AF%E8%A7%81%E8%A1%A8%E9%9D%A2)  用户可见表面

你可以通过以下方式观察压缩和会话状态：

- `/status`（在任意聊天会话中）
- `openclaw status`（CLI）
- `openclaw sessions` / `sessions --json`
- 详细模式：`🧹 Auto-compaction complete` \+ 压缩次数

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E9%9D%99%E9%BB%98%E5%86%85%E5%8A%A1%E5%A4%84%E7%90%86%EF%BC%88no_reply%EF%BC%89)  静默内务处理（`NO_REPLY`）

OpenClaw 支持用于后台任务的“静默”回合，在这些任务中用户不应看到中间输出。约定：

- 助手以精确静默 token `NO_REPLY` / `no_reply` 开始输出，表示“不要向用户投递回复”。
- OpenClaw 会在投递层剥离/抑制此内容。
- 精确静默 token 抑制不区分大小写，因此当整个载荷仅为静默 token 时，`NO_REPLY` 和 `no_reply` 都会生效。
- 这仅用于真正的后台/不投递回合；它不是普通可执行用户请求的快捷方式。

从 `2026.1.10` 起，当部分分块以 `NO_REPLY` 开头时，OpenClaw 也会抑制 **草稿/输入中流式传输**，因此静默操作不会在回合中泄漏部分输出。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E5%8E%8B%E7%BC%A9%E5%89%8D%E2%80%9C%E8%AE%B0%E5%BF%86%E5%88%B7%E6%96%B0%E2%80%9D%EF%BC%88%E5%B7%B2%E5%AE%9E%E7%8E%B0%EF%BC%89)  压缩前“记忆刷新”（已实现）

目标：在自动压缩发生前，运行一个静默的智能体回合，将持久状态写入磁盘（例如 Agent 工作区中的 `memory/YYYY-MM-DD.md`），这样压缩就不会抹除关键上下文。OpenClaw 使用 **阈值前刷新** 方法：

1. 监控会话上下文使用量。
2. 当它跨过“软阈值”（低于 Pi 的压缩阈值）时，向智能体运行一条静默的“立即写入记忆”指令。
3. 使用精确静默 token `NO_REPLY` / `no_reply`，这样用户不会看到任何内容。

配置（`agents.defaults.compaction.memoryFlush`）：

- `enabled`（默认：`true`）
- `model`（刷新回合的可选精确提供商/模型覆盖，例如 `ollama/qwen3:8b`）
- `softThresholdTokens`（默认：`4000`）
- `prompt`（刷新回合的用户消息）
- `systemPrompt`（为刷新回合追加的额外系统提示）

说明：

- 默认提示/系统提示包含 `NO_REPLY` 提示，用于抑制投递。
- 设置 `model` 时，刷新回合会使用该模型，而不会继承活跃会话的回退链，因此仅本地的内务处理不会静默回退到付费对话模型。
- 刷新每个压缩周期运行一次（在 `sessions.json` 中跟踪）。
- 刷新仅针对嵌入式 Pi 会话运行（CLI 后端会跳过）。
- 当会话工作区为只读（`workspaceAccess: "ro"` 或 `"none"`）时，会跳过刷新。
- 请参阅 [Memory](https://docs.openclaw.ai/zh-CN/concepts/memory) 了解工作区文件布局和写入模式。

Pi 还在扩展 API 中公开了 `session_before_compact` 钩子，但 OpenClaw 的刷新逻辑目前位于 Gateway 网关侧。

* * *

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4%E6%B8%85%E5%8D%95)  故障排除清单

- 会话键错误？从 [/concepts/session](https://docs.openclaw.ai/zh-CN/concepts/session) 开始，并确认 `/status` 中的 `sessionKey`。
- 存储与会话记录不匹配？从 `openclaw status` 确认 Gateway 网关主机和存储路径。
- 压缩刷屏？检查：
  - 模型上下文窗口（太小）
  - 压缩设置（`reserveTokens` 对模型窗口来说过高可能导致更早压缩）
  - 工具结果膨胀：启用/调优会话剪枝
- 静默回合泄漏？确认回复以 `NO_REPLY` 开头（不区分大小写的精确 token），并且你使用的是包含流式传输抑制修复的构建版本。

## [​](https://docs.openclaw.ai/zh-CN/reference/session-management-compaction\#%E7%9B%B8%E5%85%B3)  相关

- [会话管理](https://docs.openclaw.ai/zh-CN/concepts/session)
- [会话剪枝](https://docs.openclaw.ai/zh-CN/concepts/session-pruning)
- [上下文引擎](https://docs.openclaw.ai/zh-CN/concepts/context-engine)

[Node.js](https://docs.openclaw.ai/zh-CN/install/node) [设置](https://docs.openclaw.ai/zh-CN/start/setup)

Ctrl+I