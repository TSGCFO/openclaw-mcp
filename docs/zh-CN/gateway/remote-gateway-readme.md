---
source_url: https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme
title: "\u8fdc\u7a0b Gateway \u7f51\u5173\u8bbe\u7f6e - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

远程访问

远程 Gateway 网关设置

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [使用远程 Gateway 网关运行 OpenClaw.app](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme#%E4%BD%BF%E7%94%A8%E8%BF%9C%E7%A8%8B-gateway-%E7%BD%91%E5%85%B3%E8%BF%90%E8%A1%8C-openclaw-app)
- [概览](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme#%E6%A6%82%E8%A7%88)
- [快速开始](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)
- [第 1 步：添加 SSH 配置](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme#%E7%AC%AC-1-%E6%AD%A5%EF%BC%9A%E6%B7%BB%E5%8A%A0-ssh-%E9%85%8D%E7%BD%AE)
- [第 2 步：复制 SSH 密钥](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme#%E7%AC%AC-2-%E6%AD%A5%EF%BC%9A%E5%A4%8D%E5%88%B6-ssh-%E5%AF%86%E9%92%A5)
- [第 3 步：配置远程 Gateway 网关认证](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme#%E7%AC%AC-3-%E6%AD%A5%EF%BC%9A%E9%85%8D%E7%BD%AE%E8%BF%9C%E7%A8%8B-gateway-%E7%BD%91%E5%85%B3%E8%AE%A4%E8%AF%81)
- [第 4 步：启动 SSH 隧道](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme#%E7%AC%AC-4-%E6%AD%A5%EF%BC%9A%E5%90%AF%E5%8A%A8-ssh-%E9%9A%A7%E9%81%93)
- [第 5 步：重启 OpenClaw.app](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme#%E7%AC%AC-5-%E6%AD%A5%EF%BC%9A%E9%87%8D%E5%90%AF-openclaw-app)
- [登录时自动启动隧道](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme#%E7%99%BB%E5%BD%95%E6%97%B6%E8%87%AA%E5%8A%A8%E5%90%AF%E5%8A%A8%E9%9A%A7%E9%81%93)
- [创建 PLIST 文件](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme#%E5%88%9B%E5%BB%BA-plist-%E6%96%87%E4%BB%B6)
- [加载 Launch Agent](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme#%E5%8A%A0%E8%BD%BD-launch-agent)
- [故障排除](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4)
- [工作原理](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme#%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86)
- [相关内容](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

> 此内容已合并到 [远程访问](https://docs.openclaw.ai/zh-CN/gateway/remote#macos-persistent-ssh-tunnel-via-launchagent)。当前指南请参见该页面。

# [​](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme\#%E4%BD%BF%E7%94%A8%E8%BF%9C%E7%A8%8B-gateway-%E7%BD%91%E5%85%B3%E8%BF%90%E8%A1%8C-openclaw-app)  使用远程 Gateway 网关运行 OpenClaw.app

OpenClaw.app 使用 SSH 隧道连接到远程 Gateway 网关。本指南将向你展示如何进行设置。

## [​](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme\#%E6%A6%82%E8%A7%88)  概览

远程机器

客户端机器

OpenClaw.app

ws://127.0.0.1:18789

（本地端口）

SSH 隧道

Gateway 网关 WebSocket

ws://127.0.0.1:18789

## [​](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme\#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)  快速开始

### [​](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme\#%E7%AC%AC-1-%E6%AD%A5%EF%BC%9A%E6%B7%BB%E5%8A%A0-ssh-%E9%85%8D%E7%BD%AE)  第 1 步：添加 SSH 配置

编辑 `~/.ssh/config` 并添加：

```
Host remote-gateway
    HostName <REMOTE_IP>          # 例如：172.27.187.184
    User <REMOTE_USER>            # 例如：jefferson
    LocalForward 18789 127.0.0.1:18789
    IdentityFile ~/.ssh/id_rsa
```

将 `<REMOTE_IP>` 和 `<REMOTE_USER>` 替换为你的实际值。

### [​](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme\#%E7%AC%AC-2-%E6%AD%A5%EF%BC%9A%E5%A4%8D%E5%88%B6-ssh-%E5%AF%86%E9%92%A5)  第 2 步：复制 SSH 密钥

将你的公钥复制到远程机器（输入一次密码）：

```
ssh-copy-id -i ~/.ssh/id_rsa <REMOTE_USER>@<REMOTE_IP>
```

### [​](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme\#%E7%AC%AC-3-%E6%AD%A5%EF%BC%9A%E9%85%8D%E7%BD%AE%E8%BF%9C%E7%A8%8B-gateway-%E7%BD%91%E5%85%B3%E8%AE%A4%E8%AF%81)  第 3 步：配置远程 Gateway 网关认证

```
openclaw config set gateway.remote.token "<your-token>"
```

如果你的远程 Gateway 网关使用密码认证，请改用 `gateway.remote.password`。
`OPENCLAW_GATEWAY_TOKEN` 仍然可作为 shell 级覆盖使用，但持久化的远程客户端设置应使用 `gateway.remote.token` / `gateway.remote.password`。

### [​](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme\#%E7%AC%AC-4-%E6%AD%A5%EF%BC%9A%E5%90%AF%E5%8A%A8-ssh-%E9%9A%A7%E9%81%93)  第 4 步：启动 SSH 隧道

```
ssh -N remote-gateway &
```

### [​](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme\#%E7%AC%AC-5-%E6%AD%A5%EF%BC%9A%E9%87%8D%E5%90%AF-openclaw-app)  第 5 步：重启 OpenClaw.app

```
# 退出 OpenClaw.app（⌘Q），然后重新打开：
open /path/to/OpenClaw.app
```

应用现在将通过 SSH 隧道连接到远程 Gateway 网关。

* * *

## [​](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme\#%E7%99%BB%E5%BD%95%E6%97%B6%E8%87%AA%E5%8A%A8%E5%90%AF%E5%8A%A8%E9%9A%A7%E9%81%93)  登录时自动启动隧道

如果你希望 SSH 隧道在登录时自动启动，可以创建一个 Launch Agent。

### [​](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme\#%E5%88%9B%E5%BB%BA-plist-%E6%96%87%E4%BB%B6)  创建 PLIST 文件

将以下内容保存为 `~/Library/LaunchAgents/ai.openclaw.ssh-tunnel.plist`：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>ai.openclaw.ssh-tunnel</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/bin/ssh</string>
        <string>-N</string>
        <string>remote-gateway</string>
    </array>
    <key>KeepAlive</key>
    <true/>
    <key>RunAtLoad</key>
    <true/>
</dict>
</plist>
```

### [​](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme\#%E5%8A%A0%E8%BD%BD-launch-agent)  加载 Launch Agent

```
launchctl bootstrap gui/$UID ~/Library/LaunchAgents/ai.openclaw.ssh-tunnel.plist
```

现在，该隧道将会：

- 在你登录时自动启动
- 如果崩溃会自动重启
- 在后台持续运行

旧版说明：如果存在残留的 `com.openclaw.ssh-tunnel` LaunchAgent，请将其移除。

* * *

## [​](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme\#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4)  故障排除

**检查隧道是否正在运行：**

```
ps aux | grep "ssh -N remote-gateway" | grep -v grep
lsof -i :18789
```

**重启隧道：**

```
launchctl kickstart -k gui/$UID/ai.openclaw.ssh-tunnel
```

**停止隧道：**

```
launchctl bootout gui/$UID/ai.openclaw.ssh-tunnel
```

* * *

## [​](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme\#%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86)  工作原理

| 组件 | 作用 |
| --- | --- |
| `LocalForward 18789 127.0.0.1:18789` | 将本地端口 18789 转发到远程端口 18789 |
| `ssh -N` | SSH 连接但不执行远程命令（仅进行端口转发） |
| `KeepAlive` | 如果隧道崩溃则自动重启 |
| `RunAtLoad` | 在代理加载时启动隧道 |

OpenClaw.app 会连接到你客户端机器上的 `ws://127.0.0.1:18789`。SSH 隧道会将该连接转发到远程机器上的 18789 端口，也就是 Gateway 网关运行的端口。

## [​](https://docs.openclaw.ai/zh-CN/gateway/remote-gateway-readme\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

- [远程访问](https://docs.openclaw.ai/zh-CN/gateway/remote)
- [Tailscale](https://docs.openclaw.ai/zh-CN/gateway/tailscale)

[远程访问](https://docs.openclaw.ai/zh-CN/gateway/remote) [Tailscale](https://docs.openclaw.ai/zh-CN/gateway/tailscale)

Ctrl+I