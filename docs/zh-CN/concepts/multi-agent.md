---
source_url: https://docs.openclaw.ai/zh-CN/concepts/multi-agent
title: "\u591a\u667a\u80fd\u4f53\u8def\u7531 - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

多代理

多智能体路由

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [什么是“一个智能体”？](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#%E4%BB%80%E4%B9%88%E6%98%AF%E2%80%9C%E4%B8%80%E4%B8%AA%E6%99%BA%E8%83%BD%E4%BD%93%E2%80%9D%EF%BC%9F)
- [路径（快速映射）](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#%E8%B7%AF%E5%BE%84%EF%BC%88%E5%BF%AB%E9%80%9F%E6%98%A0%E5%B0%84%EF%BC%89)
- [单智能体模式（默认）](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#%E5%8D%95%E6%99%BA%E8%83%BD%E4%BD%93%E6%A8%A1%E5%BC%8F%EF%BC%88%E9%BB%98%E8%AE%A4%EF%BC%89)
- [智能体辅助工具](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#%E6%99%BA%E8%83%BD%E4%BD%93%E8%BE%85%E5%8A%A9%E5%B7%A5%E5%85%B7)
- [快速开始](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)
- [多个智能体 = 多个人、多种个性](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#%E5%A4%9A%E4%B8%AA%E6%99%BA%E8%83%BD%E4%BD%93-%3D-%E5%A4%9A%E4%B8%AA%E4%BA%BA%E3%80%81%E5%A4%9A%E7%A7%8D%E4%B8%AA%E6%80%A7)
- [跨智能体 QMD 记忆搜索](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#%E8%B7%A8%E6%99%BA%E8%83%BD%E4%BD%93-qmd-%E8%AE%B0%E5%BF%86%E6%90%9C%E7%B4%A2)
- [一个 WhatsApp 号码，多个人（私信拆分）](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#%E4%B8%80%E4%B8%AA-whatsapp-%E5%8F%B7%E7%A0%81%EF%BC%8C%E5%A4%9A%E4%B8%AA%E4%BA%BA%EF%BC%88%E7%A7%81%E4%BF%A1%E6%8B%86%E5%88%86%EF%BC%89)
- [路由规则（消息如何选择智能体）](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#%E8%B7%AF%E7%94%B1%E8%A7%84%E5%88%99%EF%BC%88%E6%B6%88%E6%81%AF%E5%A6%82%E4%BD%95%E9%80%89%E6%8B%A9%E6%99%BA%E8%83%BD%E4%BD%93%EF%BC%89)
- [多账号/电话号码](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#%E5%A4%9A%E8%B4%A6%E5%8F%B7%2F%E7%94%B5%E8%AF%9D%E5%8F%B7%E7%A0%81)
- [概念](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#%E6%A6%82%E5%BF%B5)
- [平台示例](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#%E5%B9%B3%E5%8F%B0%E7%A4%BA%E4%BE%8B)
- [常见模式](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#%E5%B8%B8%E8%A7%81%E6%A8%A1%E5%BC%8F)
- [每个智能体的沙箱和工具配置](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#%E6%AF%8F%E4%B8%AA%E6%99%BA%E8%83%BD%E4%BD%93%E7%9A%84%E6%B2%99%E7%AE%B1%E5%92%8C%E5%B7%A5%E5%85%B7%E9%85%8D%E7%BD%AE)
- [相关内容](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

运行多个\_隔离\_智能体，每个智能体都有自己的工作区、状态目录（`agentDir`）和会话历史；同时还可以在一个正在运行的 Gateway 网关中使用多个渠道账号（例如两个 WhatsApp 账号）。入站消息会通过绑定路由到正确的智能体。这里的 **智能体** 是完整的每角色作用域：工作区文件、认证配置文件、模型注册表和会话存储。`agentDir` 是磁盘上的状态目录，位于 `~/.openclaw/agents/<agentId>/`，用于保存此每智能体配置。 **绑定** 会将一个渠道账号（例如 Slack 工作区或 WhatsApp 号码）映射到这些智能体之一。

## [​](https://docs.openclaw.ai/zh-CN/concepts/multi-agent\#%E4%BB%80%E4%B9%88%E6%98%AF%E2%80%9C%E4%B8%80%E4%B8%AA%E6%99%BA%E8%83%BD%E4%BD%93%E2%80%9D%EF%BC%9F)  什么是“一个智能体”？

**智能体** 是一个完全限定作用域的大脑，拥有自己的：

- **工作区**（文件、AGENTS.md/SOUL.md/USER.md、本地笔记、角色规则）。
- **状态目录**（`agentDir`），用于认证配置文件、模型注册表和每智能体配置。
- **会话存储**（聊天历史 \+ 路由状态），位于 `~/.openclaw/agents/<agentId>/sessions` 下。

认证配置文件按 **每智能体** 隔离。每个智能体都会从自己的位置读取：

```
~/.openclaw/agents/<agentId>/agent/auth-profiles.json
```

`session_history` 在这里也是更安全的跨会话回忆路径：它返回的是有界且经过清理的视图，而不是原始转录的直接转储。助手回忆会在脱敏/截断前移除思考标签、`<relevant-memories>` 脚手架、明文工具调用 XML 载荷（包括 `<tool_call>...</tool_call>`、`<function_call>...</function_call>`、`<tool_calls>...</tool_calls>`、`<function_calls>...</function_calls>` 和被截断的工具调用块）、降级后的工具调用脚手架、泄漏的 ASCII/全角模型控制令牌，以及格式错误的 MiniMax 工具调用 XML。

不要在多个智能体之间复用 `agentDir`（这会导致认证/会话冲突）。当智能体没有本地配置文件时，可以回退读取默认/主智能体的认证配置文件，但 OpenClaw 不会把 OAuth 刷新令牌克隆到次级智能体存储中。如果你想使用独立的 OAuth 账号，请从该智能体登录；如果你手动复制凭证，只复制可移植的静态 `api_key` 或 `token` 配置文件。

Skills 会从每个智能体工作区以及 `~/.openclaw/skills` 等共享根目录加载，然后在配置后按生效的智能体 Skills 允许列表过滤。使用 `agents.defaults.skills` 设置共享基线，使用 `agents.list[].skills` 进行每智能体替换。参见 [Skills：每智能体与共享](https://docs.openclaw.ai/zh-CN/tools/skills#per-agent-vs-shared-skills) 和 [Skills：智能体 Skills 允许列表](https://docs.openclaw.ai/zh-CN/tools/skills#agent-skill-allowlists)。Gateway 网关可以托管 **一个智能体**（默认）或 **多个智能体** 并排运行。

**工作区说明：每个智能体的工作区是默认 cwd**，不是硬性沙箱。相对路径会在工作区内解析，但除非启用沙箱隔离，否则绝对路径可以访问主机上的其他位置。参见 [沙箱隔离](https://docs.openclaw.ai/zh-CN/gateway/sandboxing)。

## [​](https://docs.openclaw.ai/zh-CN/concepts/multi-agent\#%E8%B7%AF%E5%BE%84%EF%BC%88%E5%BF%AB%E9%80%9F%E6%98%A0%E5%B0%84%EF%BC%89)  路径（快速映射）

- 配置：`~/.openclaw/openclaw.json`（或 `OPENCLAW_CONFIG_PATH`）
- 状态目录：`~/.openclaw`（或 `OPENCLAW_STATE_DIR`）
- 工作区：`~/.openclaw/workspace`（或 `~/.openclaw/workspace-<agentId>`）
- 智能体目录：`~/.openclaw/agents/<agentId>/agent`（或 `agents.list[].agentDir`）
- 会话：`~/.openclaw/agents/<agentId>/sessions`

### [​](https://docs.openclaw.ai/zh-CN/concepts/multi-agent\#%E5%8D%95%E6%99%BA%E8%83%BD%E4%BD%93%E6%A8%A1%E5%BC%8F%EF%BC%88%E9%BB%98%E8%AE%A4%EF%BC%89)  单智能体模式（默认）

如果你什么都不做，OpenClaw 会运行一个智能体：

- `agentId` 默认是 **`main`**。
- 会话键形如 `agent:main:<mainKey>`。
- 工作区默认是 `~/.openclaw/workspace`（设置 `OPENCLAW_PROFILE` 时则为 `~/.openclaw/workspace-<profile>`）。
- 状态默认位于 `~/.openclaw/agents/main/agent`。

## [​](https://docs.openclaw.ai/zh-CN/concepts/multi-agent\#%E6%99%BA%E8%83%BD%E4%BD%93%E8%BE%85%E5%8A%A9%E5%B7%A5%E5%85%B7)  智能体辅助工具

使用智能体向导添加新的隔离智能体：

```
openclaw agents add work
```

然后添加 `bindings`（或让向导完成）以路由入站消息。用以下命令验证：

```
openclaw agents list --bindings
```

## [​](https://docs.openclaw.ai/zh-CN/concepts/multi-agent\#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)  快速开始

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#)

创建每个智能体工作区

使用向导，或手动创建工作区：

```
openclaw agents add coding
openclaw agents add social
```

每个智能体都会获得自己的工作区，其中包含 `SOUL.md`、`AGENTS.md` 和可选的 `USER.md`，并在 `~/.openclaw/agents/<agentId>` 下拥有专用 `agentDir` 和会话存储。

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#)

创建渠道账号

在你偏好的渠道上为每个智能体创建一个账号：

- Discord：每个智能体一个机器人，启用消息内容意图，复制每个令牌。
- Telegram：通过 BotFather 为每个智能体创建一个机器人，复制每个令牌。
- WhatsApp：为每个账号关联各自的电话号码。

```
openclaw channels login --channel whatsapp --account work
```

参见渠道指南： [Discord](https://docs.openclaw.ai/zh-CN/channels/discord)、 [Telegram](https://docs.openclaw.ai/zh-CN/channels/telegram)、 [WhatsApp](https://docs.openclaw.ai/zh-CN/channels/whatsapp)。

3

[Navigate to header](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#)

添加智能体、账号和绑定

在 `agents.list` 下添加智能体，在 `channels.<channel>.accounts` 下添加渠道账号，并用 `bindings` 将它们连接起来（示例如下）。

4

[Navigate to header](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#)

重启并验证

```
openclaw gateway restart
openclaw agents list --bindings
openclaw channels status --probe
```

## [​](https://docs.openclaw.ai/zh-CN/concepts/multi-agent\#%E5%A4%9A%E4%B8%AA%E6%99%BA%E8%83%BD%E4%BD%93-=-%E5%A4%9A%E4%B8%AA%E4%BA%BA%E3%80%81%E5%A4%9A%E7%A7%8D%E4%B8%AA%E6%80%A7)  多个智能体 = 多个人、多种个性

使用 **多个智能体** 时，每个 `agentId` 都会变成一个 **完全隔离的角色**：

- **不同的电话号码/账号**（每渠道一个 `accountId`）。
- **不同的个性**（每智能体工作区文件，例如 `AGENTS.md` 和 `SOUL.md`）。
- **独立的认证 \+ 会话**（除非显式启用，否则不会串话）。

这让 **多个人** 可以共享一台 Gateway 网关服务器，同时保持他们的 AI “大脑”和数据彼此隔离。

## [​](https://docs.openclaw.ai/zh-CN/concepts/multi-agent\#%E8%B7%A8%E6%99%BA%E8%83%BD%E4%BD%93-qmd-%E8%AE%B0%E5%BF%86%E6%90%9C%E7%B4%A2)  跨智能体 QMD 记忆搜索

如果一个智能体需要搜索另一个智能体的 QMD 会话转录，请在 `agents.list[].memorySearch.qmd.extraCollections` 下添加额外集合。只有当每个智能体都应继承同一组共享转录集合时，才使用 `agents.defaults.memorySearch.qmd.extraCollections`。

```
{
  agents: {
    defaults: {
      workspace: "~/workspaces/main",
      memorySearch: {
        qmd: {
          extraCollections: [{ path: "~/agents/family/sessions", name: "family-sessions" }],
        },
      },
    },
    list: [\
      {\
        id: "main",\
        workspace: "~/workspaces/main",\
        memorySearch: {\
          qmd: {\
            extraCollections: [{ path: "notes" }], // resolves inside workspace -> collection named "notes-main"\
          },\
        },\
      },\
      { id: "family", workspace: "~/workspaces/family" },\
    ],
  },
  memory: {
    backend: "qmd",
    qmd: { includeDefaultMemory: false },
  },
}
```

额外集合路径可以在多个智能体之间共享，但当路径位于智能体工作区之外时，集合名称会保持显式。工作区内的路径仍然限定在智能体作用域内，因此每个智能体都会保留自己的转录搜索集。

## [​](https://docs.openclaw.ai/zh-CN/concepts/multi-agent\#%E4%B8%80%E4%B8%AA-whatsapp-%E5%8F%B7%E7%A0%81%EF%BC%8C%E5%A4%9A%E4%B8%AA%E4%BA%BA%EF%BC%88%E7%A7%81%E4%BF%A1%E6%8B%86%E5%88%86%EF%BC%89)  一个 WhatsApp 号码，多个人（私信拆分）

你可以在 **一个 WhatsApp 账号** 上，将 **不同的 WhatsApp 私信** 路由到不同智能体。使用发送方 E.164（例如 `+15551234567`）并配合 `peer.kind: "direct"` 进行匹配。回复仍然来自同一个 WhatsApp 号码（没有按智能体区分的发件身份）。

直接聊天会折叠到智能体的 **主会话键**，因此真正隔离需要 **每个人一个智能体**。

示例：

```
{
  agents: {
    list: [\
      { id: "alex", workspace: "~/.openclaw/workspace-alex" },\
      { id: "mia", workspace: "~/.openclaw/workspace-mia" },\
    ],
  },
  bindings: [\
    {\
      agentId: "alex",\
      match: { channel: "whatsapp", peer: { kind: "direct", id: "+15551230001" } },\
    },\
    {\
      agentId: "mia",\
      match: { channel: "whatsapp", peer: { kind: "direct", id: "+15551230002" } },\
    },\
  ],
  channels: {
    whatsapp: {
      dmPolicy: "allowlist",
      allowFrom: ["+15551230001", "+15551230002"],
    },
  },
}
```

说明：

- 私信访问控制是 **每个 WhatsApp 账号全局** 的（配对/允许列表），不是按智能体隔离。
- 对于共享群组，请将群组绑定到一个智能体，或使用 [广播群组](https://docs.openclaw.ai/zh-CN/channels/broadcast-groups)。

## [​](https://docs.openclaw.ai/zh-CN/concepts/multi-agent\#%E8%B7%AF%E7%94%B1%E8%A7%84%E5%88%99%EF%BC%88%E6%B6%88%E6%81%AF%E5%A6%82%E4%BD%95%E9%80%89%E6%8B%A9%E6%99%BA%E8%83%BD%E4%BD%93%EF%BC%89)  路由规则（消息如何选择智能体）

绑定是 **确定性的**，并且 **最具体者优先**：

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#)

peer 匹配

精确的私信/群组/渠道 ID。

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#)

parentPeer 匹配

线程继承。

3

[Navigate to header](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#)

guildId + roles

Discord 角色路由。

4

[Navigate to header](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#)

guildId

Discord。

5

[Navigate to header](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#)

teamId

Slack。

6

[Navigate to header](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#)

渠道的 accountId 匹配

每账号回退。

7

[Navigate to header](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#)

渠道级匹配

`accountId: "*"`。

8

[Navigate to header](https://docs.openclaw.ai/zh-CN/concepts/multi-agent#)

默认智能体

回退到 `agents.list[].default`，否则使用列表中的第一个条目，默认值：`main`。

平局判定和 AND 语义

- 如果同一层级中有多个绑定匹配，则配置顺序中的第一个获胜。
- 如果一个绑定设置了多个匹配字段（例如 `peer` \+ `guildId`），则所有指定字段都是必需的（`AND` 语义）。

账号作用域细节

- 省略 `accountId` 的绑定只匹配默认账号。
- 对所有账号使用 `accountId: "*"` 作为渠道范围回退。
- 如果你之后为同一智能体添加带有显式账号 ID 的相同绑定，OpenClaw 会将现有的仅渠道绑定升级为账号作用域绑定，而不是复制它。

## [​](https://docs.openclaw.ai/zh-CN/concepts/multi-agent\#%E5%A4%9A%E8%B4%A6%E5%8F%B7/%E7%94%B5%E8%AF%9D%E5%8F%B7%E7%A0%81)  多账号/电话号码

支持 **多账号** 的渠道（例如 WhatsApp）使用 `accountId` 来标识每次登录。每个 `accountId` 都可以路由到不同智能体，因此一台服务器可以托管多个电话号码，而不会混淆会话。如果你想在省略 `accountId` 时使用渠道范围默认账号，请设置 `channels.<channel>.defaultAccount`（可选）。未设置时，OpenClaw 会回退到 `default`（如果存在），否则使用排序后的第一个已配置账号 ID。支持此模式的常见渠道包括：

- `whatsapp`、`telegram`、`discord`、`slack`、`signal`、`imessage`
- `irc`、`line`、`googlechat`、`mattermost`、`matrix`、`nextcloud-talk`
- `bluebubbles`、`zalo`、`zalouser`、`nostr`、`feishu`

## [​](https://docs.openclaw.ai/zh-CN/concepts/multi-agent\#%E6%A6%82%E5%BF%B5)  概念

- `agentId`：一个“大脑”（工作区、每智能体认证、每智能体会话存储）。
- `accountId`：一个渠道账号实例（例如 WhatsApp 账号 `"personal"` 与 `"biz"`）。
- `binding`：按 `(channel, accountId, peer)` 以及可选的服务器/团队 ID 将入站消息路由到 `agentId`。
- 直接聊天会折叠到 `agent:<agentId>:<mainKey>`（每智能体的 `main`；`session.mainKey`）。

## [​](https://docs.openclaw.ai/zh-CN/concepts/multi-agent\#%E5%B9%B3%E5%8F%B0%E7%A4%BA%E4%BE%8B)  平台示例

每智能体一个 Discord 机器人

每个 Discord 机器人账号都会映射到一个唯一的 `accountId`。将每个账号绑定到一个智能体，并按机器人保留允许列表。

```
{
  agents: {
    list: [\
      { id: "main", workspace: "~/.openclaw/workspace-main" },\
      { id: "coding", workspace: "~/.openclaw/workspace-coding" },\
    ],
  },
  bindings: [\
    { agentId: "main", match: { channel: "discord", accountId: "default" } },\
    { agentId: "coding", match: { channel: "discord", accountId: "coding" } },\
  ],
  channels: {
    discord: {
      groupPolicy: "allowlist",
      accounts: {
        default: {
          token: "DISCORD_BOT_TOKEN_MAIN",
          guilds: {
            "123456789012345678": {
              channels: {
                "222222222222222222": { allow: true, requireMention: false },
              },
            },
          },
        },
        coding: {
          token: "DISCORD_BOT_TOKEN_CODING",
          guilds: {
            "123456789012345678": {
              channels: {
                "333333333333333333": { allow: true, requireMention: false },
              },
            },
          },
        },
      },
    },
  },
}
```

- 邀请每个机器人加入 guild，并启用 Message Content Intent。
- Token 位于 `channels.discord.accounts.<id>.token`（默认账号可以使用 `DISCORD_BOT_TOKEN`）。

每个智能体一个 Telegram 机器人

```
{
  agents: {
    list: [\
      { id: "main", workspace: "~/.openclaw/workspace-main" },\
      { id: "alerts", workspace: "~/.openclaw/workspace-alerts" },\
    ],
  },
  bindings: [\
    { agentId: "main", match: { channel: "telegram", accountId: "default" } },\
    { agentId: "alerts", match: { channel: "telegram", accountId: "alerts" } },\
  ],
  channels: {
    telegram: {
      accounts: {
        default: {
          botToken: "123456:ABC...",
          dmPolicy: "pairing",
        },
        alerts: {
          botToken: "987654:XYZ...",
          dmPolicy: "allowlist",
          allowFrom: ["tg:123456789"],
        },
      },
    },
  },
}
```

- 使用 BotFather 为每个智能体创建一个机器人，并复制每个 token。
- Token 位于 `channels.telegram.accounts.<id>.botToken`（默认账号可以使用 `TELEGRAM_BOT_TOKEN`）。

每个智能体一个 WhatsApp 号码

在启动 Gateway 网关前关联每个账号：

```
openclaw channels login --channel whatsapp --account personal
openclaw channels login --channel whatsapp --account biz
```

`~/.openclaw/openclaw.json` (JSON5)：

```
{
  agents: {
    list: [\
      {\
        id: "home",\
        default: true,\
        name: "Home",\
        workspace: "~/.openclaw/workspace-home",\
        agentDir: "~/.openclaw/agents/home/agent",\
      },\
      {\
        id: "work",\
        name: "Work",\
        workspace: "~/.openclaw/workspace-work",\
        agentDir: "~/.openclaw/agents/work/agent",\
      },\
    ],
  },

  // Deterministic routing: first match wins (most-specific first).
  bindings: [\
    { agentId: "home", match: { channel: "whatsapp", accountId: "personal" } },\
    { agentId: "work", match: { channel: "whatsapp", accountId: "biz" } },\
\
    // Optional per-peer override (example: send a specific group to work agent).\
    {\
      agentId: "work",\
      match: {\
        channel: "whatsapp",\
        accountId: "personal",\
        peer: { kind: "group", id: "1203630...@g.us" },\
      },\
    },\
  ],

  // Off by default: agent-to-agent messaging must be explicitly enabled + allowlisted.
  tools: {
    agentToAgent: {
      enabled: false,
      allow: ["home", "work"],
    },
  },

  channels: {
    whatsapp: {
      accounts: {
        personal: {
          // Optional override. Default: ~/.openclaw/credentials/whatsapp/personal
          // authDir: "~/.openclaw/credentials/whatsapp/personal",
        },
        biz: {
          // Optional override. Default: ~/.openclaw/credentials/whatsapp/biz
          // authDir: "~/.openclaw/credentials/whatsapp/biz",
        },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/zh-CN/concepts/multi-agent\#%E5%B8%B8%E8%A7%81%E6%A8%A1%E5%BC%8F)  常见模式

- WhatsApp 日常 + Telegram 深度工作

- 同一渠道，将一个 peer 路由到 Opus

- 绑定到 WhatsApp 群组的家庭智能体


按渠道拆分：将 WhatsApp 路由到快速的日常智能体，将 Telegram 路由到 Opus 智能体。

```
{
  agents: {
    list: [\
      {\
        id: "chat",\
        name: "Everyday",\
        workspace: "~/.openclaw/workspace-chat",\
        model: "anthropic/claude-sonnet-4-6",\
      },\
      {\
        id: "opus",\
        name: "Deep Work",\
        workspace: "~/.openclaw/workspace-opus",\
        model: "anthropic/claude-opus-4-6",\
      },\
    ],
  },
  bindings: [\
    { agentId: "chat", match: { channel: "whatsapp" } },\
    { agentId: "opus", match: { channel: "telegram" } },\
  ],
}
```

注意：

- 如果某个渠道有多个账号，请在绑定中添加 `accountId`（例如 `{ channel: "whatsapp", accountId: "personal" }`）。
- 要将单个私信/群组路由到 Opus，同时让其余内容继续走 chat，请为该 peer 添加 `match.peer` 绑定；peer 匹配始终优先于渠道级规则。

让 WhatsApp 保持使用快速智能体，但将一个私信路由到 Opus：

```
{
  agents: {
    list: [\
      {\
        id: "chat",\
        name: "Everyday",\
        workspace: "~/.openclaw/workspace-chat",\
        model: "anthropic/claude-sonnet-4-6",\
      },\
      {\
        id: "opus",\
        name: "Deep Work",\
        workspace: "~/.openclaw/workspace-opus",\
        model: "anthropic/claude-opus-4-6",\
      },\
    ],
  },
  bindings: [\
    {\
      agentId: "opus",\
      match: { channel: "whatsapp", peer: { kind: "direct", id: "+15551234567" } },\
    },\
    { agentId: "chat", match: { channel: "whatsapp" } },\
  ],
}
```

Peer 绑定始终优先，因此请将它们放在渠道级规则上方。

将专用家庭智能体绑定到单个 WhatsApp 群组，并启用提及门控和更严格的工具策略：

```
{
  agents: {
    list: [\
      {\
        id: "family",\
        name: "Family",\
        workspace: "~/.openclaw/workspace-family",\
        identity: { name: "Family Bot" },\
        groupChat: {\
          mentionPatterns: ["@family", "@familybot", "@Family Bot"],\
        },\
        sandbox: {\
          mode: "all",\
          scope: "agent",\
        },\
        tools: {\
          allow: [\
            "exec",\
            "read",\
            "sessions_list",\
            "sessions_history",\
            "sessions_send",\
            "sessions_spawn",\
            "session_status",\
          ],\
          deny: ["write", "edit", "apply_patch", "browser", "canvas", "nodes", "cron"],\
        },\
      },\
    ],
  },
  bindings: [\
    {\
      agentId: "family",\
      match: {\
        channel: "whatsapp",\
        peer: { kind: "group", id: "120363999999999999@g.us" },\
      },\
    },\
  ],
}
```

注意：

- 工具允许/拒绝列表是 **工具**，不是 Skills。如果某个 skill 需要运行二进制文件，请确保允许 `exec`，并且该二进制文件存在于沙箱中。
- 如需更严格的门控，请设置 `agents.list[].groupChat.mentionPatterns`，并保持该渠道的群组允许列表启用。

## [​](https://docs.openclaw.ai/zh-CN/concepts/multi-agent\#%E6%AF%8F%E4%B8%AA%E6%99%BA%E8%83%BD%E4%BD%93%E7%9A%84%E6%B2%99%E7%AE%B1%E5%92%8C%E5%B7%A5%E5%85%B7%E9%85%8D%E7%BD%AE)  每个智能体的沙箱和工具配置

每个智能体都可以有自己的沙箱和工具限制：

```
{
  agents: {
    list: [\
      {\
        id: "personal",\
        workspace: "~/.openclaw/workspace-personal",\
        sandbox: {\
          mode: "off",  // No sandbox for personal agent\
        },\
        // No tool restrictions - all tools available\
      },\
      {\
        id: "family",\
        workspace: "~/.openclaw/workspace-family",\
        sandbox: {\
          mode: "all",     // Always sandboxed\
          scope: "agent",  // One container per agent\
          docker: {\
            // Optional one-time setup after container creation\
            setupCommand: "apt-get update && apt-get install -y git curl",\
          },\
        },\
        tools: {\
          allow: ["read"],                    // Only read tool\
          deny: ["exec", "write", "edit", "apply_patch"],    // Deny others\
        },\
      },\
    ],
  },
}
```

`setupCommand` 位于 `sandbox.docker` 下，并在容器创建时运行一次。当解析后的 scope 为 `"shared"` 时，每个智能体的 `sandbox.docker.*` 覆盖会被忽略。

**优势：**

- **安全隔离**：限制不受信任智能体可用的工具。
- **资源控制**：将特定智能体放入沙箱，同时让其他智能体保留在主机上。
- **灵活策略**：为每个智能体设置不同权限。

`tools.elevated` 是 **全局** 且基于发送者的；它不能按智能体配置。如果你需要每个智能体的边界，请使用 `agents.list[].tools` 来拒绝 `exec`。对于群组定向，请使用 `agents.list[].groupChat.mentionPatterns`，让 @提及能够清晰映射到目标智能体。

详见 [多智能体沙箱和工具](https://docs.openclaw.ai/zh-CN/tools/multi-agent-sandbox-tools) 中的详细示例。

## [​](https://docs.openclaw.ai/zh-CN/concepts/multi-agent\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

- [ACP 智能体](https://docs.openclaw.ai/zh-CN/tools/acp-agents) — 运行外部编码 harness
- [渠道路由](https://docs.openclaw.ai/zh-CN/channels/channel-routing) — 消息如何路由到智能体
- [Presence](https://docs.openclaw.ai/zh-CN/concepts/presence) — 智能体在线状态和可用性
- [会话](https://docs.openclaw.ai/zh-CN/concepts/session) — 会话隔离和路由
- [子智能体](https://docs.openclaw.ai/zh-CN/tools/subagents) — 生成后台智能体运行

[压缩](https://docs.openclaw.ai/zh-CN/concepts/compaction) [在线状态](https://docs.openclaw.ai/zh-CN/concepts/presence)

Ctrl+I