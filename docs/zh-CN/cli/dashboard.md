---
source_url: https://docs.openclaw.ai/zh-CN/cli/dashboard
title: "\u4eea\u8868\u677f - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/cli/dashboard#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

CLI 命令

仪表板

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [openclaw dashboard](https://docs.openclaw.ai/zh-CN/cli/dashboard#openclaw-dashboard)
- [相关内容](https://docs.openclaw.ai/zh-CN/cli/dashboard#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/zh-CN/cli/dashboard\#openclaw-dashboard)  `openclaw dashboard`

使用你当前的身份验证打开控制界面。

```
openclaw dashboard
openclaw dashboard --no-open
```

注意：

- `dashboard` 会在可能的情况下解析已配置的 `gateway.auth.token` SecretRefs。
- `dashboard` 会遵循 `gateway.tls.enabled`：启用 TLS 的 Gateway 网关会打印/打开 `https://` 控制界面 URL，并通过 `wss://` 连接。
- 对于由 SecretRef 管理的令牌（无论已解析还是未解析），`dashboard` 会打印/复制/打开不带令牌的 URL，以避免在终端输出、剪贴板历史记录或浏览器启动参数中暴露外部密钥。
- 如果 `gateway.auth.token` 由 SecretRef 管理，但在此命令路径中未解析，该命令会打印不带令牌的 URL 和明确的修复指导，而不是嵌入无效的令牌占位符。

## [​](https://docs.openclaw.ai/zh-CN/cli/dashboard\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

- [CLI 参考](https://docs.openclaw.ai/zh-CN/cli)
- [仪表板](https://docs.openclaw.ai/zh-CN/web/dashboard)

[定时调度](https://docs.openclaw.ai/zh-CN/cli/cron) [设备](https://docs.openclaw.ai/zh-CN/cli/devices)

Ctrl+I