---
source_url: https://docs.openclaw.ai/zh-CN/channels/wechat
title: "\u5fae\u4fe1 - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/zh-CN/channels/wechat#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

微信

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [命名](https://docs.openclaw.ai/zh-CN/channels/wechat#%E5%91%BD%E5%90%8D)
- [工作原理](https://docs.openclaw.ai/zh-CN/channels/wechat#%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86)
- [安装](https://docs.openclaw.ai/zh-CN/channels/wechat#%E5%AE%89%E8%A3%85)
- [登录](https://docs.openclaw.ai/zh-CN/channels/wechat#%E7%99%BB%E5%BD%95)
- [访问控制](https://docs.openclaw.ai/zh-CN/channels/wechat#%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6)
- [兼容性](https://docs.openclaw.ai/zh-CN/channels/wechat#%E5%85%BC%E5%AE%B9%E6%80%A7)
- [Sidecar 进程](https://docs.openclaw.ai/zh-CN/channels/wechat#sidecar-%E8%BF%9B%E7%A8%8B)
- [故障排除](https://docs.openclaw.ai/zh-CN/channels/wechat#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4)
- [相关文档](https://docs.openclaw.ai/zh-CN/channels/wechat#%E7%9B%B8%E5%85%B3%E6%96%87%E6%A1%A3)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw 通过腾讯提供的外部 `@tencent-weixin/openclaw-weixin` 渠道插件连接到微信。状态：外部插件。支持私聊和媒体。当前插件能力元数据未声明支持群聊。

## [​](https://docs.openclaw.ai/zh-CN/channels/wechat\#%E5%91%BD%E5%90%8D)  命名

- **微信** 是这些文档中的面向用户名称。
- **Weixin** 是腾讯包名和插件 id 中使用的名称。
- `openclaw-weixin` 是 OpenClaw 渠道 id。
- `@tencent-weixin/openclaw-weixin` 是 npm 包。

在 CLI 命令和配置路径中使用 `openclaw-weixin`。

## [​](https://docs.openclaw.ai/zh-CN/channels/wechat\#%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86)  工作原理

微信代码并不位于 OpenClaw 核心仓库中。OpenClaw 提供通用的渠道插件契约，而外部插件提供微信专用运行时：

1. `openclaw plugins install` 安装 `@tencent-weixin/openclaw-weixin`。
2. Gateway 网关发现插件清单并加载插件入口点。
3. 插件注册渠道 id `openclaw-weixin`。
4. `openclaw channels login --channel openclaw-weixin` 启动二维码登录。
5. 插件将账户凭证存储在 OpenClaw 状态目录下。
6. 当 Gateway 网关启动时，插件会为每个已配置账户启动其 Weixin 监控器。
7. 入站微信消息会通过渠道契约进行规范化，路由到选定的 OpenClaw 智能体，并通过插件的出站路径发回。

这种分离很重要：OpenClaw 核心应保持渠道无关。微信登录、腾讯 iLink API 调用、媒体上传/下载、上下文令牌和账户监控都由外部插件负责。

## [​](https://docs.openclaw.ai/zh-CN/channels/wechat\#%E5%AE%89%E8%A3%85)  安装

快速安装：

```
npx -y @tencent-weixin/openclaw-weixin-cli install
```

手动安装：

```
openclaw plugins install "@tencent-weixin/openclaw-weixin"
openclaw config set plugins.entries.openclaw-weixin.enabled true
```

安装后重启 Gateway 网关：

```
openclaw gateway restart
```

## [​](https://docs.openclaw.ai/zh-CN/channels/wechat\#%E7%99%BB%E5%BD%95)  登录

在运行 Gateway 网关的同一台机器上执行二维码登录：

```
openclaw channels login --channel openclaw-weixin
```

使用手机上的微信扫描二维码并确认登录。扫码成功后，插件会将账户令牌保存在本地。要添加另一个微信账户，请再次运行相同的登录命令。对于多账户，请按账户、渠道和发送方隔离私信会话：

```
openclaw config set session.dmScope per-account-channel-peer
```

## [​](https://docs.openclaw.ai/zh-CN/channels/wechat\#%E8%AE%BF%E9%97%AE%E6%8E%A7%E5%88%B6)  访问控制

私信使用渠道插件的常规 OpenClaw 配对和 allowlist 模型。批准新发送方：

```
openclaw pairing list openclaw-weixin
openclaw pairing approve openclaw-weixin <CODE>
```

完整访问控制模型请参阅 [配对](https://docs.openclaw.ai/zh-CN/channels/pairing)。

## [​](https://docs.openclaw.ai/zh-CN/channels/wechat\#%E5%85%BC%E5%AE%B9%E6%80%A7)  兼容性

插件会在启动时检查宿主 OpenClaw 版本。

| 插件版本线 | OpenClaw 版本 | npm tag |
| --- | --- | --- |
| `2.x` | `>=2026.3.22` | `latest` |
| `1.x` | `>=2026.1.0 <2026.3.22` | `legacy` |

如果插件报告你的 OpenClaw 版本过旧，请升级 OpenClaw，或安装 legacy 插件版本线：

```
openclaw plugins install @tencent-weixin/openclaw-weixin@legacy
```

## [​](https://docs.openclaw.ai/zh-CN/channels/wechat\#sidecar-%E8%BF%9B%E7%A8%8B)  Sidecar 进程

微信插件在监控腾讯 iLink API 时，可以在 Gateway 网关旁运行辅助工作。在 issue #68451 中，这条辅助路径暴露了 OpenClaw 通用陈旧 Gateway 网关清理逻辑中的一个 bug：子进程可能会尝试清理父 Gateway 网关进程，从而在 systemd 等进程管理器下导致重启循环。当前 OpenClaw 启动清理逻辑会排除当前进程及其祖先进程，因此渠道辅助进程不得终止启动它的 Gateway 网关。这个修复是通用的；它不是核心中的微信专用路径。

## [​](https://docs.openclaw.ai/zh-CN/channels/wechat\#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4)  故障排除

检查安装和状态：

```
openclaw plugins list
openclaw channels status --probe
openclaw --version
```

如果渠道显示已安装但未连接，请确认插件已启用并重启：

```
openclaw config set plugins.entries.openclaw-weixin.enabled true
openclaw gateway restart
```

如果启用微信后 Gateway 网关反复重启，请同时更新 OpenClaw 和插件：

```
npm view @tencent-weixin/openclaw-weixin version
openclaw plugins install "@tencent-weixin/openclaw-weixin" --force
openclaw gateway restart
```

临时禁用：

```
openclaw config set plugins.entries.openclaw-weixin.enabled false
openclaw gateway restart
```

## [​](https://docs.openclaw.ai/zh-CN/channels/wechat\#%E7%9B%B8%E5%85%B3%E6%96%87%E6%A1%A3)  相关文档

- 渠道概览： [聊天渠道](https://docs.openclaw.ai/zh-CN/channels)
- 配对： [配对](https://docs.openclaw.ai/zh-CN/channels/pairing)
- 渠道路由： [渠道路由](https://docs.openclaw.ai/zh-CN/channels/channel-routing)
- 插件架构： [插件架构](https://docs.openclaw.ai/zh-CN/plugins/architecture)
- 渠道插件 SDK： [渠道插件 SDK](https://docs.openclaw.ai/zh-CN/plugins/sdk-channel-plugins)
- 外部包： [@tencent-weixin/openclaw-weixin](https://www.npmjs.com/package/@tencent-weixin/openclaw-weixin)

Ctrl+I