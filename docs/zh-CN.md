---
source_url: https://docs.openclaw.ai/zh-CN
title: "OpenClaw - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

首页

OpenClaw

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [OpenClaw 🦞](https://docs.openclaw.ai/zh-CN#openclaw-)
- [OpenClaw 是什么？](https://docs.openclaw.ai/zh-CN#openclaw-%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F)
- [工作原理](https://docs.openclaw.ai/zh-CN#%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86)
- [关键能力](https://docs.openclaw.ai/zh-CN#%E5%85%B3%E9%94%AE%E8%83%BD%E5%8A%9B)
- [快速开始](https://docs.openclaw.ai/zh-CN#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)
- [仪表板](https://docs.openclaw.ai/zh-CN#%E4%BB%AA%E8%A1%A8%E6%9D%BF)
- [配置（可选）](https://docs.openclaw.ai/zh-CN#%E9%85%8D%E7%BD%AE%EF%BC%88%E5%8F%AF%E9%80%89%EF%BC%89)
- [从这里开始](https://docs.openclaw.ai/zh-CN#%E4%BB%8E%E8%BF%99%E9%87%8C%E5%BC%80%E5%A7%8B)
- [了解更多](https://docs.openclaw.ai/zh-CN#%E4%BA%86%E8%A7%A3%E6%9B%B4%E5%A4%9A)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/zh-CN\#openclaw-)  OpenClaw 🦞

![OpenClaw](https://mintcdn.com/clawdhub/-t5HSeZ3Y_0_wH4i/assets/openclaw-logo-text-dark.png?fit=max&auto=format&n=-t5HSeZ3Y_0_wH4i&q=85&s=61797dcb0c37d6e9279b8c5ad2e850e4)![OpenClaw](https://mintcdn.com/clawdhub/FaXdIfo7gPK_jSWb/assets/openclaw-logo-text.png?fit=max&auto=format&n=FaXdIfo7gPK_jSWb&q=85&s=d799bea41acb92d4c9fd1075c575879f)

> _“蜕皮！蜕皮！”_ — 大概是一只太空龙虾

**适用于 AI 智能体的任意操作系统 Gateway 网关，覆盖 Discord、Google Chat、iMessage、Matrix、Microsoft Teams、Signal、Slack、Telegram、WhatsApp、Zalo 等渠道。**

发一条消息，就能随时随地收到智能体回复。通过一个 Gateway 网关即可运行内置渠道、内置渠道插件、WebChat 和移动节点。

[**入门指南** \\
\\
安装 OpenClaw，几分钟内启动 Gateway 网关。](https://docs.openclaw.ai/zh-CN/start/getting-started)

[**运行新手引导** \\
\\
使用 `openclaw onboard` 和配对流程进行引导式设置。](https://docs.openclaw.ai/zh-CN/start/wizard)

[**打开控制 UI** \\
\\
启动浏览器仪表板，用于聊天、配置和会话。](https://docs.openclaw.ai/web/control-ui)

## [​](https://docs.openclaw.ai/zh-CN\#openclaw-%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F)  OpenClaw 是什么？

OpenClaw 是一个 **自托管 Gateway 网关**，可将你常用的聊天应用和渠道界面——包括内置渠道，以及 Discord、Google Chat、iMessage、Matrix、Microsoft Teams、Signal、Slack、Telegram、WhatsApp、Zalo 等内置或外部渠道插件——连接到像 Pi 这样的 AI 编码智能体。你只需在自己的机器上（或服务器上）运行一个 Gateway 网关进程，它就会成为你的消息应用与一个始终在线的 AI 助手之间的桥梁。**它适合谁？** 适合开发者和高级用户，他们希望拥有一个可随时随地发送消息的个人 AI 助手——同时不放弃对自己数据的控制，也不依赖托管服务。**它有什么不同？**

- **自托管**：运行在你的硬件上，按你的规则工作
- **多渠道**：一个 Gateway 网关可同时服务内置渠道以及内置或外部渠道插件
- **智能体原生**：专为编码智能体打造，支持工具使用、会话、记忆和多智能体路由
- **开源**：采用 MIT 许可证，由社区驱动

**你需要什么？** Node 24（推荐），或兼容用的 Node 22 LTS（`22.14+`），你所选提供商的 API 密钥，以及 5 分钟时间。为了获得最佳质量与安全性，请使用当前可用的最新一代最强模型。

## [​](https://docs.openclaw.ai/zh-CN\#%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86)  工作原理

聊天应用 \+ 插件

Gateway 网关

Pi 智能体

CLI

Web 控制 UI

macOS 应用

iOS 和 Android 节点

Gateway 网关是会话、路由和渠道连接的唯一事实来源。

## [​](https://docs.openclaw.ai/zh-CN\#%E5%85%B3%E9%94%AE%E8%83%BD%E5%8A%9B)  关键能力

[**多渠道 Gateway 网关** \\
\\
通过单个 Gateway 网关进程接入 Discord、iMessage、Signal、Slack、Telegram、WhatsApp、WebChat 等更多渠道。](https://docs.openclaw.ai/zh-CN/channels)

[**插件渠道** \\
\\
内置插件可在当前正式版本中添加 Matrix、Nostr、Twitch、Zalo 等更多渠道。](https://docs.openclaw.ai/zh-CN/tools/plugin)

[**多智能体路由** \\
\\
针对每个智能体、工作区或发送者提供隔离的会话。](https://docs.openclaw.ai/zh-CN/concepts/multi-agent)

[**媒体支持** \\
\\
发送和接收图片、音频与文档。](https://docs.openclaw.ai/zh-CN/nodes/images)

[**Web 控制 UI** \\
\\
用于聊天、配置、会话和节点的浏览器仪表板。](https://docs.openclaw.ai/web/control-ui)

[**移动节点** \\
\\
配对 iOS 和 Android 节点，用于 Canvas、相机和支持语音的工作流。](https://docs.openclaw.ai/zh-CN/nodes)

## [​](https://docs.openclaw.ai/zh-CN\#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)  快速开始

1

[Navigate to header](https://docs.openclaw.ai/zh-CN#)

安装 OpenClaw

```
npm install -g openclaw@latest
```

2

[Navigate to header](https://docs.openclaw.ai/zh-CN#)

执行新手引导并安装服务

```
openclaw onboard --install-daemon
```

3

[Navigate to header](https://docs.openclaw.ai/zh-CN#)

开始聊天

在浏览器中打开控制 UI 并发送一条消息：

```
openclaw dashboard
```

或连接一个渠道（ [Telegram](https://docs.openclaw.ai/zh-CN/channels/telegram) 最快），然后直接用手机聊天。

需要完整的安装和开发设置？请参阅 [入门指南](https://docs.openclaw.ai/zh-CN/start/getting-started)。

## [​](https://docs.openclaw.ai/zh-CN\#%E4%BB%AA%E8%A1%A8%E6%9D%BF)  仪表板

Gateway 网关启动后，在浏览器中打开控制 UI。

- 本地默认地址： [http://127.0.0.1:18789/](http://127.0.0.1:18789/)
- 远程访问： [Web 界面](https://docs.openclaw.ai/web) 和 [Tailscale](https://docs.openclaw.ai/zh-CN/gateway/tailscale)

![OpenClaw](https://mintcdn.com/clawdhub/FaXdIfo7gPK_jSWb/whatsapp-openclaw.jpg?fit=max&auto=format&n=FaXdIfo7gPK_jSWb&q=85&s=b74a3630b0e971f466eff15fbdc642cb)

## [​](https://docs.openclaw.ai/zh-CN\#%E9%85%8D%E7%BD%AE%EF%BC%88%E5%8F%AF%E9%80%89%EF%BC%89)  配置（可选）

配置文件位于 `~/.openclaw/openclaw.json`。

- 如果你 **什么都不做**，OpenClaw 会使用内置的 Pi 二进制并以 RPC 模式运行，同时为每个发送者创建独立会话。
- 如果你想进一步收紧权限，建议从 `channels.whatsapp.allowFrom` 和（针对群组的）提及规则开始。

示例：

```
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

## [​](https://docs.openclaw.ai/zh-CN\#%E4%BB%8E%E8%BF%99%E9%87%8C%E5%BC%80%E5%A7%8B)  从这里开始

[**文档中心** \\
\\
按使用场景组织的所有文档和指南。](https://docs.openclaw.ai/zh-CN/start/hubs)

[**配置** \\
\\
核心 Gateway 网关设置、令牌和提供商配置。](https://docs.openclaw.ai/zh-CN/gateway/configuration)

[**远程访问** \\
\\
SSH 和 tailnet 访问模式。](https://docs.openclaw.ai/zh-CN/gateway/remote)

[**渠道** \\
\\
针对 Feishu、Microsoft Teams、WhatsApp、Telegram、Discord 等渠道的专属设置说明。](https://docs.openclaw.ai/zh-CN/channels/telegram)

[**节点** \\
\\
iOS 和 Android 节点，支持配对、Canvas、相机和设备操作。](https://docs.openclaw.ai/zh-CN/nodes)

[**帮助** \\
\\
常见修复方法和故障排除入口。](https://docs.openclaw.ai/zh-CN/help)

## [​](https://docs.openclaw.ai/zh-CN\#%E4%BA%86%E8%A7%A3%E6%9B%B4%E5%A4%9A)  了解更多

[**完整功能列表** \\
\\
完整的渠道、路由和媒体能力说明。](https://docs.openclaw.ai/zh-CN/concepts/features)

[**多智能体路由** \\
\\
工作区隔离和按智能体划分的会话。](https://docs.openclaw.ai/zh-CN/concepts/multi-agent)

[**安全** \\
\\
令牌、允许列表和安全控制。](https://docs.openclaw.ai/zh-CN/gateway/security)

[**故障排除** \\
\\
Gateway 网关诊断和常见错误。](https://docs.openclaw.ai/zh-CN/gateway/troubleshooting)

[**关于与致谢** \\
\\
项目起源、贡献者和许可证。](https://docs.openclaw.ai/zh-CN/reference/credits)

[展示专区](https://docs.openclaw.ai/zh-CN/start/showcase)

Ctrl+I

![OpenClaw](https://mintcdn.com/clawdhub/-t5HSeZ3Y_0_wH4i/assets/openclaw-logo-text-dark.png?w=1100&fit=max&auto=format&n=-t5HSeZ3Y_0_wH4i&q=85&s=ed926636a9752c9ce39acccf51c3b271)

![OpenClaw](https://mintcdn.com/clawdhub/FaXdIfo7gPK_jSWb/assets/openclaw-logo-text.png?w=1100&fit=max&auto=format&n=FaXdIfo7gPK_jSWb&q=85&s=88255cdd2554a6b341c89ae709743441)

![OpenClaw](https://mintcdn.com/clawdhub/FaXdIfo7gPK_jSWb/whatsapp-openclaw.jpg?w=1100&fit=max&auto=format&n=FaXdIfo7gPK_jSWb&q=85&s=72f5064ba581433011975bde37c74964)