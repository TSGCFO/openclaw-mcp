---
source_url: https://docs.openclaw.ai/zh-CN/start/getting-started
title: "\u5165\u95e8\u6307\u5357 - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/start/getting-started#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

第一步

入门指南

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [你需要准备什么](https://docs.openclaw.ai/zh-CN/start/getting-started#%E4%BD%A0%E9%9C%80%E8%A6%81%E5%87%86%E5%A4%87%E4%BB%80%E4%B9%88)
- [快速设置](https://docs.openclaw.ai/zh-CN/start/getting-started#%E5%BF%AB%E9%80%9F%E8%AE%BE%E7%BD%AE)
- [接下来做什么](https://docs.openclaw.ai/zh-CN/start/getting-started#%E6%8E%A5%E4%B8%8B%E6%9D%A5%E5%81%9A%E4%BB%80%E4%B9%88)
- [相关内容](https://docs.openclaw.ai/zh-CN/start/getting-started#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

安装 OpenClaw，运行新手引导，并与你的 AI 助手聊天 —— 整个过程大约只需
5 分钟。完成后，你将拥有一个正在运行的 Gateway 网关、已配置好的认证，
以及一个可用的聊天会话。

## [​](https://docs.openclaw.ai/zh-CN/start/getting-started\#%E4%BD%A0%E9%9C%80%E8%A6%81%E5%87%86%E5%A4%87%E4%BB%80%E4%B9%88)  你需要准备什么

- **Node.js** —— 推荐 Node 24（也支持 Node 22.14+）
- **模型提供商的 API key**（Anthropic、OpenAI、Google 等）—— 新手引导会提示你输入

使用 `node --version` 检查你的 Node 版本。
**Windows 用户：** 原生 Windows 和 WSL2 都受支持。WSL2 更稳定，且更推荐用于完整体验。请参阅 [Windows](https://docs.openclaw.ai/zh-CN/platforms/windows)。
需要安装 Node？请参阅 [Node 设置](https://docs.openclaw.ai/zh-CN/install/node)。

## [​](https://docs.openclaw.ai/zh-CN/start/getting-started\#%E5%BF%AB%E9%80%9F%E8%AE%BE%E7%BD%AE)  快速设置

1

[Navigate to header](https://docs.openclaw.ai/zh-CN/start/getting-started#)

安装 OpenClaw

- macOS / Linux

- Windows（PowerShell）


```
curl -fsSL https://openclaw.ai/install.sh | bash
```

![Install Script Process](https://mintcdn.com/clawdhub/U8jr7qEbUc9OU9YR/assets/install-script.svg?fit=max&auto=format&n=U8jr7qEbUc9OU9YR&q=85&s=50706f81e3210a610262f14facb11f65)

```
iwr -useb https://openclaw.ai/install.ps1 | iex
```

其他安装方式（Docker、Nix、npm）： [安装](https://docs.openclaw.ai/zh-CN/install)。

2

[Navigate to header](https://docs.openclaw.ai/zh-CN/start/getting-started#)

运行新手引导

```
openclaw onboard --install-daemon
```

向导会引导你选择模型提供商、设置 API key，
并配置 Gateway 网关。整个过程大约需要 2 分钟。完整参考请参阅 [新手引导（CLI）](https://docs.openclaw.ai/zh-CN/start/wizard)。

3

[Navigate to header](https://docs.openclaw.ai/zh-CN/start/getting-started#)

验证 Gateway 网关正在运行

```
openclaw gateway status
```

你应该会看到 Gateway 网关正在监听端口 18789。

4

[Navigate to header](https://docs.openclaw.ai/zh-CN/start/getting-started#)

打开仪表板

```
openclaw dashboard
```

这会在你的浏览器中打开控制 UI。如果能够加载，说明一切正常。

5

[Navigate to header](https://docs.openclaw.ai/zh-CN/start/getting-started#)

发送你的第一条消息

在控制 UI 聊天中输入一条消息，你应该会收到 AI 回复。想改用手机聊天？最快可设置的渠道是
[Telegram](https://docs.openclaw.ai/zh-CN/channels/telegram)（只需要一个 bot token）。所有选项请参阅 [渠道](https://docs.openclaw.ai/zh-CN/channels)。

高级：挂载自定义控制 UI 构建

如果你维护的是本地化或自定义的仪表板构建，请将
`gateway.controlUi.root` 指向一个包含已构建静态资源和 `index.html` 的目录。

```
mkdir -p "$HOME/.openclaw/control-ui-custom"
# 将你构建好的静态文件复制到该目录中。
```

然后设置：

```
{
  "gateway": {
    "controlUi": {
      "enabled": true,
      "root": "$HOME/.openclaw/control-ui-custom"
    }
  }
}
```

重启 Gateway 网关并重新打开仪表板：

```
openclaw gateway restart
openclaw dashboard
```

## [​](https://docs.openclaw.ai/zh-CN/start/getting-started\#%E6%8E%A5%E4%B8%8B%E6%9D%A5%E5%81%9A%E4%BB%80%E4%B9%88)  接下来做什么

[**连接一个渠道** \\
\\
Discord、Feishu、iMessage、Matrix、Microsoft Teams、Signal、Slack、Telegram、WhatsApp、Zalo 等等。](https://docs.openclaw.ai/zh-CN/channels)

[**配对与安全** \\
\\
控制谁可以向你的智能体发消息。](https://docs.openclaw.ai/zh-CN/channels/pairing)

[**配置 Gateway 网关** \\
\\
模型、工具、沙箱和高级设置。](https://docs.openclaw.ai/zh-CN/gateway/configuration)

[**浏览工具** \\
\\
浏览器、exec、网页搜索、Skills 和插件。](https://docs.openclaw.ai/zh-CN/tools)

高级：环境变量

如果你将 OpenClaw 作为服务账户运行，或希望使用自定义路径：

- `OPENCLAW_HOME` —— 用于内部路径解析的主目录
- `OPENCLAW_STATE_DIR` —— 覆盖状态目录
- `OPENCLAW_CONFIG_PATH` —— 覆盖配置文件路径

完整参考： [环境变量](https://docs.openclaw.ai/zh-CN/help/environment)。

## [​](https://docs.openclaw.ai/zh-CN/start/getting-started\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

- [安装概览](https://docs.openclaw.ai/zh-CN/install)
- [渠道概览](https://docs.openclaw.ai/zh-CN/channels)
- [设置](https://docs.openclaw.ai/zh-CN/start/setup)

[功能](https://docs.openclaw.ai/zh-CN/concepts/features) [Onboarding: CLI](https://docs.openclaw.ai/zh-CN/start/wizard)

Ctrl+I

![Install Script Process](https://mintcdn.com/clawdhub/U8jr7qEbUc9OU9YR/assets/install-script.svg?w=1100&fit=max&auto=format&n=U8jr7qEbUc9OU9YR&q=85&s=b5bc84222a0a894ebf01ab00b70c9ec4)