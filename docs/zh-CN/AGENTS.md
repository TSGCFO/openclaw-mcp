---
source_url: https://docs.openclaw.ai/zh-CN/AGENTS
title: "AGENTS - OpenClaw"
---

[跳转到主要内容](https://docs.openclaw.ai/zh-CN/AGENTS#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/zh-CN)

![CN](https://d3gk2c5xim1je2.cloudfront.net/flags/CN.svg)

简体中文

搜索...

Ctrl K

搜索...

Navigation

[快速开始](https://docs.openclaw.ai/zh-CN) [安装](https://docs.openclaw.ai/zh-CN/install) [消息渠道](https://docs.openclaw.ai/zh-CN/channels) [代理](https://docs.openclaw.ai/zh-CN/pi) [工具](https://docs.openclaw.ai/zh-CN/tools) [模型](https://docs.openclaw.ai/zh-CN/providers) [平台](https://docs.openclaw.ai/zh-CN/platforms) [网关与运维](https://docs.openclaw.ai/zh-CN/gateway) [参考](https://docs.openclaw.ai/zh-CN/cli) [帮助](https://docs.openclaw.ai/zh-CN/help)

在此页面

- [文档指南](https://docs.openclaw.ai/zh-CN/AGENTS#%E6%96%87%E6%A1%A3%E6%8C%87%E5%8D%97)
- [Mintlify 规则](https://docs.openclaw.ai/zh-CN/AGENTS#mintlify-%E8%A7%84%E5%88%99)
- [文档内容规则](https://docs.openclaw.ai/zh-CN/AGENTS#%E6%96%87%E6%A1%A3%E5%86%85%E5%AE%B9%E8%A7%84%E5%88%99)
- [文档 i18n](https://docs.openclaw.ai/zh-CN/AGENTS#%E6%96%87%E6%A1%A3-i18n)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

# [​](https://docs.openclaw.ai/zh-CN/AGENTS\#%E6%96%87%E6%A1%A3%E6%8C%87%E5%8D%97)  文档指南

此目录负责文档编写、Mintlify 链接规则以及文档 i18n 策略。

## [​](https://docs.openclaw.ai/zh-CN/AGENTS\#mintlify-%E8%A7%84%E5%88%99)  Mintlify 规则

- 文档托管在 Mintlify（`https://docs.openclaw.ai`）上。
- `docs/**/*.md` 中的内部文档链接必须保持为根相对路径，且不带 `.md` 或 `.mdx` 后缀（示例：`[配置](/gateway/configuration)`）。
- 章节交叉引用应在根相对路径上使用锚点（示例：`[Hooks](/gateway/configuration-reference#hooks)`）。
- 文档标题应避免使用长破折号和撇号，因为 Mintlify 的锚点生成在这些情况下不稳定。
- README 和其他在 GitHub 上渲染的文档应保留绝对文档 URL，以确保链接在 Mintlify 之外也能正常工作。
- 文档内容必须保持通用：不要使用个人设备名称、主机名或本地路径；请使用类似 `user@gateway-host` 的占位符。

## [​](https://docs.openclaw.ai/zh-CN/AGENTS\#%E6%96%87%E6%A1%A3%E5%86%85%E5%AE%B9%E8%A7%84%E5%88%99)  文档内容规则

- 对于文档、UI 文案和选择器列表，服务/提供商应按字母顺序排列，除非该部分明确是在描述运行时顺序或自动检测顺序。
- 保持内置 plugin 的命名与根 `AGENTS.md` 中的全仓库 plugin 术语规则一致。

## [​](https://docs.openclaw.ai/zh-CN/AGENTS\#%E6%96%87%E6%A1%A3-i18n)  文档 i18n

- 此仓库不维护外语文档。生成后的发布输出位于单独的 `openclaw/docs` 仓库中（本地通常克隆为 `../openclaw-docs`）。
- 不要在此仓库中的 `docs/<locale>/**` 下添加或编辑本地化文档。
- 将此仓库中的英文文档以及术语表文件视为唯一事实来源。
- 流程：先在此处更新英文文档，再按需要更新 `docs/.i18n/glossary.<locale>.json`，然后让发布仓库同步，并在 `openclaw/docs` 中运行 `scripts/docs-i18n`。
- 在重新运行 `scripts/docs-i18n` 之前，为任何必须保留英文或需要固定译法的新技术术语、页面标题或简短导航标签添加术语表条目。
- `pnpm docs:check-i18n-glossary` 是用于检查已变更英文文档标题和简短内部文档标签的守卫命令。
- 翻译记忆库存储在发布仓库中生成的 `docs/.i18n/*.tm.jsonl` 文件里。
- 参见 `docs/.i18n/README.md`。

[文档目录](https://docs.openclaw.ai/zh-CN/start/docs-directory)

Ctrl+I