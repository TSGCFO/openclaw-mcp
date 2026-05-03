---
source_url: https://docs.openclaw.ai/zh-CN/tools/browser-login
title: "\u6d4f\u89c8\u5668\u767b\u5f55 - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/tools/browser-login#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

浏览器

浏览器登录

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [浏览器登录 \+ X/Twitter 发帖](https://docs.openclaw.ai/zh-CN/tools/browser-login#%E6%B5%8F%E8%A7%88%E5%99%A8%E7%99%BB%E5%BD%95-%2B-x%2Ftwitter-%E5%8F%91%E5%B8%96)
- [手动登录（推荐）](https://docs.openclaw.ai/zh-CN/tools/browser-login#%E6%89%8B%E5%8A%A8%E7%99%BB%E5%BD%95%EF%BC%88%E6%8E%A8%E8%8D%90%EF%BC%89)
- [使用的是哪个 Chrome 配置文件？](https://docs.openclaw.ai/zh-CN/tools/browser-login#%E4%BD%BF%E7%94%A8%E7%9A%84%E6%98%AF%E5%93%AA%E4%B8%AA-chrome-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%EF%BC%9F)
- [X/Twitter：推荐流程](https://docs.openclaw.ai/zh-CN/tools/browser-login#x%2Ftwitter%EF%BC%9A%E6%8E%A8%E8%8D%90%E6%B5%81%E7%A8%8B)
- [沙箱隔离 \+ 主机浏览器访问](https://docs.openclaw.ai/zh-CN/tools/browser-login#%E6%B2%99%E7%AE%B1%E9%9A%94%E7%A6%BB-%2B-%E4%B8%BB%E6%9C%BA%E6%B5%8F%E8%A7%88%E5%99%A8%E8%AE%BF%E9%97%AE)
- [相关](https://docs.openclaw.ai/zh-CN/tools/browser-login#%E7%9B%B8%E5%85%B3)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/zh-CN/tools/browser-login\#%E6%B5%8F%E8%A7%88%E5%99%A8%E7%99%BB%E5%BD%95-+-x/twitter-%E5%8F%91%E5%B8%96)  浏览器登录 \+ X/Twitter 发帖

## [​](https://docs.openclaw.ai/zh-CN/tools/browser-login\#%E6%89%8B%E5%8A%A8%E7%99%BB%E5%BD%95%EF%BC%88%E6%8E%A8%E8%8D%90%EF%BC%89)  手动登录（推荐）

当某个网站要求登录时，请在 **主机** 浏览器配置文件（即 openclaw 浏览器）中 **手动登录**。**不要** 将你的凭证提供给模型。自动化登录通常会触发反机器人防护，并可能导致账号被锁定。返回主浏览器文档： [浏览器](https://docs.openclaw.ai/zh-CN/tools/browser)。

## [​](https://docs.openclaw.ai/zh-CN/tools/browser-login\#%E4%BD%BF%E7%94%A8%E7%9A%84%E6%98%AF%E5%93%AA%E4%B8%AA-chrome-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%EF%BC%9F)  使用的是哪个 Chrome 配置文件？

OpenClaw 控制的是一个 **专用 Chrome 配置文件**（名为 `openclaw`，UI 带橙色色调）。它与你日常使用的浏览器配置文件是分开的。对于智能体浏览器工具调用：

- 默认选择：智能体应使用其隔离的 `openclaw` 浏览器。
- 仅当现有登录会话很重要，且用户本人就在电脑前可以点击/批准任何附加提示时，才使用 `profile="user"`。
- 如果你有多个用户浏览器配置文件，请显式指定配置文件，而不要猜测。

有两种简单方式访问它：

1. **让智能体打开浏览器**，然后你自己完成登录。
2. **通过 CLI 打开它**：

```
openclaw browser start
openclaw browser open https://x.com
```

如果你有多个配置文件，请传入 `--browser-profile <name>`（默认是 `openclaw`）。

## [​](https://docs.openclaw.ai/zh-CN/tools/browser-login\#x/twitter%EF%BC%9A%E6%8E%A8%E8%8D%90%E6%B5%81%E7%A8%8B)  X/Twitter：推荐流程

- **阅读/搜索/线程：** 使用 **主机** 浏览器（手动登录）。
- **发布更新：** 使用 **主机** 浏览器（手动登录）。

## [​](https://docs.openclaw.ai/zh-CN/tools/browser-login\#%E6%B2%99%E7%AE%B1%E9%9A%94%E7%A6%BB-+-%E4%B8%BB%E6%9C%BA%E6%B5%8F%E8%A7%88%E5%99%A8%E8%AE%BF%E9%97%AE)  沙箱隔离 \+ 主机浏览器访问

沙箱隔离浏览器会话 **更容易** 触发机器人检测。对于 X/Twitter（以及其他严格网站），请优先使用 **主机** 浏览器。如果智能体处于沙箱隔离中，浏览器工具默认会使用沙箱。若要允许控制主机浏览器：

```
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main",
        browser: {
          allowHostControl: true,
        },
      },
    },
  },
}
```

然后将目标设为主机浏览器：

```
openclaw browser open https://x.com --browser-profile openclaw --target host
```

或者为负责发布更新的智能体禁用沙箱隔离。

## [​](https://docs.openclaw.ai/zh-CN/tools/browser-login\#%E7%9B%B8%E5%85%B3)  相关

- [浏览器](https://docs.openclaw.ai/zh-CN/tools/browser)
- [浏览器 Linux 故障排除](https://docs.openclaw.ai/zh-CN/tools/browser-linux-troubleshooting)
- [浏览器 WSL2 故障排除](https://docs.openclaw.ai/zh-CN/tools/browser-wsl2-windows-remote-cdp-troubleshooting)

[浏览器（由 OpenClaw 管理）](https://docs.openclaw.ai/zh-CN/tools/browser) [浏览器故障排除](https://docs.openclaw.ai/zh-CN/tools/browser-linux-troubleshooting)

Ctrl+I