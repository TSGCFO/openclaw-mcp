---
source_url: https://docs.openclaw.ai/zh-CN/channels/feishu
title: "Feishu - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/channels/feishu#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

消息平台

Feishu

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [Feishu / Lark](https://docs.openclaw.ai/zh-CN/channels/feishu#feishu-%2F-lark)
- [快速开始](https://docs.openclaw.ai/zh-CN/channels/feishu#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)
- [访问控制](https://docs.openclaw.ai/zh-CN/channels/feishu#%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6)
- [私信](https://docs.openclaw.ai/zh-CN/channels/feishu#%E7%A7%81%E4%BF%A1)
- [群聊](https://docs.openclaw.ai/zh-CN/channels/feishu#%E7%BE%A4%E8%81%8A)
- [群组配置示例](https://docs.openclaw.ai/zh-CN/channels/feishu#%E7%BE%A4%E7%BB%84%E9%85%8D%E7%BD%AE%E7%A4%BA%E4%BE%8B)
- [允许所有群组，无需 @提及](https://docs.openclaw.ai/zh-CN/channels/feishu#%E5%85%81%E8%AE%B8%E6%89%80%E6%9C%89%E7%BE%A4%E7%BB%84%EF%BC%8C%E6%97%A0%E9%9C%80-%40%E6%8F%90%E5%8F%8A)
- [允许所有群组，仍然要求 @提及](https://docs.openclaw.ai/zh-CN/channels/feishu#%E5%85%81%E8%AE%B8%E6%89%80%E6%9C%89%E7%BE%A4%E7%BB%84%EF%BC%8C%E4%BB%8D%E7%84%B6%E8%A6%81%E6%B1%82-%40%E6%8F%90%E5%8F%8A)
- [仅允许指定群组](https://docs.openclaw.ai/zh-CN/channels/feishu#%E4%BB%85%E5%85%81%E8%AE%B8%E6%8C%87%E5%AE%9A%E7%BE%A4%E7%BB%84)
- [限制群组内的发送者](https://docs.openclaw.ai/zh-CN/channels/feishu#%E9%99%90%E5%88%B6%E7%BE%A4%E7%BB%84%E5%86%85%E7%9A%84%E5%8F%91%E9%80%81%E8%80%85)
- [获取群组/用户 ID](https://docs.openclaw.ai/zh-CN/channels/feishu#%E8%8E%B7%E5%8F%96%E7%BE%A4%E7%BB%84%2F%E7%94%A8%E6%88%B7-id)
- [群组 ID（chat\_id，格式：oc\_xxx）](https://docs.openclaw.ai/zh-CN/channels/feishu#%E7%BE%A4%E7%BB%84-id%EF%BC%88chat_id%EF%BC%8C%E6%A0%BC%E5%BC%8F%EF%BC%9Aoc_xxx%EF%BC%89)
- [用户 ID（open\_id，格式：ou\_xxx）](https://docs.openclaw.ai/zh-CN/channels/feishu#%E7%94%A8%E6%88%B7-id%EF%BC%88open_id%EF%BC%8C%E6%A0%BC%E5%BC%8F%EF%BC%9Aou_xxx%EF%BC%89)
- [常用命令](https://docs.openclaw.ai/zh-CN/channels/feishu#%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)
- [故障排除](https://docs.openclaw.ai/zh-CN/channels/feishu#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4)
- [机器人在群聊中没有响应](https://docs.openclaw.ai/zh-CN/channels/feishu#%E6%9C%BA%E5%99%A8%E4%BA%BA%E5%9C%A8%E7%BE%A4%E8%81%8A%E4%B8%AD%E6%B2%A1%E6%9C%89%E5%93%8D%E5%BA%94)
- [机器人没有收到消息](https://docs.openclaw.ai/zh-CN/channels/feishu#%E6%9C%BA%E5%99%A8%E4%BA%BA%E6%B2%A1%E6%9C%89%E6%94%B6%E5%88%B0%E6%B6%88%E6%81%AF)
- [App Secret 泄露](https://docs.openclaw.ai/zh-CN/channels/feishu#app-secret-%E6%B3%84%E9%9C%B2)
- [高级配置](https://docs.openclaw.ai/zh-CN/channels/feishu#%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE)
- [多账户](https://docs.openclaw.ai/zh-CN/channels/feishu#%E5%A4%9A%E8%B4%A6%E6%88%B7)
- [消息限制](https://docs.openclaw.ai/zh-CN/channels/feishu#%E6%B6%88%E6%81%AF%E9%99%90%E5%88%B6)
- [流式传输](https://docs.openclaw.ai/zh-CN/channels/feishu#%E6%B5%81%E5%BC%8F%E4%BC%A0%E8%BE%93)
- [配额优化](https://docs.openclaw.ai/zh-CN/channels/feishu#%E9%85%8D%E9%A2%9D%E4%BC%98%E5%8C%96)
- [ACP 会话](https://docs.openclaw.ai/zh-CN/channels/feishu#acp-%E4%BC%9A%E8%AF%9D)
- [持久 ACP 绑定](https://docs.openclaw.ai/zh-CN/channels/feishu#%E6%8C%81%E4%B9%85-acp-%E7%BB%91%E5%AE%9A)
- [从聊天中生成 ACP](https://docs.openclaw.ai/zh-CN/channels/feishu#%E4%BB%8E%E8%81%8A%E5%A4%A9%E4%B8%AD%E7%94%9F%E6%88%90-acp)
- [多智能体路由](https://docs.openclaw.ai/zh-CN/channels/feishu#%E5%A4%9A%E6%99%BA%E8%83%BD%E4%BD%93%E8%B7%AF%E7%94%B1)
- [配置参考](https://docs.openclaw.ai/zh-CN/channels/feishu#%E9%85%8D%E7%BD%AE%E5%8F%82%E8%80%83)
- [支持的消息类型](https://docs.openclaw.ai/zh-CN/channels/feishu#%E6%94%AF%E6%8C%81%E7%9A%84%E6%B6%88%E6%81%AF%E7%B1%BB%E5%9E%8B)
- [接收](https://docs.openclaw.ai/zh-CN/channels/feishu#%E6%8E%A5%E6%94%B6)
- [发送](https://docs.openclaw.ai/zh-CN/channels/feishu#%E5%8F%91%E9%80%81)
- [线程和回复](https://docs.openclaw.ai/zh-CN/channels/feishu#%E7%BA%BF%E7%A8%8B%E5%92%8C%E5%9B%9E%E5%A4%8D)
- [相关内容](https://docs.openclaw.ai/zh-CN/channels/feishu#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#feishu-/-lark)  Feishu / Lark

Feishu/Lark 是一体化协作平台，团队可在其中聊天、共享文档、管理日历并协同完成工作。**Status：** 已可用于生产环境中的机器人私信 \+ 群聊。WebSocket 是默认模式；webhook 模式是可选项。

* * *

## [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)  快速开始

需要 OpenClaw 2026.4.25 或更高版本。运行 `openclaw --version` 检查。使用 `openclaw update` 升级。

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/channels/feishu#)

运行渠道设置向导

```
openclaw channels login --channel feishu
```

使用你的 Feishu/Lark 移动应用扫描二维码，即可自动创建 Feishu/Lark 机器人。

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/channels/feishu#)

设置完成后，重启 Gateway 网关以应用更改

```
openclaw gateway restart
```

* * *

## [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6)  访问控制

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E7%A7%81%E4%BF%A1)  私信

配置 `dmPolicy` 来控制谁可以向机器人发送私信：

- `"pairing"` — 未知用户会收到配对码；通过 CLI 批准
- `"allowlist"` — 只有列在 `allowFrom` 中的用户可以聊天（默认：仅机器人所有者）
- `"open"` — 仅当 `allowFrom` 包含 `"*"` 时允许公开私信；如果使用限制性条目，只有匹配的用户可以聊天
- `"disabled"` — 禁用所有私信

**批准配对请求：**

```
openclaw pairing list feishu
openclaw pairing approve feishu <CODE>
```

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E7%BE%A4%E8%81%8A)  群聊

**群组策略**（`channels.feishu.groupPolicy`）：

| 值 | 行为 |
| --- | --- |
| `"open"` | 回复群组中的所有消息 |
| `"allowlist"` | 只回复 `groupAllowFrom` 中的群组，或在 `groups.<chat_id>` 下显式配置的群组 |
| `"disabled"` | 禁用所有群组消息；显式的 `groups.<chat_id>` 条目不会覆盖此设置 |

默认值：`allowlist`**提及要求**（`channels.feishu.requireMention`）：

- `true` — 要求 @提及（默认）
- `false` — 无需 @提及也会回复
- 按群组覆盖：`channels.feishu.groups.<chat_id>.requireMention`
- 仅用于广播的 `@all` 和 `@_all` 不会被视为机器人提及。同时提及 `@all` 和机器人本身的消息仍会算作机器人提及。

* * *

## [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E7%BE%A4%E7%BB%84%E9%85%8D%E7%BD%AE%E7%A4%BA%E4%BE%8B)  群组配置示例

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E5%85%81%E8%AE%B8%E6%89%80%E6%9C%89%E7%BE%A4%E7%BB%84%EF%BC%8C%E6%97%A0%E9%9C%80-@%E6%8F%90%E5%8F%8A)  允许所有群组，无需 @提及

```
{
  channels: {
    feishu: {
      groupPolicy: "open",
    },
  },
}
```

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E5%85%81%E8%AE%B8%E6%89%80%E6%9C%89%E7%BE%A4%E7%BB%84%EF%BC%8C%E4%BB%8D%E7%84%B6%E8%A6%81%E6%B1%82-@%E6%8F%90%E5%8F%8A)  允许所有群组，仍然要求 @提及

```
{
  channels: {
    feishu: {
      groupPolicy: "open",
      requireMention: true,
    },
  },
}
```

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E4%BB%85%E5%85%81%E8%AE%B8%E6%8C%87%E5%AE%9A%E7%BE%A4%E7%BB%84)  仅允许指定群组

```
{
  channels: {
    feishu: {
      groupPolicy: "allowlist",
      // Group IDs look like: oc_xxx
      groupAllowFrom: ["oc_xxx", "oc_yyy"],
    },
  },
}
```

在 `allowlist` 模式下，你也可以通过添加显式的 `groups.<chat_id>` 条目来接纳某个群组。显式条目不会覆盖 `groupPolicy: "disabled"`。`groups.*` 下的通配符默认值会配置匹配的群组，但它们本身不会接纳群组。

```
{
  channels: {
    feishu: {
      groupPolicy: "allowlist",
      groups: {
        oc_xxx: {
          requireMention: false,
        },
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E9%99%90%E5%88%B6%E7%BE%A4%E7%BB%84%E5%86%85%E7%9A%84%E5%8F%91%E9%80%81%E8%80%85)  限制群组内的发送者

```
{
  channels: {
    feishu: {
      groupPolicy: "allowlist",
      groupAllowFrom: ["oc_xxx"],
      groups: {
        oc_xxx: {
          // User open_ids look like: ou_xxx
          allowFrom: ["ou_user1", "ou_user2"],
        },
      },
    },
  },
}
```

* * *

## [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E8%8E%B7%E5%8F%96%E7%BE%A4%E7%BB%84/%E7%94%A8%E6%88%B7-id)  获取群组/用户 ID

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E7%BE%A4%E7%BB%84-id%EF%BC%88chat_id%EF%BC%8C%E6%A0%BC%E5%BC%8F%EF%BC%9Aoc_xxx%EF%BC%89)  群组 ID（`chat_id`，格式：`oc_xxx`）

在 Feishu/Lark 中打开群组，点击右上角的菜单图标，然后进入 **设置**。群组 ID（`chat_id`）会列在设置页面上。![获取群组 ID](https://mintcdn.com/clawdhub/0NpU6wNaI7exeaOE/images/feishu-get-group-id.png?fit=max&auto=format&n=0NpU6wNaI7exeaOE&q=85&s=1c9b41e1f9743621dfdd3abf7e952405)

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E7%94%A8%E6%88%B7-id%EF%BC%88open_id%EF%BC%8C%E6%A0%BC%E5%BC%8F%EF%BC%9Aou_xxx%EF%BC%89)  用户 ID（`open_id`，格式：`ou_xxx`）

启动 Gateway 网关，向机器人发送私信，然后检查日志：

```
openclaw logs --follow
```

在日志输出中查找 `open_id`。你也可以检查待处理的配对请求：

```
openclaw pairing list feishu
```

* * *

## [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)  常用命令

| 命令 | 描述 |
| --- | --- |
| `/status` | 显示机器人状态 |
| `/reset` | 重置当前会话 |
| `/model` | 显示或切换 AI 模型 |

Feishu/Lark 不支持原生斜杠菜单，因此请将这些命令作为纯文本消息发送。

* * *

## [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4)  故障排除

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E6%9C%BA%E5%99%A8%E4%BA%BA%E5%9C%A8%E7%BE%A4%E8%81%8A%E4%B8%AD%E6%B2%A1%E6%9C%89%E5%93%8D%E5%BA%94)  机器人在群聊中没有响应

1. 确保机器人已添加到群组
2. 确保你 @提及了机器人（默认要求）
3. 验证 `groupPolicy` 不是 `"disabled"`
4. 检查日志：`openclaw logs --follow`

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E6%9C%BA%E5%99%A8%E4%BA%BA%E6%B2%A1%E6%9C%89%E6%94%B6%E5%88%B0%E6%B6%88%E6%81%AF)  机器人没有收到消息

1. 确保机器人已在 Feishu Open Platform / Lark Developer 中发布并获批
2. 确保事件订阅包含 `im.message.receive_v1`
3. 确保已选择 **持久连接**（WebSocket）
4. 确保已授予所有必需的权限范围
5. 确保 Gateway 网关正在运行：`openclaw gateway status`
6. 检查日志：`openclaw logs --follow`

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#app-secret-%E6%B3%84%E9%9C%B2)  App Secret 泄露

1. 在 Feishu Open Platform / Lark Developer 中重置 App Secret
2. 在你的配置中更新该值
3. 重启 Gateway 网关：`openclaw gateway restart`

* * *

## [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E9%AB%98%E7%BA%A7%E9%85%8D%E7%BD%AE)  高级配置

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E5%A4%9A%E8%B4%A6%E6%88%B7)  多账户

```
{
  channels: {
    feishu: {
      defaultAccount: "main",
      accounts: {
        main: {
          appId: "cli_xxx",
          appSecret: "xxx",
          name: "Primary bot",
          tts: {
            providers: {
              openai: { voice: "shimmer" },
            },
          },
        },
        backup: {
          appId: "cli_yyy",
          appSecret: "yyy",
          name: "Backup bot",
          enabled: false,
        },
      },
    },
  },
}
```

`defaultAccount` 控制在出站 API 未指定 `accountId` 时使用哪个账户。
`accounts.<id>.tts` 使用与 `messages.tts` 相同的形状，并会深度合并到全局 TTS 配置之上，因此多机器人 Feishu 设置可以在全局保留共享的提供商凭证，同时仅按账户覆盖语音、模型、角色或自动模式。

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E6%B6%88%E6%81%AF%E9%99%90%E5%88%B6)  消息限制

- `textChunkLimit` — 出站文本分块大小（默认：`2000` 个字符）
- `mediaMaxMb` — 媒体上传/下载限制（默认：`30` MB）

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E6%B5%81%E5%BC%8F%E4%BC%A0%E8%BE%93)  流式传输

Feishu/Lark 支持通过交互式卡片进行流式回复。启用后，机器人会在生成文本时实时更新卡片。

```
{
  channels: {
    feishu: {
      streaming: true, // enable streaming card output (default: true)
      blockStreaming: true, // enable block-level streaming (default: true)
    },
  },
}
```

设置 `streaming: false` 可在一条消息中发送完整回复。

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E9%85%8D%E9%A2%9D%E4%BC%98%E5%8C%96)  配额优化

使用两个可选标志减少 Feishu/Lark API 调用次数：

- `typingIndicator`（默认 `true`）：设置为 `false` 可跳过输入中反应调用
- `resolveSenderNames`（默认 `true`）：设置为 `false` 可跳过发送者资料查询

```
{
  channels: {
    feishu: {
      typingIndicator: false,
      resolveSenderNames: false,
    },
  },
}
```

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#acp-%E4%BC%9A%E8%AF%9D)  ACP 会话

Feishu/Lark 支持用于私信和群组线程消息的 ACP。Feishu/Lark ACP 由文本命令驱动，没有原生斜杠菜单，因此请直接在对话中使用 `/acp ...` 消息。

#### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E6%8C%81%E4%B9%85-acp-%E7%BB%91%E5%AE%9A)  持久 ACP 绑定

```
{
  agents: {
    list: [\
      {\
        id: "codex",\
        runtime: {\
          type: "acp",\
          acp: {\
            agent: "codex",\
            backend: "acpx",\
            mode: "persistent",\
            cwd: "/workspace/openclaw",\
          },\
        },\
      },\
    ],
  },
  bindings: [\
    {\
      type: "acp",\
      agentId: "codex",\
      match: {\
        channel: "feishu",\
        accountId: "default",\
        peer: { kind: "direct", id: "ou_1234567890" },\
      },\
    },\
    {\
      type: "acp",\
      agentId: "codex",\
      match: {\
        channel: "feishu",\
        accountId: "default",\
        peer: { kind: "group", id: "oc_group_chat:topic:om_topic_root" },\
      },\
      acp: { label: "codex-feishu-topic" },\
    },\
  ],
}
```

#### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E4%BB%8E%E8%81%8A%E5%A4%A9%E4%B8%AD%E7%94%9F%E6%88%90-acp)  从聊天中生成 ACP

在 Feishu/Lark 私信或线程中：

```
/acp spawn codex --thread here
```

`--thread here` 适用于私信和 Feishu/Lark 线程消息。绑定对话中的后续消息会直接路由到该 ACP 会话。

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E5%A4%9A%E6%99%BA%E8%83%BD%E4%BD%93%E8%B7%AF%E7%94%B1)  多智能体路由

使用 `bindings` 将 Feishu/Lark 私信或群组路由到不同智能体。

```
{
  agents: {
    list: [\
      { id: "main" },\
      { id: "agent-a", workspace: "/home/user/agent-a" },\
      { id: "agent-b", workspace: "/home/user/agent-b" },\
    ],
  },
  bindings: [\
    {\
      agentId: "agent-a",\
      match: {\
        channel: "feishu",\
        peer: { kind: "direct", id: "ou_xxx" },\
      },\
    },\
    {\
      agentId: "agent-b",\
      match: {\
        channel: "feishu",\
        peer: { kind: "group", id: "oc_zzz" },\
      },\
    },\
  ],
}
```

路由字段：

- `match.channel`：`"feishu"`
- `match.peer.kind`：`"direct"`（私信）或 `"group"`（群聊）
- `match.peer.id`：用户 Open ID（`ou_xxx`）或群组 ID（`oc_xxx`）

请参阅 [获取群组/用户 ID](https://docs.openclaw.ai/zh-CN/channels/feishu#get-groupuser-ids) 了解查找提示。

* * *

## [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E9%85%8D%E7%BD%AE%E5%8F%82%E8%80%83)  配置参考

完整配置： [Gateway 网关配置](https://docs.openclaw.ai/zh-CN/gateway/configuration)

| 设置 | 描述 | 默认值 |
| --- | --- | --- |
| `channels.feishu.enabled` | 启用/停用该渠道 | `true` |
| `channels.feishu.domain` | API 域名（`feishu` 或 `lark`） | `feishu` |
| `channels.feishu.connectionMode` | 事件传输方式（`websocket` 或 `webhook`） | `websocket` |
| `channels.feishu.defaultAccount` | 用于出站路由的默认账户 | `default` |
| `channels.feishu.verificationToken` | webhook 模式必需 | — |
| `channels.feishu.encryptKey` | webhook 模式必需 | — |
| `channels.feishu.webhookPath` | Webhook 路由路径 | `/feishu/events` |
| `channels.feishu.webhookHost` | Webhook 绑定主机 | `127.0.0.1` |
| `channels.feishu.webhookPort` | Webhook 绑定端口 | `3000` |
| `channels.feishu.accounts.<id>.appId` | App ID | — |
| `channels.feishu.accounts.<id>.appSecret` | App Secret | — |
| `channels.feishu.accounts.<id>.domain` | 每个账户的域名覆盖设置 | `feishu` |
| `channels.feishu.accounts.<id>.tts` | 每个账户的 TTS 覆盖设置 | `messages.tts` |
| `channels.feishu.dmPolicy` | 私信策略 | `allowlist` |
| `channels.feishu.allowFrom` | 私信允许列表（open\_id 列表） | \[BotOwnerId\] |
| `channels.feishu.groupPolicy` | 群组策略 | `allowlist` |
| `channels.feishu.groupAllowFrom` | 群组允许列表 | — |
| `channels.feishu.requireMention` | 群组中要求 @mention | `true` |
| `channels.feishu.groups.<chat_id>.requireMention` | 每个群组的 @mention 覆盖设置；显式 ID 在允许列表模式下也会准入该群组 | inherited |
| `channels.feishu.groups.<chat_id>.enabled` | 启用/停用特定群组 | `true` |
| `channels.feishu.textChunkLimit` | 消息分块大小 | `2000` |
| `channels.feishu.mediaMaxMb` | 媒体大小限制 | `30` |
| `channels.feishu.streaming` | 流式卡片输出 | `true` |
| `channels.feishu.blockStreaming` | 块级流式传输 | `true` |
| `channels.feishu.typingIndicator` | 发送正在输入反应 | `true` |
| `channels.feishu.resolveSenderNames` | 解析发送者显示名称 | `true` |

* * *

## [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E6%94%AF%E6%8C%81%E7%9A%84%E6%B6%88%E6%81%AF%E7%B1%BB%E5%9E%8B)  支持的消息类型

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E6%8E%A5%E6%94%B6)  接收

- ✅ 文本
- ✅ 富文本（post）
- ✅ 图片
- ✅ 文件
- ✅ 音频
- ✅ 视频/媒体
- ✅ 表情贴纸

入站 Feishu/Lark 音频消息会被规范化为媒体占位符，而不是原始 `file_key` JSON。当配置了 `tools.media.audio` 时，OpenClaw 会下载语音留言资源，并在智能体轮次开始前运行共享音频转写，因此智能体会收到语音转写文本。如果 Feishu 在音频载荷中直接包含转写文本，则会直接使用该文本，不再进行另一次 ASR 调用。如果没有音频转写提供商，智能体仍会收到一个 `<media:audio>` 占位符和已保存的附件，而不是原始 Feishu 资源载荷。

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E5%8F%91%E9%80%81)  发送

- ✅ 文本
- ✅ 图片
- ✅ 文件
- ✅ 音频
- ✅ 视频/媒体
- ✅ 交互式卡片（包括流式更新）
- ⚠️ 富文本（post 风格格式；不支持完整 Feishu/Lark 创作能力）

原生 Feishu/Lark 音频气泡使用 Feishu `audio` 消息类型，并要求上传 Ogg/Opus 媒体（`file_type: "opus"`）。已有的 `.opus` 和 `.ogg` 媒体会直接作为原生音频发送。只有当回复请求语音递送（`audioAsVoice` / 消息工具 `asVoice`，包括 TTS 语音留言回复）时，MP3/WAV/M4A 和其他可能的音频格式才会通过 `ffmpeg` 转码为 48kHz Ogg/Opus。普通 MP3 附件仍作为常规文件发送。如果缺少 `ffmpeg` 或转换失败，OpenClaw 会回退为文件附件并记录原因。

### [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E7%BA%BF%E7%A8%8B%E5%92%8C%E5%9B%9E%E5%A4%8D)  线程和回复

- ✅ 行内回复
- ✅ 线程回复
- ✅ 回复线程消息时，媒体回复会保持线程感知

对于 `groupSessionScope: "group_topic"` 和 `"group_topic_sender"`，原生 Feishu/Lark 话题群使用事件 `thread_id`（`omt_*`）作为规范的话题会话键。OpenClaw 转换成线程的普通群组回复会继续使用回复根消息 ID（`om_*`），因此第一轮和后续轮次会保持在同一会话中。

* * *

## [​](https://docs.openclaw.ai/zh-CN/channels/feishu\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

- [渠道概览](https://docs.openclaw.ai/zh-CN/channels) — 所有支持的渠道
- [配对](https://docs.openclaw.ai/zh-CN/channels/pairing) — 私信认证和配对流程
- [群组](https://docs.openclaw.ai/zh-CN/channels/groups) — 群聊行为和提及门控
- [渠道路由](https://docs.openclaw.ai/zh-CN/channels/channel-routing) — 消息的会话路由
- [安全](https://docs.openclaw.ai/zh-CN/gateway/security) — 访问模型和加固

[Discord](https://docs.openclaw.ai/zh-CN/channels/discord) [Grammy](https://docs.openclaw.ai/zh-CN/channels/grammy)

Ctrl+I

![获取群组 ID](https://mintcdn.com/clawdhub/0NpU6wNaI7exeaOE/images/feishu-get-group-id.png?w=1100&fit=max&auto=format&n=0NpU6wNaI7exeaOE&q=85&s=36df634e2caf2690c29c722f5068b77b)