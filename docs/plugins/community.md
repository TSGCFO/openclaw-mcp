---
source_url: https://docs.openclaw.ai/plugins/community
title: "Community plugins - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/plugins/community#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Plugins

Community plugins

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Listed plugins](https://docs.openclaw.ai/plugins/community#listed-plugins)
- [Apify](https://docs.openclaw.ai/plugins/community#apify)
- [Codex App Server Bridge](https://docs.openclaw.ai/plugins/community#codex-app-server-bridge)
- [DingTalk](https://docs.openclaw.ai/plugins/community#dingtalk)
- [Lossless Claw (LCM)](https://docs.openclaw.ai/plugins/community#lossless-claw-lcm)
- [Opik](https://docs.openclaw.ai/plugins/community#opik)
- [Prometheus Avatar](https://docs.openclaw.ai/plugins/community#prometheus-avatar)
- [QQbot](https://docs.openclaw.ai/plugins/community#qqbot)
- [wecom](https://docs.openclaw.ai/plugins/community#wecom)
- [Yuanbao](https://docs.openclaw.ai/plugins/community#yuanbao)
- [Submit your plugin](https://docs.openclaw.ai/plugins/community#submit-your-plugin)
- [Quality bar](https://docs.openclaw.ai/plugins/community#quality-bar)
- [Related](https://docs.openclaw.ai/plugins/community#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Community plugins are third-party packages that extend OpenClaw with new
channels, tools, providers, or other capabilities. They are built and maintained
by the community, usually published on [ClawHub](https://docs.openclaw.ai/tools/clawhub), and installable
with a single command. Npm remains the launch default for bare package specs
while ClawHub pack installs roll out.ClawHub is the canonical discovery surface for community plugins. Do not open
docs-only PRs just to add your plugin here for discoverability; publish it on
ClawHub instead.

```
openclaw plugins install clawhub:<package-name>
```

Use `openclaw plugins install <package-name>` for npm-hosted packages.

## [​](https://docs.openclaw.ai/plugins/community\#listed-plugins)  Listed plugins

### [​](https://docs.openclaw.ai/plugins/community\#apify)  Apify

Scrape data from any website with 20,000+ ready-made scrapers. Let your agent
extract data from Instagram, Facebook, TikTok, YouTube, Google Maps, Google
Search, e-commerce sites, and more — just by asking.

- **npm:**`@apify/apify-openclaw-plugin`
- **repo:** [github.com/apify/apify-openclaw-plugin](https://github.com/apify/apify-openclaw-plugin)

```
openclaw plugins install @apify/apify-openclaw-plugin
```

### [​](https://docs.openclaw.ai/plugins/community\#codex-app-server-bridge)  Codex App Server Bridge

Independent OpenClaw bridge for Codex App Server conversations. Bind a chat to
a Codex thread, talk to it with plain text, and control it with chat-native
commands for resume, planning, review, model selection, compaction, and more.

- **npm:**`openclaw-codex-app-server`
- **repo:** [github.com/pwrdrvr/openclaw-codex-app-server](https://github.com/pwrdrvr/openclaw-codex-app-server)

```
openclaw plugins install openclaw-codex-app-server
```

### [​](https://docs.openclaw.ai/plugins/community\#dingtalk)  DingTalk

Enterprise robot integration using Stream mode. Supports text, images, and
file messages via any DingTalk client.

- **npm:**`@largezhou/ddingtalk`
- **repo:** [github.com/largezhou/openclaw-dingtalk](https://github.com/largezhou/openclaw-dingtalk)

```
openclaw plugins install @largezhou/ddingtalk
```

### [​](https://docs.openclaw.ai/plugins/community\#lossless-claw-lcm)  Lossless Claw (LCM)

Lossless Context Management plugin for OpenClaw. DAG-based conversation
summarization with incremental compaction — preserves full context fidelity
while reducing token usage.

- **npm:**`@martian-engineering/lossless-claw`
- **repo:** [github.com/Martian-Engineering/lossless-claw](https://github.com/Martian-Engineering/lossless-claw)

```
openclaw plugins install @martian-engineering/lossless-claw
```

### [​](https://docs.openclaw.ai/plugins/community\#opik)  Opik

Official plugin that exports agent traces to Opik. Monitor agent behavior,
cost, tokens, errors, and more.

- **npm:**`@opik/opik-openclaw`
- **repo:** [github.com/comet-ml/opik-openclaw](https://github.com/comet-ml/opik-openclaw)

```
openclaw plugins install @opik/opik-openclaw
```

### [​](https://docs.openclaw.ai/plugins/community\#prometheus-avatar)  Prometheus Avatar

Give your OpenClaw agent a Live2D avatar with real-time lip-sync, emotion
expressions, and text-to-speech. Includes creator tools for AI asset generation
and one-click deployment to the Prometheus Marketplace. Currently in alpha.

- **npm:**`@prometheusavatar/openclaw-plugin`
- **repo:** [github.com/myths-labs/prometheus-avatar](https://github.com/myths-labs/prometheus-avatar)

```
openclaw plugins install @prometheusavatar/openclaw-plugin
```

### [​](https://docs.openclaw.ai/plugins/community\#qqbot)  QQbot

Connect OpenClaw to QQ via the QQ Bot API. Supports private chats, group
mentions, channel messages, and rich media including voice, images, videos,
and files.Current OpenClaw releases bundle QQ Bot. Use the bundled setup in
[QQ Bot](https://docs.openclaw.ai/channels/qqbot) for normal installs; install this external plugin only
when you intentionally want the Tencent-maintained standalone package.

- **npm:**`@tencent-connect/openclaw-qqbot`
- **repo:** [github.com/tencent-connect/openclaw-qqbot](https://github.com/tencent-connect/openclaw-qqbot)

```
openclaw plugins install @tencent-connect/openclaw-qqbot
```

### [​](https://docs.openclaw.ai/plugins/community\#wecom)  wecom

WeCom channel plugin for OpenClaw by the Tencent WeCom team. Powered by
WeCom Bot WebSocket persistent connections, it supports direct messages & group
chats, streaming replies, proactive messaging, image/file processing, Markdown
formatting, built-in access control, and document/meeting/messaging skills.

- **npm:**`@wecom/wecom-openclaw-plugin`
- **repo:** [github.com/WecomTeam/wecom-openclaw-plugin](https://github.com/WecomTeam/wecom-openclaw-plugin)

```
openclaw plugins install @wecom/wecom-openclaw-plugin
```

### [​](https://docs.openclaw.ai/plugins/community\#yuanbao)  Yuanbao

Yuanbao channel plugin for OpenClaw by the Tencent Yuanbao team. Powered by
WebSocket persistent connections, it supports direct messages & group chats,
streaming replies, proactive messaging, image/file/audio/video processing,
Markdown formatting, built-in access control, and slash-command menus.

- **npm:**`openclaw-plugin-yuanbao`
- **repo:** [github.com/YuanbaoTeam/yuanbao-openclaw-plugin](https://github.com/YuanbaoTeam/yuanbao-openclaw-plugin)

```
openclaw plugins install openclaw-plugin-yuanbao
```

## [​](https://docs.openclaw.ai/plugins/community\#submit-your-plugin)  Submit your plugin

We welcome community plugins that are useful, documented, and safe to operate.

1

[Navigate to header](https://docs.openclaw.ai/plugins/community#)

Publish to ClawHub or npm

Your plugin must be installable via `openclaw plugins install \<package-name\>`.
Publish to [ClawHub](https://docs.openclaw.ai/tools/clawhub) unless you specifically need npm-only
distribution.
See [Building Plugins](https://docs.openclaw.ai/plugins/building-plugins) for the full guide.

2

[Navigate to header](https://docs.openclaw.ai/plugins/community#)

Host on GitHub

Source code must be in a public repository with setup docs and an issue
tracker.

3

[Navigate to header](https://docs.openclaw.ai/plugins/community#)

Use docs PRs only for source-doc changes

You do not need a docs PR just to make your plugin discoverable. Publish it
on ClawHub instead.Open a docs PR only when OpenClaw’s source docs need an actual content
change, such as correcting install guidance or adding cross-repo
documentation that belongs in the main docs set.

## [​](https://docs.openclaw.ai/plugins/community\#quality-bar)  Quality bar

| Requirement | Why |
| --- | --- |
| Published on ClawHub or npm | Users need `openclaw plugins install` to work |
| Public GitHub repo | Source review, issue tracking, transparency |
| Setup and usage docs | Users need to know how to configure it |
| Active maintenance | Recent updates or responsive issue handling |

Low-effort wrappers, unclear ownership, or unmaintained packages may be declined.

## [​](https://docs.openclaw.ai/plugins/community\#related)  Related

- [Install and Configure Plugins](https://docs.openclaw.ai/tools/plugin) — how to install any plugin
- [Building Plugins](https://docs.openclaw.ai/plugins/building-plugins) — create your own
- [Plugin Manifest](https://docs.openclaw.ai/plugins/manifest) — manifest schema

[Manage plugins](https://docs.openclaw.ai/plugins/manage-plugins) [Plugin inventory](https://docs.openclaw.ai/plugins/plugin-inventory)

Ctrl+I