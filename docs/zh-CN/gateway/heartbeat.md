---
source_url: https://docs.openclaw.ai/zh-CN/gateway/heartbeat
title: "Heartbeat - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

配置与运维

Heartbeat

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [快速开始（初学者）](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B%EF%BC%88%E5%88%9D%E5%AD%A6%E8%80%85%EF%BC%89)
- [默认值](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E9%BB%98%E8%AE%A4%E5%80%BC)
- [heartbeat 提示的用途](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#heartbeat-%E6%8F%90%E7%A4%BA%E7%9A%84%E7%94%A8%E9%80%94)
- [响应契约](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E5%93%8D%E5%BA%94%E5%A5%91%E7%BA%A6)
- [配置](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E9%85%8D%E7%BD%AE)
- [范围和优先级](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E8%8C%83%E5%9B%B4%E5%92%8C%E4%BC%98%E5%85%88%E7%BA%A7)
- [每智能体 Heartbeat](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E6%AF%8F%E6%99%BA%E8%83%BD%E4%BD%93-heartbeat)
- [活跃时段示例](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E6%B4%BB%E8%B7%83%E6%97%B6%E6%AE%B5%E7%A4%BA%E4%BE%8B)
- [全天候设置](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E5%85%A8%E5%A4%A9%E5%80%99%E8%AE%BE%E7%BD%AE)
- [多账号示例](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E5%A4%9A%E8%B4%A6%E5%8F%B7%E7%A4%BA%E4%BE%8B)
- [字段说明](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E5%AD%97%E6%AE%B5%E8%AF%B4%E6%98%8E)
- [递送行为](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E9%80%92%E9%80%81%E8%A1%8C%E4%B8%BA)
- [可见性控制](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E5%8F%AF%E8%A7%81%E6%80%A7%E6%8E%A7%E5%88%B6)
- [每个标志的作用](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E6%AF%8F%E4%B8%AA%E6%A0%87%E5%BF%97%E7%9A%84%E4%BD%9C%E7%94%A8)
- [按渠道与按账号示例](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E6%8C%89%E6%B8%A0%E9%81%93%E4%B8%8E%E6%8C%89%E8%B4%A6%E5%8F%B7%E7%A4%BA%E4%BE%8B)
- [常见模式](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E5%B8%B8%E8%A7%81%E6%A8%A1%E5%BC%8F)
- [HEARTBEAT.md（可选）](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#heartbeat-md%EF%BC%88%E5%8F%AF%E9%80%89%EF%BC%89)
- [tasks: 块](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#tasks-%E5%9D%97)
- [智能体可以更新 HEARTBEAT.md 吗？](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E6%99%BA%E8%83%BD%E4%BD%93%E5%8F%AF%E4%BB%A5%E6%9B%B4%E6%96%B0-heartbeat-md-%E5%90%97%EF%BC%9F)
- [手动唤醒（按需）](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E6%89%8B%E5%8A%A8%E5%94%A4%E9%86%92%EF%BC%88%E6%8C%89%E9%9C%80%EF%BC%89)
- [推理递送（可选）](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E6%8E%A8%E7%90%86%E9%80%92%E9%80%81%EF%BC%88%E5%8F%AF%E9%80%89%EF%BC%89)
- [成本意识](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E6%88%90%E6%9C%AC%E6%84%8F%E8%AF%86)
- [Heartbeat 后的上下文溢出](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#heartbeat-%E5%90%8E%E7%9A%84%E4%B8%8A%E4%B8%8B%E6%96%87%E6%BA%A2%E5%87%BA)
- [相关](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#%E7%9B%B8%E5%85%B3)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

**Heartbeat 和 cron？** 请参阅 [自动化与任务](https://docs.openclaw.ai/zh-CN/automation)，了解何时使用二者。

Heartbeat 会在主会话中运行 **周期性的智能体轮次**，让模型可以提示任何需要注意的事项，而不会向你发送大量消息。Heartbeat 是一次计划的主会话轮次，它 **不会** 创建 [后台任务](https://docs.openclaw.ai/zh-CN/automation/tasks) 记录。任务记录用于脱离主流程的工作（ACP 运行、子智能体、隔离的 cron 作业）。故障排除： [定时任务](https://docs.openclaw.ai/zh-CN/automation/cron-jobs#troubleshooting)

## [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B%EF%BC%88%E5%88%9D%E5%AD%A6%E8%80%85%EF%BC%89)  快速开始（初学者）

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#)

选择节奏

保持 heartbeat 启用（默认是 `30m`，Anthropic OAuth/token auth 时为 `1h`，包括 Claude CLI 复用），或设置你自己的节奏。

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#)

添加 HEARTBEAT.md（可选）

在 Agent 工作区中创建一个小型 `HEARTBEAT.md` 清单或 `tasks:` 块。

3

[Navigate to header](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#)

决定 heartbeat 消息应发送到哪里

`target: "none"` 是默认值；设置 `target: "last"` 可路由到最近的联系人。

4

[Navigate to header](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#)

可选调优

- 启用 heartbeat 推理传递以提高透明度。
- 如果 heartbeat 运行只需要 `HEARTBEAT.md`，使用轻量级 bootstrap 上下文。
- 启用隔离会话，以避免每次 heartbeat 都发送完整对话历史。
- 将 heartbeat 限制在活跃时段（本地时间）。

配置示例：

```
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",
        target: "last", // explicit delivery to last contact (default is "none")
        directPolicy: "allow", // default: allow direct/DM targets; set "block" to suppress
        lightContext: true, // optional: only inject HEARTBEAT.md from bootstrap files
        isolatedSession: true, // optional: fresh session each run (no conversation history)
        skipWhenBusy: true, // optional: also defer when subagent or nested lanes are busy
        // activeHours: { start: "08:00", end: "24:00" },
        // includeReasoning: true, // optional: send separate `Reasoning:` message too
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E9%BB%98%E8%AE%A4%E5%80%BC)  默认值

- 间隔：`30m`（当检测到的 auth 模式为 Anthropic OAuth/token auth 时为 `1h`，包括 Claude CLI 复用）。设置 `agents.defaults.heartbeat.every` 或每个智能体的 `agents.list[].heartbeat.every`；使用 `0m` 禁用。
- 提示正文（可通过 `agents.defaults.heartbeat.prompt` 配置）：`Read HEARTBEAT.md if it exists (workspace context). Follow it strictly. Do not infer or repeat old tasks from prior chats. If nothing needs attention, reply HEARTBEAT_OK.`
- heartbeat 提示会作为用户消息 **逐字** 发送。只有当默认智能体启用了 heartbeat 时，系统提示才会包含 “Heartbeat” 部分，并且该运行会在内部标记。
- 当使用 `0m` 禁用 heartbeat 时，普通运行也会从 bootstrap 上下文中省略 `HEARTBEAT.md`，因此模型不会看到仅用于 heartbeat 的指令。
- 活跃时段（`heartbeat.activeHours`）会按配置的时区检查。在窗口外，heartbeat 会被跳过，直到窗口内的下一个 tick。
- 当 cron 工作处于活动或排队状态时，heartbeat 会自动延后。设置 `heartbeat.skipWhenBusy: true`，还会在额外繁忙的通道（子智能体或嵌套命令工作）上延后；这对本地 Ollama 和其他受限的单运行时主机很有用。

## [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#heartbeat-%E6%8F%90%E7%A4%BA%E7%9A%84%E7%94%A8%E9%80%94)  heartbeat 提示的用途

默认提示有意保持宽泛：

- **后台任务**：“Consider outstanding tasks” 会提示智能体审查跟进事项（收件箱、日历、提醒、排队工作），并提示任何紧急事项。
- **与人类确认**：“Checkup sometimes on your human during day time” 会提示偶尔发送轻量的“anything you need?”消息，但会使用你配置的本地时区来避免夜间打扰（参见 [时区](https://docs.openclaw.ai/zh-CN/concepts/timezone)）。

Heartbeat 可以响应已完成的 [后台任务](https://docs.openclaw.ai/zh-CN/automation/tasks)，但 heartbeat 运行本身不会创建任务记录。如果你希望 heartbeat 执行非常具体的事项（例如“检查 Gmail PubSub 统计信息”或“验证 Gateway 网关健康状况”），请将 `agents.defaults.heartbeat.prompt`（或 `agents.list[].heartbeat.prompt`）设置为自定义正文（逐字发送）。

## [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E5%93%8D%E5%BA%94%E5%A5%91%E7%BA%A6)  响应契约

- 如果没有需要注意的事项，回复 **`HEARTBEAT_OK`**。
- 支持工具的 heartbeat 运行也可以调用 `heartbeat_respond`，使用 `notify: false` 表示没有可见更新，或使用 `notify: true` 加 `notificationText` 发送提醒。存在结构化工具响应时，它优先于文本回退。
- 在 heartbeat 运行期间，如果 `HEARTBEAT_OK` 出现在回复的 **开头或结尾**，OpenClaw 会将其视为确认。该 token 会被移除；如果剩余内容\*\*≤ `ackMaxChars`\*\*（默认：300），回复会被丢弃。
- 如果 `HEARTBEAT_OK` 出现在回复 **中间**，不会被特殊处理。
- 对于提醒， **不要** 包含 `HEARTBEAT_OK`；只返回提醒文本。

在 heartbeat 之外，消息开头/结尾出现的游离 `HEARTBEAT_OK` 会被移除并记录；只有 `HEARTBEAT_OK` 的消息会被丢弃。

## [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E9%85%8D%E7%BD%AE)  配置

```
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m", // default: 30m (0m disables)
        model: "anthropic/claude-opus-4-6",
        includeReasoning: false, // default: false (deliver separate Reasoning: message when available)
        lightContext: false, // default: false; true keeps only HEARTBEAT.md from workspace bootstrap files
        isolatedSession: false, // default: false; true runs each heartbeat in a fresh session (no conversation history)
        skipWhenBusy: false, // default: false; true also waits for subagent/nested lanes
        target: "last", // default: none | options: last | none | <channel id> (core or plugin, e.g. "bluebubbles")
        to: "+15551234567", // optional channel-specific override
        accountId: "ops-bot", // optional multi-account channel id
        prompt: "Read HEARTBEAT.md if it exists (workspace context). Follow it strictly. Do not infer or repeat old tasks from prior chats. If nothing needs attention, reply HEARTBEAT_OK.",
        ackMaxChars: 300, // max chars allowed after HEARTBEAT_OK
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E8%8C%83%E5%9B%B4%E5%92%8C%E4%BC%98%E5%85%88%E7%BA%A7)  范围和优先级

- `agents.defaults.heartbeat` 设置全局 heartbeat 行为。
- `agents.list[].heartbeat` 在其上合并；如果任何智能体有 `heartbeat` 块， **只有这些智能体** 会运行 heartbeat。
- `channels.defaults.heartbeat` 为所有渠道设置可见性默认值。
- `channels.<channel>.heartbeat` 覆盖渠道默认值。
- `channels.<channel>.accounts.<id>.heartbeat`（多账号渠道）覆盖每个渠道的设置。

### [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E6%AF%8F%E6%99%BA%E8%83%BD%E4%BD%93-heartbeat)  每智能体 Heartbeat

如果任何 `agents.list[]` 条目包含 `heartbeat` 块， **只有这些智能体** 会运行 Heartbeat。每智能体块会合并到 `agents.defaults.heartbeat` 之上（因此你可以只设置一次共享默认值，并按智能体覆盖）。示例：两个智能体，只有第二个智能体运行 Heartbeat。

```
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",
        target: "last", // explicit delivery to last contact (default is "none")
      },
    },
    list: [\
      { id: "main", default: true },\
      {\
        id: "ops",\
        heartbeat: {\
          every: "1h",\
          target: "whatsapp",\
          to: "+15551234567",\
          timeoutSeconds: 45,\
          prompt: "Read HEARTBEAT.md if it exists (workspace context). Follow it strictly. Do not infer or repeat old tasks from prior chats. If nothing needs attention, reply HEARTBEAT_OK.",\
        },\
      },\
    ],
  },
}
```

### [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E6%B4%BB%E8%B7%83%E6%97%B6%E6%AE%B5%E7%A4%BA%E4%BE%8B)  活跃时段示例

将 Heartbeat 限制在特定时区的工作时间内：

```
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",
        target: "last", // explicit delivery to last contact (default is "none")
        activeHours: {
          start: "09:00",
          end: "22:00",
          timezone: "America/New_York", // optional; uses your userTimezone if set, otherwise host tz
        },
      },
    },
  },
}
```

在此时间窗口之外（美国东部时间上午 9 点之前或晚上 10 点之后），会跳过 Heartbeat。窗口内的下一次计划触发会正常运行。

### [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E5%85%A8%E5%A4%A9%E5%80%99%E8%AE%BE%E7%BD%AE)  全天候设置

如果你希望 Heartbeat 全天运行，请使用以下模式之一：

- 完全省略 `activeHours`（无时间窗口限制；这是默认行为）。
- 设置全天窗口：`activeHours: { start: "00:00", end: "24:00" }`。

不要将 `start` 和 `end` 时间设置为相同（例如从 `08:00` 到 `08:00`）。这会被视为零宽窗口，因此 Heartbeat 总是会被跳过。

### [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E5%A4%9A%E8%B4%A6%E5%8F%B7%E7%A4%BA%E4%BE%8B)  多账号示例

使用 `accountId` 在 Telegram 等多账号渠道中定位特定账号：

```
{
  agents: {
    list: [\
      {\
        id: "ops",\
        heartbeat: {\
          every: "1h",\
          target: "telegram",\
          to: "12345678:topic:42", // optional: route to a specific topic/thread\
          accountId: "ops-bot",\
        },\
      },\
    ],
  },
  channels: {
    telegram: {
      accounts: {
        "ops-bot": { botToken: "YOUR_TELEGRAM_BOT_TOKEN" },
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E5%AD%97%E6%AE%B5%E8%AF%B4%E6%98%8E)  字段说明

[​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#param-every)

every

string

Heartbeat 间隔（时长字符串；默认单位 = 分钟）。

[​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#param-model)

model

string

Heartbeat 运行的可选模型覆盖（`provider/model`）。

[​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#param-include-reasoning)

includeReasoning

boolean

默认值:"false"

启用后，在可用时也会发送单独的 `Reasoning:` 消息（形状与 `/reasoning on` 相同）。

[​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#param-light-context)

lightContext

boolean

默认值:"false"

为 true 时，Heartbeat 运行会使用轻量级启动上下文，并且只保留工作区启动文件中的 `HEARTBEAT.md`。

[​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#param-isolated-session)

isolatedSession

boolean

默认值:"false"

为 true 时，每次 Heartbeat 都会在一个没有先前对话历史的全新会话中运行。使用与 cron `sessionTarget: "isolated"` 相同的隔离模式。可大幅降低每次 Heartbeat 的 token 成本。与 `lightContext: true` 结合使用可获得最大节省。递送路由仍使用主会话上下文。

[​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#param-skip-when-busy)

skipWhenBusy

boolean

默认值:"false"

为 true 时，Heartbeat 运行会在额外繁忙的执行通道上延后：子智能体或嵌套命令工作。cron 执行通道始终会延后 Heartbeat，即使没有此标志也是如此，因此本地模型主机不会同时运行 cron 和 Heartbeat prompt。

[​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#param-session)

session

string

Heartbeat 运行的可选会话键。

- `main`（默认）：智能体主会话。
- 显式会话键（从 `openclaw sessions --json` 或 [会话 CLI](https://docs.openclaw.ai/zh-CN/cli/sessions) 复制）。
- 会话键格式：请参阅 [会话](https://docs.openclaw.ai/zh-CN/concepts/session) 和 [组](https://docs.openclaw.ai/zh-CN/channels/groups)。

[​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#param-target)

target

string

- `last`：递送到上次使用的外部渠道。
- 显式渠道：任何已配置的渠道或插件 ID，例如 `discord`、`matrix`、`telegram` 或 `whatsapp`。
- `none`（默认）：运行 Heartbeat，但 **不向外部递送**。

[​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#param-direct-policy)

directPolicy

"allow" \| "block"

默认值:"allow"

控制直接/私信递送行为。`allow`：允许直接/私信 Heartbeat 递送。`block`：禁止直接/私信递送（`reason=dm-blocked`）。

[​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#param-to)

to

string

可选的接收者覆盖（特定于渠道的 ID，例如 WhatsApp 的 E.164 或 Telegram 聊天 ID）。对于 Telegram 主题/线程，使用 `<chatId>:topic:<messageThreadId>`。

[​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#param-account-id)

accountId

string

多账户渠道的可选账户 ID。当 `target: "last"` 时，如果解析出的上次渠道支持账户，则账户 ID 会应用于该渠道；否则会被忽略。如果账户 ID 与解析出的渠道中已配置的账户不匹配，则会跳过递送。

[​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#param-prompt)

prompt

string

覆盖默认提示词正文（不合并）。

[​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#param-ack-max-chars)

ackMaxChars

number

默认值:"300"

`HEARTBEAT_OK` 后、递送前允许的最大字符数。

[​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#param-suppress-tool-error-warnings)

suppressToolErrorWarnings

boolean

为 true 时，在 Heartbeat 运行期间抑制工具错误警告载荷。

[​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat#param-active-hours)

activeHours

object

将 Heartbeat 运行限制在一个时间窗口内。对象包含 `start`（HH:MM，含起点；使用 `00:00` 表示一天开始）、`end`（HH:MM，不含终点；允许 `24:00` 表示一天结束），以及可选的 `timezone`。

- 省略或 `"user"`：如果已设置，则使用你的 `agents.defaults.userTimezone`，否则回退到主机系统时区。
- `"local"`：始终使用主机系统时区。
- 任意 IANA 标识符（例如 `America/New_York`）：直接使用；如果无效，则回退到上面的 `"user"` 行为。
- 对于活跃窗口，`start` 和 `end` 不能相等；相等的值会被视为零宽度（始终在窗口外）。
- 在活跃窗口之外，Heartbeat 会被跳过，直到窗口内的下一个 tick。

## [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E9%80%92%E9%80%81%E8%A1%8C%E4%B8%BA)  递送行为

会话和目标路由

- Heartbeat 默认在智能体的主会话中运行（`agent:<id>:<mainKey>`），或者当 `session.scope = "global"` 时在 `global` 中运行。设置 `session` 可覆盖为特定渠道会话（Discord/WhatsApp 等）。
- `session` 只影响运行上下文；递送由 `target` 和 `to` 控制。
- 要递送到特定渠道/接收者，请设置 `target` \+ `to`。使用 `target: "last"` 时，递送会使用该会话的最后一个外部渠道。
- Heartbeat 递送默认允许直接目标/私信目标。设置 `directPolicy: "block"` 可抑制直接目标发送，同时仍然运行 Heartbeat 回合。
- 如果主队列、目标会话 lane、cron lane 或活跃 cron 作业正忙，Heartbeat 会被跳过并稍后重试。
- 如果 `skipWhenBusy: true`，子智能体和嵌套 lane 也会推迟 Heartbeat 运行。
- 如果 `target` 解析不到外部目的地，运行仍会发生，但不会发送出站消息。

可见性和跳过行为

- 如果 `showOk`、`showAlerts` 和 `useIndicator` 全部禁用，运行会在前置阶段以 `reason=alerts-disabled` 跳过。
- 如果只禁用了告警递送，OpenClaw 仍可运行 Heartbeat、更新到期任务时间戳、恢复会话空闲时间戳，并抑制对外告警载荷。
- 如果解析出的 Heartbeat 目标支持 typing，OpenClaw 会在 Heartbeat 运行处于活跃状态时显示 typing。这会使用 Heartbeat 本应向其发送聊天输出的同一目标，并且可通过 `typingMode: "never"` 禁用。

会话生命周期和审计

- 仅 Heartbeat 的回复 **不会** 保持会话存活。Heartbeat 元数据可能会更新会话行，但空闲过期使用最后一条真实用户/渠道消息的 `lastInteractionAt`，每日过期使用 `sessionStartedAt`。
- Control UI 和 WebChat 历史会隐藏 Heartbeat 提示词和仅 OK 的确认。底层会话转录仍可包含这些回合，用于审计/重放。
- 分离的 [后台任务](https://docs.openclaw.ai/zh-CN/automation/tasks) 可以入队一个系统事件，并在主会话应快速注意到某些内容时唤醒 Heartbeat。该唤醒不会让 Heartbeat 运行变成后台任务。

## [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E5%8F%AF%E8%A7%81%E6%80%A7%E6%8E%A7%E5%88%B6)  可见性控制

默认情况下，`HEARTBEAT_OK` 确认会被抑制，而告警内容会被递送。你可以按渠道或按账号调整：

```
channels:
  defaults:
    heartbeat:
      showOk: false # Hide HEARTBEAT_OK (default)
      showAlerts: true # Show alert messages (default)
      useIndicator: true # Emit indicator events (default)
  telegram:
    heartbeat:
      showOk: true # Show OK acknowledgments on Telegram
  whatsapp:
    accounts:
      work:
        heartbeat:
          showAlerts: false # Suppress alert delivery for this account
```

优先级：按账号 → 按渠道 → 渠道默认值 → 内置默认值。

### [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E6%AF%8F%E4%B8%AA%E6%A0%87%E5%BF%97%E7%9A%84%E4%BD%9C%E7%94%A8)  每个标志的作用

- `showOk`：当模型返回仅 OK 的回复时，发送 `HEARTBEAT_OK` 确认。
- `showAlerts`：当模型返回非 OK 回复时，发送告警内容。
- `useIndicator`：为 UI Status 表面发出指示器事件。

如果 **三个** 都为 false，OpenClaw 会完全跳过 Heartbeat 运行（不会调用模型）。

### [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E6%8C%89%E6%B8%A0%E9%81%93%E4%B8%8E%E6%8C%89%E8%B4%A6%E5%8F%B7%E7%A4%BA%E4%BE%8B)  按渠道与按账号示例

```
channels:
  defaults:
    heartbeat:
      showOk: false
      showAlerts: true
      useIndicator: true
  slack:
    heartbeat:
      showOk: true # all Slack accounts
    accounts:
      ops:
        heartbeat:
          showAlerts: false # suppress alerts for the ops account only
  telegram:
    heartbeat:
      showOk: true
```

### [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E5%B8%B8%E8%A7%81%E6%A8%A1%E5%BC%8F)  常见模式

| 目标 | 配置 |
| --- | --- |
| 默认行为（静默 OK，开启告警） | _（无需配置）_ |
| 完全静默（无消息、无指示器） | `channels.defaults.heartbeat: { showOk: false, showAlerts: false, useIndicator: false }` |
| 仅指示器（无消息） | `channels.defaults.heartbeat: { showOk: false, showAlerts: false, useIndicator: true }` |
| 仅在一个渠道中显示 OK | `channels.telegram.heartbeat: { showOk: true }` |

## [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#heartbeat-md%EF%BC%88%E5%8F%AF%E9%80%89%EF%BC%89)  HEARTBEAT.md（可选）

如果工作区中存在 `HEARTBEAT.md` 文件，默认提示词会告诉智能体读取它。可以把它看作你的 “Heartbeat 检查清单”：小巧、稳定，并且可安全地每 30 分钟纳入一次。在正常运行中，只有为默认智能体启用 Heartbeat 指引时，才会注入 `HEARTBEAT.md`。用 `0m` 禁用 Heartbeat 节奏，或设置 `includeSystemPromptSection: false`，会将它从正常引导上下文中省略。如果 `HEARTBEAT.md` 存在但实际上为空（只有空行和类似 `# Heading` 的 markdown 标题），OpenClaw 会跳过 Heartbeat 运行以节省 API 调用。该跳过会报告为 `reason=empty-heartbeat-file`。如果文件缺失，Heartbeat 仍会运行，并由模型决定要做什么。保持精简（简短检查清单或提醒），以避免提示词膨胀。`HEARTBEAT.md` 示例：

```
# Heartbeat checklist

- Quick scan: anything urgent in inboxes?
- If it's daytime, do a lightweight check-in if nothing else is pending.
- If a task is blocked, write down _what is missing_ and ask Peter next time.
```

### [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#tasks-%E5%9D%97)  `tasks:` 块

`HEARTBEAT.md` 还支持一个小型结构化 `tasks:` 块，用于在 Heartbeat 本身内部进行基于间隔的检查。示例：

```
tasks:

- name: inbox-triage
  interval: 30m
  prompt: "Check for urgent unread emails and flag anything time sensitive."
- name: calendar-scan
  interval: 2h
  prompt: "Check for upcoming meetings that need prep or follow-up."

# Additional instructions

- Keep alerts short.
- If nothing needs attention after all due tasks, reply HEARTBEAT_OK.
```

行为

- OpenClaw 解析 `tasks:` 块，并按每个任务自己的 `interval` 检查它。
- 只有 **到期** 任务会被包含在该 tick 的 Heartbeat 提示词中。
- 如果没有任务到期，Heartbeat 会被完全跳过（`reason=no-tasks-due`），以避免浪费模型调用。
- `HEARTBEAT.md` 中的非任务内容会保留，并作为附加上下文追加到到期任务列表之后。
- 任务上次运行时间戳会存储在会话状态中（`heartbeatTaskState`），因此间隔能在正常重启后保留。
- 只有在 Heartbeat 运行完成其正常回复路径后，任务时间戳才会推进。被跳过的 `empty-heartbeat-file` / `no-tasks-due` 运行不会将任务标记为已完成。

当你希望一个 Heartbeat 文件保存多个周期性检查，但不想每个 tick 都为所有检查付费时，任务模式很有用。

### [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E6%99%BA%E8%83%BD%E4%BD%93%E5%8F%AF%E4%BB%A5%E6%9B%B4%E6%96%B0-heartbeat-md-%E5%90%97%EF%BC%9F)  智能体可以更新 HEARTBEAT.md 吗？

可以，只要你要求它这样做。`HEARTBEAT.md` 只是智能体工作区中的普通文件，所以你可以在普通聊天中告诉智能体，例如：

- “更新 `HEARTBEAT.md`，添加每日日历检查。”
- “重写 `HEARTBEAT.md`，让它更短，并专注于收件箱跟进。”

如果你希望这主动发生，也可以在 Heartbeat 提示词中加入明确的一行，例如：“如果检查清单变得过时，请用更好的版本更新 HEARTBEAT.md。”

不要把秘密（API key、电话号码、私有令牌）放进 `HEARTBEAT.md`，它会成为提示词上下文的一部分。

## [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E6%89%8B%E5%8A%A8%E5%94%A4%E9%86%92%EF%BC%88%E6%8C%89%E9%9C%80%EF%BC%89)  手动唤醒（按需）

你可以用以下命令入队一个系统事件并触发即时 Heartbeat：

```
openclaw system event --text "Check for urgent follow-ups" --mode now
```

如果多个智能体配置了 `heartbeat`，手动唤醒会立即运行这些智能体的每个 Heartbeat。使用 `--mode next-heartbeat` 等待下一个计划 tick。

## [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E6%8E%A8%E7%90%86%E9%80%92%E9%80%81%EF%BC%88%E5%8F%AF%E9%80%89%EF%BC%89)  推理递送（可选）

默认情况下，Heartbeat 只递送最终 “answer” 载荷。如果你想提高透明度，请启用：

- `agents.defaults.heartbeat.includeReasoning: true`

启用后，Heartbeat 还会递送一条以 `Reasoning:` 为前缀的单独消息（形状与 `/reasoning on` 相同）。当智能体管理多个会话/codex，并且你想了解它为何决定 ping 你时，这会很有用，但它也可能泄露比你想要的更多内部细节。建议在群聊中保持关闭。

## [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E6%88%90%E6%9C%AC%E6%84%8F%E8%AF%86)  成本意识

Heartbeat 会运行完整的智能体回合。更短的间隔会消耗更多 token。要降低成本：

- 使用 `isolatedSession: true`，避免发送完整对话历史（每次运行从约 100K token 降到约 2-5K）。
- 使用 `lightContext: true`，将引导文件限制为只有 `HEARTBEAT.md`。
- 设置更便宜的 `model`（例如 `ollama/llama3.2:1b`）。
- 保持 `HEARTBEAT.md` 小巧。
- 如果你只想更新内部状态，请使用 `target: "none"`。

## [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#heartbeat-%E5%90%8E%E7%9A%84%E4%B8%8A%E4%B8%8B%E6%96%87%E6%BA%A2%E5%87%BA)  Heartbeat 后的上下文溢出

如果某次 Heartbeat 之前把现有会话留在较小的本地模型上，例如窗口为 32k 的 Ollama 模型，并且下一次主会话回合报告上下文溢出，请将会话运行时模型重置回已配置的主模型。当最后一个运行时模型匹配已配置的 `heartbeat.model` 时，OpenClaw 的重置消息会指出这一点。当前 Heartbeat 会在运行完成后保留共享会话的现有运行时模型。你仍可使用 `isolatedSession: true` 在新会话中运行 Heartbeat，将它与 `lightContext: true` 结合以获得最小提示词，或选择上下文窗口足以容纳共享会话的 Heartbeat 模型。

## [​](https://docs.openclaw.ai/zh-CN/gateway/heartbeat\#%E7%9B%B8%E5%85%B3)  相关

- [自动化与任务](https://docs.openclaw.ai/zh-CN/automation) — 一览所有自动化机制
- [后台任务](https://docs.openclaw.ai/zh-CN/automation/tasks) — 分离工作如何被跟踪
- [时区](https://docs.openclaw.ai/zh-CN/concepts/timezone) — 时区如何影响 Heartbeat 调度
- [故障排除](https://docs.openclaw.ai/zh-CN/automation/cron-jobs#troubleshooting) — 调试自动化问题

[健康检查](https://docs.openclaw.ai/zh-CN/gateway/health) [Doctor](https://docs.openclaw.ai/zh-CN/gateway/doctor)

Ctrl+I