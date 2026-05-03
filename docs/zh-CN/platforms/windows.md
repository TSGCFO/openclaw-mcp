---
source_url: https://docs.openclaw.ai/zh-CN/platforms/windows
title: "Windows - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/platforms/windows#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

平台概览

Windows

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [WSL2（推荐）](https://docs.openclaw.ai/zh-CN/platforms/windows#wsl2%EF%BC%88%E6%8E%A8%E8%8D%90%EF%BC%89)
- [原生 Windows 状态](https://docs.openclaw.ai/zh-CN/platforms/windows#%E5%8E%9F%E7%94%9F-windows-%E7%8A%B6%E6%80%81)
- [Gateway 网关](https://docs.openclaw.ai/zh-CN/platforms/windows#gateway-%E7%BD%91%E5%85%B3)
- [Gateway 网关服务安装（CLI）](https://docs.openclaw.ai/zh-CN/platforms/windows#gateway-%E7%BD%91%E5%85%B3%E6%9C%8D%E5%8A%A1%E5%AE%89%E8%A3%85%EF%BC%88cli%EF%BC%89)
- [Windows 登录前的 Gateway 网关自动启动](https://docs.openclaw.ai/zh-CN/platforms/windows#windows-%E7%99%BB%E5%BD%95%E5%89%8D%E7%9A%84-gateway-%E7%BD%91%E5%85%B3%E8%87%AA%E5%8A%A8%E5%90%AF%E5%8A%A8)
- [1）让用户服务在未登录时继续运行](https://docs.openclaw.ai/zh-CN/platforms/windows#1%EF%BC%89%E8%AE%A9%E7%94%A8%E6%88%B7%E6%9C%8D%E5%8A%A1%E5%9C%A8%E6%9C%AA%E7%99%BB%E5%BD%95%E6%97%B6%E7%BB%A7%E7%BB%AD%E8%BF%90%E8%A1%8C)
- [2）安装 OpenClaw Gateway 网关用户服务](https://docs.openclaw.ai/zh-CN/platforms/windows#2%EF%BC%89%E5%AE%89%E8%A3%85-openclaw-gateway-%E7%BD%91%E5%85%B3%E7%94%A8%E6%88%B7%E6%9C%8D%E5%8A%A1)
- [3）在 Windows 启动时自动启动 WSL](https://docs.openclaw.ai/zh-CN/platforms/windows#3%EF%BC%89%E5%9C%A8-windows-%E5%90%AF%E5%8A%A8%E6%97%B6%E8%87%AA%E5%8A%A8%E5%90%AF%E5%8A%A8-wsl)
- [验证启动链](https://docs.openclaw.ai/zh-CN/platforms/windows#%E9%AA%8C%E8%AF%81%E5%90%AF%E5%8A%A8%E9%93%BE)
- [高级：通过 LAN 暴露 WSL 服务（portproxy）](https://docs.openclaw.ai/zh-CN/platforms/windows#%E9%AB%98%E7%BA%A7%EF%BC%9A%E9%80%9A%E8%BF%87-lan-%E6%9A%B4%E9%9C%B2-wsl-%E6%9C%8D%E5%8A%A1%EF%BC%88portproxy%EF%BC%89)
- [分步 WSL2 安装](https://docs.openclaw.ai/zh-CN/platforms/windows#%E5%88%86%E6%AD%A5-wsl2-%E5%AE%89%E8%A3%85)
- [1）安装 WSL2 + Ubuntu](https://docs.openclaw.ai/zh-CN/platforms/windows#1%EF%BC%89%E5%AE%89%E8%A3%85-wsl2-%2B-ubuntu)
- [2）启用 systemd（Gateway 网关安装所必需）](https://docs.openclaw.ai/zh-CN/platforms/windows#2%EF%BC%89%E5%90%AF%E7%94%A8-systemd%EF%BC%88gateway-%E7%BD%91%E5%85%B3%E5%AE%89%E8%A3%85%E6%89%80%E5%BF%85%E9%9C%80%EF%BC%89)
- [3）安装 OpenClaw（在 WSL 内）](https://docs.openclaw.ai/zh-CN/platforms/windows#3%EF%BC%89%E5%AE%89%E8%A3%85-openclaw%EF%BC%88%E5%9C%A8-wsl-%E5%86%85%EF%BC%89)
- [Windows 配套应用](https://docs.openclaw.ai/zh-CN/platforms/windows#windows-%E9%85%8D%E5%A5%97%E5%BA%94%E7%94%A8)
- [相关内容](https://docs.openclaw.ai/zh-CN/platforms/windows#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw 同时支持 **原生 Windows** 和 **WSL2**。WSL2 是更稳定的路径，也是获得完整体验的推荐方式——CLI、Gateway 网关和工具都在 Linux 内运行，并具备完整兼容性。原生 Windows 可用于核心 CLI 和 Gateway 网关使用，但有一些如下所述的注意事项。原生 Windows 配套应用正在规划中。

## [​](https://docs.openclaw.ai/zh-CN/platforms/windows\#wsl2%EF%BC%88%E6%8E%A8%E8%8D%90%EF%BC%89)  WSL2（推荐）

- [入门指南](https://docs.openclaw.ai/zh-CN/start/getting-started)（请在 WSL 内使用）
- [安装与更新](https://docs.openclaw.ai/zh-CN/install/updating)
- 官方 WSL2 指南（Microsoft）： [https://learn.microsoft.com/windows/wsl/install](https://learn.microsoft.com/windows/wsl/install)

## [​](https://docs.openclaw.ai/zh-CN/platforms/windows\#%E5%8E%9F%E7%94%9F-windows-%E7%8A%B6%E6%80%81)  原生 Windows 状态

原生 Windows CLI 流程正在不断改进，但 WSL2 仍然是推荐路径。目前原生 Windows 上运行良好的内容：

- 通过 `install.ps1` 使用网站安装器
- 本地 CLI 使用，例如 `openclaw --version`、`openclaw doctor` 和 `openclaw plugins list --json`
- 内嵌本地智能体 / 提供商冒烟测试，例如：

```
openclaw agent --local --agent main --thinking low -m "Reply with exactly WINDOWS-HATCH-OK."
```

当前注意事项：

- `openclaw onboard --non-interactive` 仍然要求本地 Gateway 网关可访问，除非你传入 `--skip-health`
- `openclaw onboard --non-interactive --install-daemon` 和 `openclaw gateway install` 会优先尝试 Windows Scheduled Tasks
- 如果 Scheduled Task 创建被拒绝，OpenClaw 会回退为每用户 Startup 文件夹登录项，并立即启动 Gateway 网关
- 如果 `schtasks` 自身卡住或停止响应，OpenClaw 现在会快速中止该路径并回退，而不是永远挂起
- 在可用时仍优先使用 Scheduled Tasks，因为它们能提供更好的 supervisor 状态

如果你只想使用原生 CLI，而不安装 Gateway 网关服务，请使用以下其中一种方式：

```
openclaw onboard --non-interactive --skip-health
openclaw gateway run
```

如果你确实想在原生 Windows 上启用托管启动：

```
openclaw gateway install
openclaw gateway status --json
```

如果 Scheduled Task 创建被阻止，回退服务模式仍会通过当前用户的 Startup 文件夹在登录后自动启动。

## [​](https://docs.openclaw.ai/zh-CN/platforms/windows\#gateway-%E7%BD%91%E5%85%B3)  Gateway 网关

- [Gateway 网关运行手册](https://docs.openclaw.ai/zh-CN/gateway)
- [配置](https://docs.openclaw.ai/zh-CN/gateway/configuration)

## [​](https://docs.openclaw.ai/zh-CN/platforms/windows\#gateway-%E7%BD%91%E5%85%B3%E6%9C%8D%E5%8A%A1%E5%AE%89%E8%A3%85%EF%BC%88cli%EF%BC%89)  Gateway 网关服务安装（CLI）

在 WSL2 内：

```
openclaw onboard --install-daemon
```

或：

```
openclaw gateway install
```

或：

```
openclaw configure
```

在提示时选择 **Gateway service**。修复 / 迁移：

```
openclaw doctor
```

## [​](https://docs.openclaw.ai/zh-CN/platforms/windows\#windows-%E7%99%BB%E5%BD%95%E5%89%8D%E7%9A%84-gateway-%E7%BD%91%E5%85%B3%E8%87%AA%E5%8A%A8%E5%90%AF%E5%8A%A8)  Windows 登录前的 Gateway 网关自动启动

对于无头部署，请确保完整的启动链即使在无人登录 Windows 时也能运行。

### [​](https://docs.openclaw.ai/zh-CN/platforms/windows\#1%EF%BC%89%E8%AE%A9%E7%94%A8%E6%88%B7%E6%9C%8D%E5%8A%A1%E5%9C%A8%E6%9C%AA%E7%99%BB%E5%BD%95%E6%97%B6%E7%BB%A7%E7%BB%AD%E8%BF%90%E8%A1%8C)  1）让用户服务在未登录时继续运行

在 WSL 内：

```
sudo loginctl enable-linger "$(whoami)"
```

### [​](https://docs.openclaw.ai/zh-CN/platforms/windows\#2%EF%BC%89%E5%AE%89%E8%A3%85-openclaw-gateway-%E7%BD%91%E5%85%B3%E7%94%A8%E6%88%B7%E6%9C%8D%E5%8A%A1)  2）安装 OpenClaw Gateway 网关用户服务

在 WSL 内：

```
openclaw gateway install
```

### [​](https://docs.openclaw.ai/zh-CN/platforms/windows\#3%EF%BC%89%E5%9C%A8-windows-%E5%90%AF%E5%8A%A8%E6%97%B6%E8%87%AA%E5%8A%A8%E5%90%AF%E5%8A%A8-wsl)  3）在 Windows 启动时自动启动 WSL

在以管理员身份运行的 PowerShell 中：

```
schtasks /create /tn "WSL Boot" /tr "wsl.exe -d Ubuntu --exec /bin/true" /sc onstart /ru SYSTEM
```

将 `Ubuntu` 替换为以下命令输出中的发行版名称：

```
wsl --list --verbose
```

### [​](https://docs.openclaw.ai/zh-CN/platforms/windows\#%E9%AA%8C%E8%AF%81%E5%90%AF%E5%8A%A8%E9%93%BE)  验证启动链

重启后（在 Windows 登录前），从 WSL 中检查：

```
systemctl --user is-enabled openclaw-gateway.service
systemctl --user status openclaw-gateway.service --no-pager
```

## [​](https://docs.openclaw.ai/zh-CN/platforms/windows\#%E9%AB%98%E7%BA%A7%EF%BC%9A%E9%80%9A%E8%BF%87-lan-%E6%9A%B4%E9%9C%B2-wsl-%E6%9C%8D%E5%8A%A1%EF%BC%88portproxy%EF%BC%89)  高级：通过 LAN 暴露 WSL 服务（portproxy）

WSL 有自己的虚拟网络。如果另一台机器需要访问 **运行在 WSL 内部** 的服务
（SSH、本地 TTS 服务器或 Gateway 网关），你必须将 Windows 端口
转发到当前 WSL IP。WSL IP 会在重启后变化，
因此你可能需要刷新转发规则。示例（以管理员身份运行 PowerShell）：

```
$Distro = "Ubuntu-24.04"
$ListenPort = 2222
$TargetPort = 22

$WslIp = (wsl -d $Distro -- hostname -I).Trim().Split(" ")[0]
if (-not $WslIp) { throw "WSL IP not found." }

netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=$ListenPort `
  connectaddress=$WslIp connectport=$TargetPort
```

允许该端口通过 Windows 防火墙（一次性）：

```
New-NetFirewallRule -DisplayName "WSL SSH $ListenPort" -Direction Inbound `
  -Protocol TCP -LocalPort $ListenPort -Action Allow
```

在 WSL 重启后刷新 portproxy：

```
netsh interface portproxy delete v4tov4 listenport=$ListenPort listenaddress=0.0.0.0 | Out-Null
netsh interface portproxy add v4tov4 listenport=$ListenPort listenaddress=0.0.0.0 `
  connectaddress=$WslIp connectport=$TargetPort | Out-Null
```

说明：

- 从另一台机器发起 SSH 时，目标应是 **Windows 主机 IP**（例如：`ssh user@windows-host -p 2222`）。
- 远程节点必须指向一个 **可访问的** Gateway 网关 URL（而不是 `127.0.0.1`）；请使用
`openclaw status --all` 进行确认。
- 对于 LAN 访问，请使用 `listenaddress=0.0.0.0`；使用 `127.0.0.1` 则仅限本地访问。
- 如果你想自动完成此操作，请注册一个 Scheduled Task，在登录时运行刷新步骤。

## [​](https://docs.openclaw.ai/zh-CN/platforms/windows\#%E5%88%86%E6%AD%A5-wsl2-%E5%AE%89%E8%A3%85)  分步 WSL2 安装

### [​](https://docs.openclaw.ai/zh-CN/platforms/windows\#1%EF%BC%89%E5%AE%89%E8%A3%85-wsl2-+-ubuntu)  1）安装 WSL2 + Ubuntu

打开 PowerShell（管理员）：

```
wsl --install
# 或显式选择发行版：
wsl --list --online
wsl --install -d Ubuntu-24.04
```

如果 Windows 提示，请重启。

### [​](https://docs.openclaw.ai/zh-CN/platforms/windows\#2%EF%BC%89%E5%90%AF%E7%94%A8-systemd%EF%BC%88gateway-%E7%BD%91%E5%85%B3%E5%AE%89%E8%A3%85%E6%89%80%E5%BF%85%E9%9C%80%EF%BC%89)  2）启用 systemd（Gateway 网关安装所必需）

在你的 WSL 终端中：

```
sudo tee /etc/wsl.conf >/dev/null <<'EOF'
[boot]
systemd=true
EOF
```

然后在 PowerShell 中运行：

```
wsl --shutdown
```

重新打开 Ubuntu，然后验证：

```
systemctl --user status
```

### [​](https://docs.openclaw.ai/zh-CN/platforms/windows\#3%EF%BC%89%E5%AE%89%E8%A3%85-openclaw%EF%BC%88%E5%9C%A8-wsl-%E5%86%85%EF%BC%89)  3）安装 OpenClaw（在 WSL 内）

如果你是在 WSL 内首次进行普通安装，请遵循 Linux 的 入门指南 流程：

```
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install
pnpm build
pnpm ui:build
pnpm openclaw onboard --install-daemon
```

如果你是在从源码开发，而不是进行首次新手引导，请使用 [设置](https://docs.openclaw.ai/zh-CN/start/setup) 中的
源码开发循环：

```
pnpm install
# 仅首次运行（或在重置本地 OpenClaw 配置 / 工作区后）
pnpm openclaw setup
pnpm gateway:watch
```

完整指南： [入门指南](https://docs.openclaw.ai/zh-CN/start/getting-started)

## [​](https://docs.openclaw.ai/zh-CN/platforms/windows\#windows-%E9%85%8D%E5%A5%97%E5%BA%94%E7%94%A8)  Windows 配套应用

我们目前还没有 Windows 配套应用。如果你希望推动这件事发生，欢迎贡献。

## [​](https://docs.openclaw.ai/zh-CN/platforms/windows\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

- [安装概览](https://docs.openclaw.ai/zh-CN/install)
- [平台](https://docs.openclaw.ai/zh-CN/platforms)

[Linux 应用](https://docs.openclaw.ai/zh-CN/platforms/linux) [Android 应用](https://docs.openclaw.ai/zh-CN/platforms/android)

Ctrl+I