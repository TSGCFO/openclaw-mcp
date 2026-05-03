---
source_url: https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup
title: "macOS \u5f00\u53d1\u73af\u5883\u8bbe\u7f6e - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

macOS 配套应用

macOS 开发环境设置

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [macOS 开发者设置](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup#macos-%E5%BC%80%E5%8F%91%E8%80%85%E8%AE%BE%E7%BD%AE)
- [前置要求](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup#%E5%89%8D%E7%BD%AE%E8%A6%81%E6%B1%82)
- [1\. 安装依赖](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup#1-%E5%AE%89%E8%A3%85%E4%BE%9D%E8%B5%96)
- [2\. 构建并打包应用](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup#2-%E6%9E%84%E5%BB%BA%E5%B9%B6%E6%89%93%E5%8C%85%E5%BA%94%E7%94%A8)
- [3\. 安装 CLI](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup#3-%E5%AE%89%E8%A3%85-cli)
- [故障排除](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4)
- [构建失败：工具链或 SDK 不匹配](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup#%E6%9E%84%E5%BB%BA%E5%A4%B1%E8%B4%A5%EF%BC%9A%E5%B7%A5%E5%85%B7%E9%93%BE%E6%88%96-sdk-%E4%B8%8D%E5%8C%B9%E9%85%8D)
- [授予权限时应用崩溃](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup#%E6%8E%88%E4%BA%88%E6%9D%83%E9%99%90%E6%97%B6%E5%BA%94%E7%94%A8%E5%B4%A9%E6%BA%83)
- [Gateway 网关一直显示 “Starting…”](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup#gateway-%E7%BD%91%E5%85%B3%E4%B8%80%E7%9B%B4%E6%98%BE%E7%A4%BA-%E2%80%9Cstarting%E2%80%A6%E2%80%9D)
- [相关内容](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup\#macos-%E5%BC%80%E5%8F%91%E8%80%85%E8%AE%BE%E7%BD%AE)  macOS 开发者设置

从源代码构建并运行 OpenClaw macOS 应用。

## [​](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup\#%E5%89%8D%E7%BD%AE%E8%A6%81%E6%B1%82)  前置要求

在构建应用之前，请确保你已安装以下内容：

1. **Xcode 26.2+**：Swift 开发所必需。
2. **Node.js 24 和 pnpm**：推荐用于 Gateway 网关、CLI 和打包脚本。Node 22 LTS（当前为 `22.14+`）仍受支持，以保持兼容性。

## [​](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup\#1-%E5%AE%89%E8%A3%85%E4%BE%9D%E8%B5%96)  1\. 安装依赖

安装整个项目所需的依赖：

```
pnpm install
```

## [​](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup\#2-%E6%9E%84%E5%BB%BA%E5%B9%B6%E6%89%93%E5%8C%85%E5%BA%94%E7%94%A8)  2\. 构建并打包应用

要构建 macOS 应用并将其打包到 `dist/OpenClaw.app`，请运行：

```
./scripts/package-mac-app.sh
```

如果你没有 Apple Developer ID 证书，脚本会自动使用 **临时签名**（`-`）。关于开发运行模式、签名标志以及 Team ID 故障排除，请参阅 macOS 应用 README：
[https://github.com/openclaw/openclaw/blob/main/apps/macos/README.md](https://github.com/openclaw/openclaw/blob/main/apps/macos/README.md)

> **注意**：使用临时签名的应用可能会触发安全提示。如果应用立即崩溃并显示 “Abort trap 6”，请参阅 [故障排除](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup#troubleshooting) 部分。

## [​](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup\#3-%E5%AE%89%E8%A3%85-cli)  3\. 安装 CLI

macOS 应用需要全局安装 `openclaw` CLI 来管理后台任务。**安装方法（推荐）：**

1. 打开 OpenClaw 应用。
2. 前往 **General** 设置标签页。
3. 点击 **“Install CLI”**。

你也可以手动安装：

```
npm install -g openclaw@<version>
```

`pnpm add -g openclaw@<version>` 和 `bun add -g openclaw@<version>` 也可以使用。
对于 Gateway 网关运行时，仍推荐使用 Node。

## [​](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup\#%E6%95%85%E9%9A%9C%E6%8E%92%E9%99%A4)  故障排除

### [​](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup\#%E6%9E%84%E5%BB%BA%E5%A4%B1%E8%B4%A5%EF%BC%9A%E5%B7%A5%E5%85%B7%E9%93%BE%E6%88%96-sdk-%E4%B8%8D%E5%8C%B9%E9%85%8D)  构建失败：工具链或 SDK 不匹配

macOS 应用构建需要最新的 macOS SDK 和 Swift 6.2 工具链。**系统依赖（必需）：**

- **Software Update 中可用的最新 macOS 版本**（Xcode 26.2 SDK 所必需）
- **Xcode 26.2**（Swift 6.2 工具链）

**检查命令：**

```
xcodebuild -version
xcrun swift --version
```

如果版本不匹配，请更新 macOS / Xcode 后重新构建。

### [​](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup\#%E6%8E%88%E4%BA%88%E6%9D%83%E9%99%90%E6%97%B6%E5%BA%94%E7%94%A8%E5%B4%A9%E6%BA%83)  授予权限时应用崩溃

如果你在允许 **语音识别** 或 **麦克风** 访问时应用崩溃，可能是由于损坏的 TCC 缓存或签名不匹配导致。**修复方法：**

1. 重置 TCC 权限：














```
tccutil reset All ai.openclaw.mac.debug
```

2. 如果仍然无效，请临时修改 [`scripts/package-mac-app.sh`](https://github.com/openclaw/openclaw/blob/main/scripts/package-mac-app.sh) 中的 `BUNDLE_ID`，以强制 macOS 生成一个“全新状态”。

### [​](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup\#gateway-%E7%BD%91%E5%85%B3%E4%B8%80%E7%9B%B4%E6%98%BE%E7%A4%BA-%E2%80%9Cstarting%E2%80%A6%E2%80%9D)  Gateway 网关一直显示 “Starting…”

如果 Gateway 网关状态一直停留在 “Starting…”，请检查是否有僵尸进程占用了端口：

```
openclaw gateway status
openclaw gateway stop

# 如果你没有使用 LaunchAgent（开发模式 / 手动运行），请查找监听进程：
lsof -nP -iTCP:18789 -sTCP:LISTEN
```

如果是手动运行的进程占用了端口，请停止该进程（Ctrl+C）。作为最后手段，可以杀掉上面查到的 PID。

## [​](https://docs.openclaw.ai/zh-CN/platforms/mac/dev-setup\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

- [macOS 应用](https://docs.openclaw.ai/zh-CN/platforms/macos)
- [安装概览](https://docs.openclaw.ai/zh-CN/install)

[Raspberry Pi（平台）](https://docs.openclaw.ai/zh-CN/platforms/raspberry-pi) [菜单栏](https://docs.openclaw.ai/zh-CN/platforms/mac/menu-bar)

Ctrl+I