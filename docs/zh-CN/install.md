---
source_url: https://docs.openclaw.ai/zh-CN/install
title: "\u5b89\u88c5 - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/install#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

安装概览

安装

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [系统要求](https://docs.openclaw.ai/zh-CN/install#%E7%B3%BB%E7%BB%9F%E8%A6%81%E6%B1%82)
- [推荐：安装脚本](https://docs.openclaw.ai/zh-CN/install#%E6%8E%A8%E8%8D%90%EF%BC%9A%E5%AE%89%E8%A3%85%E8%84%9A%E6%9C%AC)
- [其他安装方式](https://docs.openclaw.ai/zh-CN/install#%E5%85%B6%E4%BB%96%E5%AE%89%E8%A3%85%E6%96%B9%E5%BC%8F)
- [本地前缀安装器（install-cli.sh）](https://docs.openclaw.ai/zh-CN/install#%E6%9C%AC%E5%9C%B0%E5%89%8D%E7%BC%80%E5%AE%89%E8%A3%85%E5%99%A8%EF%BC%88install-cli-sh%EF%BC%89)
- [npm、pnpm 或 bun](https://docs.openclaw.ai/zh-CN/install#npm%E3%80%81pnpm-%E6%88%96-bun)
- [从源码安装](https://docs.openclaw.ai/zh-CN/install#%E4%BB%8E%E6%BA%90%E7%A0%81%E5%AE%89%E8%A3%85)
- [从 GitHub main 安装](https://docs.openclaw.ai/zh-CN/install#%E4%BB%8E-github-main-%E5%AE%89%E8%A3%85)
- [容器和包管理器](https://docs.openclaw.ai/zh-CN/install#%E5%AE%B9%E5%99%A8%E5%92%8C%E5%8C%85%E7%AE%A1%E7%90%86%E5%99%A8)
- [验证安装](https://docs.openclaw.ai/zh-CN/install#%E9%AA%8C%E8%AF%81%E5%AE%89%E8%A3%85)
- [托管与部署](https://docs.openclaw.ai/zh-CN/install#%E6%89%98%E7%AE%A1%E4%B8%8E%E9%83%A8%E7%BD%B2)
- [更新、迁移或卸载](https://docs.openclaw.ai/zh-CN/install#%E6%9B%B4%E6%96%B0%E3%80%81%E8%BF%81%E7%A7%BB%E6%88%96%E5%8D%B8%E8%BD%BD)
- [故障排除：找不到 openclaw](https://docs.openclaw.ai/zh-CN/install#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4%EF%BC%9A%E6%89%BE%E4%B8%8D%E5%88%B0-openclaw)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

## [​](https://docs.openclaw.ai/zh-CN/install\#%E7%B3%BB%E7%BB%9F%E8%A6%81%E6%B1%82)  系统要求

- **Node 24**（推荐）或 Node 22.14+ —— 安装脚本会自动处理这一点
- **macOS、Linux 或 Windows** —— 同时支持原生 Windows 和 WSL2；WSL2 更稳定。参见 [Windows](https://docs.openclaw.ai/zh-CN/platforms/windows)。
- 只有在你从源码构建时才需要 `pnpm`

## [​](https://docs.openclaw.ai/zh-CN/install\#%E6%8E%A8%E8%8D%90%EF%BC%9A%E5%AE%89%E8%A3%85%E8%84%9A%E6%9C%AC)  推荐：安装脚本

这是最快的安装方式。它会检测你的操作系统，在需要时安装 Node，安装 OpenClaw，并启动新手引导。

- macOS / Linux / WSL2

- Windows (PowerShell)


```
curl -fsSL https://openclaw.ai/install.sh | bash
```

```
iwr -useb https://openclaw.ai/install.ps1 | iex
```

如需安装但不运行新手引导：

- macOS / Linux / WSL2

- Windows (PowerShell)


```
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard
```

```
& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -NoOnboard
```

有关所有标志以及 CI / 自动化选项，请参见 [安装器内部机制](https://docs.openclaw.ai/zh-CN/install/installer)。

## [​](https://docs.openclaw.ai/zh-CN/install\#%E5%85%B6%E4%BB%96%E5%AE%89%E8%A3%85%E6%96%B9%E5%BC%8F)  其他安装方式

### [​](https://docs.openclaw.ai/zh-CN/install\#%E6%9C%AC%E5%9C%B0%E5%89%8D%E7%BC%80%E5%AE%89%E8%A3%85%E5%99%A8%EF%BC%88install-cli-sh%EF%BC%89)  本地前缀安装器（`install-cli.sh`）

如果你希望将 OpenClaw 和 Node 保存在本地前缀目录下（例如 `~/.openclaw`），而不依赖系统范围安装的 Node，请使用此方式：

```
curl -fsSL https://openclaw.ai/install-cli.sh | bash
```

它默认支持 npm 安装，也支持在相同前缀流程下通过 git 检出进行安装。完整参考请见： [安装器内部机制](https://docs.openclaw.ai/zh-CN/install/installer#install-clish)。已经安装过了？你可以使用 `openclaw update --channel dev` 和 `openclaw update --channel stable` 在软件包安装和 git 安装之间切换。参见 [更新](https://docs.openclaw.ai/zh-CN/install/updating#switch-between-npm-and-git-installs)。

### [​](https://docs.openclaw.ai/zh-CN/install\#npm%E3%80%81pnpm-%E6%88%96-bun)  npm、pnpm 或 bun

如果你已经自行管理 Node：

- npm

- pnpm

- bun


```
npm install -g openclaw@latest
openclaw onboard --install-daemon
```

```
pnpm add -g openclaw@latest
pnpm approve-builds -g
openclaw onboard --install-daemon
```

pnpm 对带有构建脚本的软件包要求显式批准。首次安装后请运行 `pnpm approve-builds -g`。

```
bun add -g openclaw@latest
openclaw onboard --install-daemon
```

Bun 支持用于全局 CLI 安装路径。对于 Gateway 网关运行时，Node 仍然是推荐的守护进程运行时。

故障排除：\`sharp\` 构建错误（npm）

如果由于全局安装的 libvips 导致 `sharp` 失败：

```
SHARP_IGNORE_GLOBAL_LIBVIPS=1 npm install -g openclaw@latest
```

### [​](https://docs.openclaw.ai/zh-CN/install\#%E4%BB%8E%E6%BA%90%E7%A0%81%E5%AE%89%E8%A3%85)  从源码安装

适用于贡献者，或任何希望从本地检出运行的人：

```
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install && pnpm build && pnpm ui:build
pnpm link --global
openclaw onboard --install-daemon
```

或者你也可以跳过 link，直接在仓库内部使用 `pnpm openclaw ...`。完整开发工作流请参见 [设置](https://docs.openclaw.ai/zh-CN/start/setup)。

### [​](https://docs.openclaw.ai/zh-CN/install\#%E4%BB%8E-github-main-%E5%AE%89%E8%A3%85)  从 GitHub main 安装

```
npm install -g github:openclaw/openclaw#main
```

### [​](https://docs.openclaw.ai/zh-CN/install\#%E5%AE%B9%E5%99%A8%E5%92%8C%E5%8C%85%E7%AE%A1%E7%90%86%E5%99%A8)  容器和包管理器

[**Docker** \\
\\
容器化或无头部署。](https://docs.openclaw.ai/zh-CN/install/docker)

[**Podman** \\
\\
Docker 的无 root 容器替代方案。](https://docs.openclaw.ai/zh-CN/install/podman)

[**Nix** \\
\\
通过 Nix flake 进行声明式安装。](https://docs.openclaw.ai/zh-CN/install/nix)

[**Ansible** \\
\\
自动化批量部署。](https://docs.openclaw.ai/zh-CN/install/ansible)

[**Bun** \\
\\
通过 Bun 运行时进行仅 CLI 用法。](https://docs.openclaw.ai/zh-CN/install/bun)

## [​](https://docs.openclaw.ai/zh-CN/install\#%E9%AA%8C%E8%AF%81%E5%AE%89%E8%A3%85)  验证安装

```
openclaw --version      # 确认 CLI 可用
openclaw doctor         # 检查配置问题
openclaw gateway status # 验证 Gateway 网关正在运行
```

如果你希望在安装后由系统托管启动：

- macOS：通过 `openclaw onboard --install-daemon` 或 `openclaw gateway install` 安装 LaunchAgent
- Linux / WSL2：通过相同命令安装 systemd 用户服务
- 原生 Windows：优先使用计划任务；如果任务创建被拒绝，则回退为每用户“启动”文件夹登录项

## [​](https://docs.openclaw.ai/zh-CN/install\#%E6%89%98%E7%AE%A1%E4%B8%8E%E9%83%A8%E7%BD%B2)  托管与部署

将 OpenClaw 部署到云服务器或 VPS：

[**VPS** \\
\\
任意 Linux VPS](https://docs.openclaw.ai/zh-CN/vps)

[**Docker VM** \\
\\
共享的 Docker 步骤](https://docs.openclaw.ai/zh-CN/install/docker-vm-runtime)

[**Kubernetes** \\
\\
K8s](https://docs.openclaw.ai/zh-CN/install/kubernetes)

[**Fly.io** \\
\\
Fly.io](https://docs.openclaw.ai/zh-CN/install/fly)

[**Hetzner** \\
\\
Hetzner](https://docs.openclaw.ai/zh-CN/install/hetzner)

[**GCP** \\
\\
Google Cloud](https://docs.openclaw.ai/zh-CN/install/gcp)

[**Azure** \\
\\
Azure](https://docs.openclaw.ai/zh-CN/install/azure)

[**Railway** \\
\\
Railway](https://docs.openclaw.ai/zh-CN/install/railway)

[**Render** \\
\\
Render](https://docs.openclaw.ai/zh-CN/install/render)

[**Northflank** \\
\\
Northflank](https://docs.openclaw.ai/zh-CN/install/northflank)

## [​](https://docs.openclaw.ai/zh-CN/install\#%E6%9B%B4%E6%96%B0%E3%80%81%E8%BF%81%E7%A7%BB%E6%88%96%E5%8D%B8%E8%BD%BD)  更新、迁移或卸载

[**Updating** \\
\\
让 OpenClaw 保持最新。](https://docs.openclaw.ai/zh-CN/install/updating)

[**Migrating** \\
\\
迁移到新机器。](https://docs.openclaw.ai/zh-CN/install/migrating)

[**Uninstall** \\
\\
完全移除 OpenClaw。](https://docs.openclaw.ai/zh-CN/install/uninstall)

## [​](https://docs.openclaw.ai/zh-CN/install\#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4%EF%BC%9A%E6%89%BE%E4%B8%8D%E5%88%B0-openclaw)  故障排除：找不到 `openclaw`

如果安装成功了，但你的终端中找不到 `openclaw`：

```
node -v           # 已安装 Node？
npm prefix -g     # 全局软件包装在哪里？
echo "$PATH"      # 全局 bin 目录是否在 PATH 中？
```

如果 `$(npm prefix -g)/bin` 不在你的 `$PATH` 中，请将它添加到你的 shell 启动文件（`~/.zshrc` 或 `~/.bashrc`）中：

```
export PATH="$(npm prefix -g)/bin:$PATH"
```

然后打开一个新的终端。更多详情请参见 [Node 设置](https://docs.openclaw.ai/zh-CN/install/node)。

[安装程序内部机制](https://docs.openclaw.ai/zh-CN/install/installer)

Ctrl+I