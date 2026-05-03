---
source_url: https://docs.openclaw.ai/zh-CN/platforms/mac/canvas
title: "Canvas - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

macOS 配套应用

Canvas

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [Canvas 的位置](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas#canvas-%E7%9A%84%E4%BD%8D%E7%BD%AE)
- [面板行为](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas#%E9%9D%A2%E6%9D%BF%E8%A1%8C%E4%B8%BA)
- [智能体 API 能力](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas#%E6%99%BA%E8%83%BD%E4%BD%93-api-%E8%83%BD%E5%8A%9B)
- [Canvas 中的 A2UI](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas#canvas-%E4%B8%AD%E7%9A%84-a2ui)
- [A2UI 命令（v0.8）](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas#a2ui-%E5%91%BD%E4%BB%A4%EF%BC%88v0-8%EF%BC%89)
- [从 Canvas 触发智能体运行](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas#%E4%BB%8E-canvas-%E8%A7%A6%E5%8F%91%E6%99%BA%E8%83%BD%E4%BD%93%E8%BF%90%E8%A1%8C)
- [安全说明](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas#%E5%AE%89%E5%85%A8%E8%AF%B4%E6%98%8E)
- [相关内容](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

macOS 应用使用 `WKWebView` 嵌入了一个由智能体控制的 **Canvas 面板**。它是一个轻量级可视化工作区，用于 HTML/CSS/JS、A2UI 和小型交互式 UI 界面。

## [​](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas\#canvas-%E7%9A%84%E4%BD%8D%E7%BD%AE)  Canvas 的位置

Canvas 状态存储在 Application Support 下：

- `~/Library/Application Support/OpenClaw/canvas/<session>/...`

Canvas 面板通过 **自定义 URL scheme** 提供这些文件：

- `openclaw-canvas://<session>/<path>`

示例：

- `openclaw-canvas://main/` → `<canvasRoot>/main/index.html`
- `openclaw-canvas://main/assets/app.css` → `<canvasRoot>/main/assets/app.css`
- `openclaw-canvas://main/widgets/todo/` → `<canvasRoot>/main/widgets/todo/index.html`

如果根目录不存在 `index.html`，应用会显示 **内置脚手架页面**。

## [​](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas\#%E9%9D%A2%E6%9D%BF%E8%A1%8C%E4%B8%BA)  面板行为

- 无边框、可调整大小的面板，固定在菜单栏附近（或鼠标光标附近）。
- 按会话记住大小和位置。
- 本地 Canvas 文件更改时自动重新加载。
- 任意时刻只显示一个 Canvas 面板（会根据需要切换会话）。

可在设置 → **Allow Canvas** 中禁用 Canvas。禁用后，canvas
节点命令会返回 `CANVAS_DISABLED`。

## [​](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas\#%E6%99%BA%E8%83%BD%E4%BD%93-api-%E8%83%BD%E5%8A%9B)  智能体 API 能力

Canvas 通过 **Gateway 网关 WebSocket** 暴露，因此智能体可以：

- 显示/隐藏面板
- 导航到某个路径或 URL
- 执行 JavaScript
- 捕获快照图像

CLI 示例：

```
openclaw nodes canvas present --node <id>
openclaw nodes canvas navigate --node <id> --url "/"
openclaw nodes canvas eval --node <id> --js "document.title"
openclaw nodes canvas snapshot --node <id>
```

说明：

- `canvas.navigate` 接受 **本地 Canvas 路径**、`http(s)` URL 和 `file://` URL。
- 如果传入 `"/"`，Canvas 会显示本地脚手架或 `index.html`。

## [​](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas\#canvas-%E4%B8%AD%E7%9A%84-a2ui)  Canvas 中的 A2UI

A2UI 由 Gateway 网关 canvas host 托管，并在 Canvas 面板内渲染。
当 Gateway 网关发布 Canvas host 时，macOS 应用会在首次打开时自动导航到
A2UI host 页面。默认 A2UI host URL：

```
http://<gateway-host>:18789/__openclaw__/a2ui/
```

### [​](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas\#a2ui-%E5%91%BD%E4%BB%A4%EF%BC%88v0-8%EF%BC%89)  A2UI 命令（v0.8）

Canvas 当前接受 **A2UI v0.8** 的 server→client 消息：

- `beginRendering`
- `surfaceUpdate`
- `dataModelUpdate`
- `deleteSurface`

不支持 `createSurface`（v0.9）。CLI 示例：

```
cat > /tmp/a2ui-v0.8.jsonl <<'EOFA2'
{"surfaceUpdate":{"surfaceId":"main","components":[{"id":"root","component":{"Column":{"children":{"explicitList":["title","content"]}}}},{"id":"title","component":{"Text":{"text":{"literalString":"Canvas (A2UI v0.8)"},"usageHint":"h1"}}},{"id":"content","component":{"Text":{"text":{"literalString":"If you can read this, A2UI push works."},"usageHint":"body"}}}]}}
{"beginRendering":{"surfaceId":"main","root":"root"}}
EOFA2

openclaw nodes canvas a2ui push --jsonl /tmp/a2ui-v0.8.jsonl --node <id>
```

快速冒烟测试：

```
openclaw nodes canvas a2ui push --node <id> --text "Hello from A2UI"
```

## [​](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas\#%E4%BB%8E-canvas-%E8%A7%A6%E5%8F%91%E6%99%BA%E8%83%BD%E4%BD%93%E8%BF%90%E8%A1%8C)  从 Canvas 触发智能体运行

Canvas 可以通过深链接触发新的智能体运行：

- `openclaw://agent?...`

示例（JavaScript 中）：

```
window.location.href = "openclaw://agent?message=Review%20this%20design";
```

除非提供了有效 key，否则应用会提示确认。

## [​](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas\#%E5%AE%89%E5%85%A8%E8%AF%B4%E6%98%8E)  安全说明

- Canvas scheme 会阻止目录遍历；文件必须位于会话根目录下。
- 本地 Canvas 内容使用自定义 scheme（无需 local loopback 服务器）。
- 外部 `http(s)` URL 仅在显式导航时才允许。

## [​](https://docs.openclaw.ai/zh-CN/platforms/mac/canvas\#%E7%9B%B8%E5%85%B3%E5%86%85%E5%AE%B9)  相关内容

- [macOS 应用](https://docs.openclaw.ai/zh-CN/platforms/macos)
- [WebChat](https://docs.openclaw.ai/zh-CN/web/webchat)

[WebChat（macOS）](https://docs.openclaw.ai/zh-CN/platforms/mac/webchat) [Gateway 网关生命周期](https://docs.openclaw.ai/zh-CN/platforms/mac/child-process)

Ctrl+I