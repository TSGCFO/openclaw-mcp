---
source_url: https://docs.openclaw.ai/zh-CN/channels/irc
title: "IRC - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/zh-CN/channels/irc#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

IRC

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [快速开始](https://docs.openclaw.ai/zh-CN/channels/irc#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)
- [默认安全设置](https://docs.openclaw.ai/zh-CN/channels/irc#%E9%BB%98%E8%AE%A4%E5%AE%89%E5%85%A8%E8%AE%BE%E7%BD%AE)
- [访问控制](https://docs.openclaw.ai/zh-CN/channels/irc#%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6)
- [常见陷阱：allowFrom 用于私信，不用于渠道](https://docs.openclaw.ai/zh-CN/channels/irc#%E5%B8%B8%E8%A7%81%E9%99%B7%E9%98%B1%EF%BC%9Aallowfrom-%E7%94%A8%E4%BA%8E%E7%A7%81%E4%BF%A1%EF%BC%8C%E4%B8%8D%E7%94%A8%E4%BA%8E%E6%B8%A0%E9%81%93)
- [回复触发（提及）](https://docs.openclaw.ai/zh-CN/channels/irc#%E5%9B%9E%E5%A4%8D%E8%A7%A6%E5%8F%91%EF%BC%88%E6%8F%90%E5%8F%8A%EF%BC%89)
- [安全说明（推荐用于公共渠道）](https://docs.openclaw.ai/zh-CN/channels/irc#%E5%AE%89%E5%85%A8%E8%AF%B4%E6%98%8E%EF%BC%88%E6%8E%A8%E8%8D%90%E7%94%A8%E4%BA%8E%E5%85%AC%E5%85%B1%E6%B8%A0%E9%81%93%EF%BC%89)
- [渠道内所有人使用相同的工具权限](https://docs.openclaw.ai/zh-CN/channels/irc#%E6%B8%A0%E9%81%93%E5%86%85%E6%89%80%E6%9C%89%E4%BA%BA%E4%BD%BF%E7%94%A8%E7%9B%B8%E5%90%8C%E7%9A%84%E5%B7%A5%E5%85%B7%E6%9D%83%E9%99%90)
- [按发送者区分工具权限（所有者权限更大）](https://docs.openclaw.ai/zh-CN/channels/irc#%E6%8C%89%E5%8F%91%E9%80%81%E8%80%85%E5%8C%BA%E5%88%86%E5%B7%A5%E5%85%B7%E6%9D%83%E9%99%90%EF%BC%88%E6%89%80%E6%9C%89%E8%80%85%E6%9D%83%E9%99%90%E6%9B%B4%E5%A4%A7%EF%BC%89)
- [NickServ](https://docs.openclaw.ai/zh-CN/channels/irc#nickserv)
- [环境变量](https://docs.openclaw.ai/zh-CN/channels/irc#%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
- [故障排除](https://docs.openclaw.ai/zh-CN/channels/irc#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4)
- [相关内容](https://docs.openclaw.ai/zh-CN/channels/irc#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

当你希望 OpenClaw 进入经典 IRC 渠道（`#room`）和私信时，请使用 IRC。
IRC 作为内置插件提供，但需要在主配置的 `channels.irc` 下进行配置。

## [​](https://docs.openclaw.ai/zh-CN/channels/irc\#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)  快速开始

1. 在 `~/.openclaw/openclaw.json` 中启用 IRC 配置。
2. 至少设置以下内容：

```
{
  channels: {
    irc: {
      enabled: true,
      host: "irc.example.com",
      port: 6697,
      tls: true,
      nick: "openclaw-bot",
      channels: ["#openclaw"],
    },
  },
}
```

建议使用私有 IRC 服务器进行机器人协作。如果你有意使用公共 IRC 网络，常见选择包括 Libera.Chat、OFTC 和 Snoonet。避免将可预测的公共频道用于机器人或 swarm 的回传流量。

3. 启动/重启 Gateway 网关：

```
openclaw gateway run
```

## [​](https://docs.openclaw.ai/zh-CN/channels/irc\#%E9%BB%98%E8%AE%A4%E5%AE%89%E5%85%A8%E8%AE%BE%E7%BD%AE)  默认安全设置

- `channels.irc.dmPolicy` 默认为 `"pairing"`。
- `channels.irc.groupPolicy` 默认为 `"allowlist"`。
- 当 `groupPolicy="allowlist"` 时，设置 `channels.irc.groups` 以定义允许的渠道。
- 除非你有意接受明文传输，否则请使用 TLS（`channels.irc.tls=true`）。

## [​](https://docs.openclaw.ai/zh-CN/channels/irc\#%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6)  访问控制

IRC 渠道有两个独立的“门”：

1. **渠道访问**（`groupPolicy` \+ `groups`）：机器人是否完全接受来自某个渠道的消息。
2. **发送者访问**（`groupAllowFrom` / 每渠道 `groups["#channel"].allowFrom`）：谁有权在该渠道内触发机器人。

配置键：

- 私信允许列表（私信发送者访问）：`channels.irc.allowFrom`
- 群组发送者允许列表（渠道发送者访问）：`channels.irc.groupAllowFrom`
- 每渠道控制（渠道 \+ 发送者 \+ 提及规则）：`channels.irc.groups["#channel"]`
- `channels.irc.groupPolicy="open"` 允许未配置的渠道（ **默认仍然受提及门控限制**）

允许列表项应使用稳定的发送者身份（`nick!user@host`）。
仅使用裸 `nick` 匹配是可变的，并且只有在 `channels.irc.dangerouslyAllowNameMatching: true` 时才会启用。

### [​](https://docs.openclaw.ai/zh-CN/channels/irc\#%E5%B8%B8%E8%A7%81%E9%99%B7%E9%98%B1%EF%BC%9Aallowfrom-%E7%94%A8%E4%BA%8E%E7%A7%81%E4%BF%A1%EF%BC%8C%E4%B8%8D%E7%94%A8%E4%BA%8E%E6%B8%A0%E9%81%93)  常见陷阱：`allowFrom` 用于私信，不用于渠道

如果你看到这样的日志：

- `irc: drop group sender alice!ident@host (policy=allowlist)`

……这意味着该发送者未被允许发送 **群组/渠道** 消息。你可以通过以下方式修复：

- 设置 `channels.irc.groupAllowFrom`（对所有渠道全局生效），或
- 设置每渠道发送者允许列表：`channels.irc.groups["#channel"].allowFrom`

示例（允许 `#tuirc-dev` 中的任何人与机器人对话）：

```
{
  channels: {
    irc: {
      groupPolicy: "allowlist",
      groups: {
        "#tuirc-dev": { allowFrom: ["*"] },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/zh-CN/channels/irc\#%E5%9B%9E%E5%A4%8D%E8%A7%A6%E5%8F%91%EF%BC%88%E6%8F%90%E5%8F%8A%EF%BC%89)  回复触发（提及）

即使某个渠道已被允许（通过 `groupPolicy` \+ `groups`），并且发送者也被允许，OpenClaw 在群组场景下默认仍启用 **提及门控**。这意味着，除非消息中包含与机器人匹配的提及模式，否则你可能会看到类似 `drop channel … (missing-mention)` 的日志。如果你希望机器人在 IRC 渠道中 **无需提及即可回复**，请为该渠道禁用提及门控：

```
{
  channels: {
    irc: {
      groupPolicy: "allowlist",
      groups: {
        "#tuirc-dev": {
          requireMention: false,
          allowFrom: ["*"],
        },
      },
    },
  },
}
```

或者，如果你希望允许 **所有** IRC 渠道（不使用每渠道允许列表），并且仍然无需提及即可回复：

```
{
  channels: {
    irc: {
      groupPolicy: "open",
      groups: {
        "*": { requireMention: false, allowFrom: ["*"] },
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/zh-CN/channels/irc\#%E5%AE%89%E5%85%A8%E8%AF%B4%E6%98%8E%EF%BC%88%E6%8E%A8%E8%8D%90%E7%94%A8%E4%BA%8E%E5%85%AC%E5%85%B1%E6%B8%A0%E9%81%93%EF%BC%89)  安全说明（推荐用于公共渠道）

如果你在公共渠道中设置 `allowFrom: ["*"]`，任何人都可以提示机器人。
为降低风险，建议限制该渠道可用的工具。

### [​](https://docs.openclaw.ai/zh-CN/channels/irc\#%E6%B8%A0%E9%81%93%E5%86%85%E6%89%80%E6%9C%89%E4%BA%BA%E4%BD%BF%E7%94%A8%E7%9B%B8%E5%90%8C%E7%9A%84%E5%B7%A5%E5%85%B7%E6%9D%83%E9%99%90)  渠道内所有人使用相同的工具权限

```
{
  channels: {
    irc: {
      groups: {
        "#tuirc-dev": {
          allowFrom: ["*"],
          tools: {
            deny: ["group:runtime", "group:fs", "gateway", "nodes", "cron", "browser"],
          },
        },
      },
    },
  },
}
```

### [​](https://docs.openclaw.ai/zh-CN/channels/irc\#%E6%8C%89%E5%8F%91%E9%80%81%E8%80%85%E5%8C%BA%E5%88%86%E5%B7%A5%E5%85%B7%E6%9D%83%E9%99%90%EF%BC%88%E6%89%80%E6%9C%89%E8%80%85%E6%9D%83%E9%99%90%E6%9B%B4%E5%A4%A7%EF%BC%89)  按发送者区分工具权限（所有者权限更大）

使用 `toolsBySender`，对 `"*"` 应用更严格的策略，对你的 nick 应用更宽松的策略：

```
{
  channels: {
    irc: {
      groups: {
        "#tuirc-dev": {
          allowFrom: ["*"],
          toolsBySender: {
            "*": {
              deny: ["group:runtime", "group:fs", "gateway", "nodes", "cron", "browser"],
            },
            "id:eigen": {
              deny: ["gateway", "nodes", "cron"],
            },
          },
        },
      },
    },
  },
}
```

说明：

- `toolsBySender` 键应对 IRC 发送者身份值使用 `id:`：
使用 `id:eigen`，或使用 `id:eigen!~eigen@174.127.248.171` 进行更强匹配。
- 旧版无前缀键仍然受支持，但只会按 `id:` 进行匹配。
- 首个匹配到的发送者策略优先生效；`"*"` 是通配回退。

有关群组访问与提及门控（以及它们如何交互）的更多信息，请参阅： [/channels/groups](https://docs.openclaw.ai/zh-CN/channels/groups)。

## [​](https://docs.openclaw.ai/zh-CN/channels/irc\#nickserv)  NickServ

连接后若要通过 NickServ 进行身份验证：

```
{
  channels: {
    irc: {
      nickserv: {
        enabled: true,
        service: "NickServ",
        password: "your-nickserv-password",
      },
    },
  },
}
```

连接时可选执行一次性注册：

```
{
  channels: {
    irc: {
      nickserv: {
        register: true,
        registerEmail: "bot@example.com",
      },
    },
  },
}
```

在 nick 完成注册后禁用 `register`，以避免重复尝试执行 REGISTER。

## [​](https://docs.openclaw.ai/zh-CN/channels/irc\#%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)  环境变量

默认账户支持：

- `IRC_HOST`
- `IRC_PORT`
- `IRC_TLS`
- `IRC_NICK`
- `IRC_USERNAME`
- `IRC_REALNAME`
- `IRC_PASSWORD`
- `IRC_CHANNELS`（逗号分隔）
- `IRC_NICKSERV_PASSWORD`
- `IRC_NICKSERV_REGISTER_EMAIL`

`IRC_HOST` 不能通过工作区 `.env` 设置；请参阅 [工作区 `.env` 文件](https://docs.openclaw.ai/zh-CN/gateway/security)。

## [​](https://docs.openclaw.ai/zh-CN/channels/irc\#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4)  故障排除

- 如果机器人已连接但从不在渠道中回复，请检查 `channels.irc.groups`， **以及** 是否因为提及门控而丢弃了消息（`missing-mention`）。如果你希望它无需 ping 就能回复，请为该渠道设置 `requireMention:false`。
- 如果登录失败，请检查 nick 是否可用以及服务器密码是否正确。
- 如果在自定义网络上 TLS 失败，请检查 host/port 和证书配置。

## [​](https://docs.openclaw.ai/zh-CN/channels/irc\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

- [渠道概览](https://docs.openclaw.ai/zh-CN/channels)——所有支持的渠道
- [配对](https://docs.openclaw.ai/zh-CN/channels/pairing)——私信认证与配对流程
- [群组](https://docs.openclaw.ai/zh-CN/channels/groups)——群聊行为与提及门控
- [渠道路由](https://docs.openclaw.ai/zh-CN/channels/channel-routing)——消息的会话路由
- [安全](https://docs.openclaw.ai/zh-CN/gateway/security)——访问模型与加固措施

Ctrl+I