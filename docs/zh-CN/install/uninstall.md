---
source_url: https://docs.openclaw.ai/zh-CN/install/uninstall
title: "\u5378\u8f7d - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/install/uninstall#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

维护

卸载

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [简易路径（CLI 仍已安装）](https://docs.openclaw.ai/zh-CN/install/uninstall#%E7%AE%80%E6%98%93%E8%B7%AF%E5%BE%84%EF%BC%88cli-%E4%BB%8D%E5%B7%B2%E5%AE%89%E8%A3%85%EF%BC%89)
- [手动移除服务（CLI 未安装）](https://docs.openclaw.ai/zh-CN/install/uninstall#%E6%89%8B%E5%8A%A8%E7%A7%BB%E9%99%A4%E6%9C%8D%E5%8A%A1%EF%BC%88cli-%E6%9C%AA%E5%AE%89%E8%A3%85%EF%BC%89)
- [macOS（launchd）](https://docs.openclaw.ai/zh-CN/install/uninstall#macos%EF%BC%88launchd%EF%BC%89)
- [Linux（systemd 用户单元）](https://docs.openclaw.ai/zh-CN/install/uninstall#linux%EF%BC%88systemd-%E7%94%A8%E6%88%B7%E5%8D%95%E5%85%83%EF%BC%89)
- [Windows（计划任务）](https://docs.openclaw.ai/zh-CN/install/uninstall#windows%EF%BC%88%E8%AE%A1%E5%88%92%E4%BB%BB%E5%8A%A1%EF%BC%89)
- [正常安装 vs 源码检出](https://docs.openclaw.ai/zh-CN/install/uninstall#%E6%AD%A3%E5%B8%B8%E5%AE%89%E8%A3%85-vs-%E6%BA%90%E7%A0%81%E6%A3%80%E5%87%BA)
- [正常安装（install.sh / npm / pnpm / bun）](https://docs.openclaw.ai/zh-CN/install/uninstall#%E6%AD%A3%E5%B8%B8%E5%AE%89%E8%A3%85%EF%BC%88install-sh-%2F-npm-%2F-pnpm-%2F-bun%EF%BC%89)
- [源码检出（git clone）](https://docs.openclaw.ai/zh-CN/install/uninstall#%E6%BA%90%E7%A0%81%E6%A3%80%E5%87%BA%EF%BC%88git-clone%EF%BC%89)
- [相关](https://docs.openclaw.ai/zh-CN/install/uninstall#%E7%9B%B8%E5%85%B3)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

两种路径：

- 如果 `openclaw` 仍已安装，使用 **简易路径**。
- 如果 CLI 已不存在但服务仍在运行，使用 **手动移除服务**。

## [​](https://docs.openclaw.ai/zh-CN/install/uninstall\#%E7%AE%80%E6%98%93%E8%B7%AF%E5%BE%84%EF%BC%88cli-%E4%BB%8D%E5%B7%B2%E5%AE%89%E8%A3%85%EF%BC%89)  简易路径（CLI 仍已安装）

推荐：使用内置卸载程序：

```
openclaw uninstall
```

非交互式（自动化 / npx）：

```
openclaw uninstall --all --yes --non-interactive
npx -y openclaw uninstall --all --yes --non-interactive
```

手动步骤（结果相同）：

1. 停止 Gateway 网关服务：

```
openclaw gateway stop
```

2. 卸载 Gateway 网关服务（launchd/systemd/schtasks）：

```
openclaw gateway uninstall
```

3. 删除状态 \+ 配置：

```
rm -rf "${OPENCLAW_STATE_DIR:-$HOME/.openclaw}"
```

如果你将 `OPENCLAW_CONFIG_PATH` 设置为状态目录之外的自定义位置，也请删除该文件。

4. 删除你的工作区（可选，会移除智能体文件）：

```
rm -rf ~/.openclaw/workspace
```

5. 移除 CLI 安装（选择你使用的方式）：

```
npm rm -g openclaw
pnpm remove -g openclaw
bun remove -g openclaw
```

6. 如果你安装了 macOS 应用：

```
rm -rf /Applications/OpenClaw.app
```

说明：

- 如果你使用了配置文件（`--profile` / `OPENCLAW_PROFILE`），请对每个状态目录重复步骤 3（默认值为 `~/.openclaw-<profile>`）。
- 在远程模式下，状态目录位于 **Gateway 网关主机** 上，因此也要在那里运行步骤 1-4。

## [​](https://docs.openclaw.ai/zh-CN/install/uninstall\#%E6%89%8B%E5%8A%A8%E7%A7%BB%E9%99%A4%E6%9C%8D%E5%8A%A1%EF%BC%88cli-%E6%9C%AA%E5%AE%89%E8%A3%85%EF%BC%89)  手动移除服务（CLI 未安装）

如果 Gateway 网关服务持续运行但 `openclaw` 缺失，请使用此方法。

### [​](https://docs.openclaw.ai/zh-CN/install/uninstall\#macos%EF%BC%88launchd%EF%BC%89)  macOS（launchd）

默认标签为 `ai.openclaw.gateway`（或 `ai.openclaw.<profile>`；旧版 `com.openclaw.*` 可能仍然存在）：

```
launchctl bootout gui/$UID/ai.openclaw.gateway
rm -f ~/Library/LaunchAgents/ai.openclaw.gateway.plist
```

如果你使用了配置文件，请将标签和 plist 名称替换为 `ai.openclaw.<profile>`。如果存在任何旧版 `com.openclaw.*` plist，也请将其删除。

### [​](https://docs.openclaw.ai/zh-CN/install/uninstall\#linux%EF%BC%88systemd-%E7%94%A8%E6%88%B7%E5%8D%95%E5%85%83%EF%BC%89)  Linux（systemd 用户单元）

默认单元名称为 `openclaw-gateway.service`（或 `openclaw-gateway-<profile>.service`）：

```
systemctl --user disable --now openclaw-gateway.service
rm -f ~/.config/systemd/user/openclaw-gateway.service
systemctl --user daemon-reload
```

### [​](https://docs.openclaw.ai/zh-CN/install/uninstall\#windows%EF%BC%88%E8%AE%A1%E5%88%92%E4%BB%BB%E5%8A%A1%EF%BC%89)  Windows（计划任务）

默认任务名称为 `OpenClaw Gateway`（或 `OpenClaw Gateway (<profile>)`）。
任务脚本位于你的状态目录下。

```
schtasks /Delete /F /TN "OpenClaw Gateway"
Remove-Item -Force "$env:USERPROFILE\.openclaw\gateway.cmd"
```

如果你使用了配置文件，请删除匹配的任务名称和 `~\.openclaw-<profile>\gateway.cmd`。

## [​](https://docs.openclaw.ai/zh-CN/install/uninstall\#%E6%AD%A3%E5%B8%B8%E5%AE%89%E8%A3%85-vs-%E6%BA%90%E7%A0%81%E6%A3%80%E5%87%BA)  正常安装 vs 源码检出

### [​](https://docs.openclaw.ai/zh-CN/install/uninstall\#%E6%AD%A3%E5%B8%B8%E5%AE%89%E8%A3%85%EF%BC%88install-sh-/-npm-/-pnpm-/-bun%EF%BC%89)  正常安装（install.sh / npm / pnpm / bun）

如果你使用了 `https://openclaw.ai/install.sh` 或 `install.ps1`，CLI 是通过 `npm install -g openclaw@latest` 安装的。
请使用 `npm rm -g openclaw` 将其移除（如果你是通过 `pnpm` / `bun` 安装的，则使用 `pnpm remove -g` / `bun remove -g`）。

### [​](https://docs.openclaw.ai/zh-CN/install/uninstall\#%E6%BA%90%E7%A0%81%E6%A3%80%E5%87%BA%EF%BC%88git-clone%EF%BC%89)  源码检出（git clone）

如果你是从仓库检出运行（`git clone` \+ `openclaw ...` / `bun run openclaw ...`）：

1. 在删除仓库之前 **先** 卸载 Gateway 网关服务（使用上面的简易路径或手动移除服务）。
2. 删除仓库目录。
3. 按上文所示移除状态 \+ 工作区。

## [​](https://docs.openclaw.ai/zh-CN/install/uninstall\#%E7%9B%B8%E5%85%B3)  相关

- [安装概览](https://docs.openclaw.ai/zh-CN/install)
- [迁移指南](https://docs.openclaw.ai/zh-CN/install/migrating)

[迁移指南](https://docs.openclaw.ai/zh-CN/install/migrating) [Linux Server](https://docs.openclaw.ai/zh-CN/vps)

Ctrl+I