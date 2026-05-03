---
source_url: https://docs.openclaw.ai/channels
title: "Chat channels - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/channels#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Overview

Chat channels

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Delivery notes](https://docs.openclaw.ai/channels#delivery-notes)
- [Supported channels](https://docs.openclaw.ai/channels#supported-channels)
- [Notes](https://docs.openclaw.ai/channels#notes)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw can talk to you on any chat app you already use. Each channel connects via the Gateway.
Text is supported everywhere; media and reactions vary by channel.

## [​](https://docs.openclaw.ai/channels\#delivery-notes)  Delivery notes

- Telegram replies that contain markdown image syntax, such as `![alt](url)`,
are converted into media replies on the final outbound path when possible.
- Slack multi-person DMs route as group chats, so group policy, mention
behavior, and group-session rules apply to MPIM conversations.
- WhatsApp setup is install-on-demand: onboarding can show the setup flow before
the plugin package is installed, and the Gateway loads the WhatsApp runtime
only when the channel is actually active.

## [​](https://docs.openclaw.ai/channels\#supported-channels)  Supported channels

- [BlueBubbles](https://docs.openclaw.ai/channels/bluebubbles) — **Recommended for iMessage**; uses the BlueBubbles macOS server REST API with full feature support (bundled plugin; edit, unsend, effects, reactions, group management — edit currently broken on macOS 26 Tahoe).
- [Discord](https://docs.openclaw.ai/channels/discord) — Discord Bot API + Gateway; supports servers, channels, and DMs.
- [Feishu](https://docs.openclaw.ai/channels/feishu) — Feishu/Lark bot via WebSocket (bundled plugin).
- [Google Chat](https://docs.openclaw.ai/channels/googlechat) — Google Chat API app via HTTP webhook (downloadable plugin).
- [iMessage (legacy)](https://docs.openclaw.ai/channels/imessage) — Legacy macOS integration via imsg CLI (deprecated, use BlueBubbles for new setups).
- [IRC](https://docs.openclaw.ai/channels/irc) — Classic IRC servers; channels + DMs with pairing/allowlist controls.
- [LINE](https://docs.openclaw.ai/channels/line) — LINE Messaging API bot (downloadable plugin).
- [Matrix](https://docs.openclaw.ai/channels/matrix) — Matrix protocol (downloadable plugin).
- [Mattermost](https://docs.openclaw.ai/channels/mattermost) — Bot API + WebSocket; channels, groups, DMs (downloadable plugin).
- [Microsoft Teams](https://docs.openclaw.ai/channels/msteams) — Bot Framework; enterprise support (bundled plugin).
- [Nextcloud Talk](https://docs.openclaw.ai/channels/nextcloud-talk) — Self-hosted chat via Nextcloud Talk (bundled plugin).
- [Nostr](https://docs.openclaw.ai/channels/nostr) — Decentralized DMs via NIP-04 (bundled plugin).
- [QQ Bot](https://docs.openclaw.ai/channels/qqbot) — QQ Bot API; private chat, group chat, and rich media (bundled plugin).
- [Signal](https://docs.openclaw.ai/channels/signal) — signal-cli; privacy-focused.
- [Slack](https://docs.openclaw.ai/channels/slack) — Bolt SDK; workspace apps.
- [Synology Chat](https://docs.openclaw.ai/channels/synology-chat) — Synology NAS Chat via outgoing+incoming webhooks (bundled plugin).
- [Telegram](https://docs.openclaw.ai/channels/telegram) — Bot API via grammY; supports groups.
- [Tlon](https://docs.openclaw.ai/channels/tlon) — Urbit-based messenger (bundled plugin).
- [Twitch](https://docs.openclaw.ai/channels/twitch) — Twitch chat via IRC connection (bundled plugin).
- [Voice Call](https://docs.openclaw.ai/plugins/voice-call) — Telephony via Plivo or Twilio (plugin, installed separately).
- [WebChat](https://docs.openclaw.ai/web/webchat) — Gateway WebChat UI over WebSocket.
- [WeChat](https://docs.openclaw.ai/channels/wechat) — Tencent iLink Bot plugin via QR login; private chats only (external plugin).
- [WhatsApp](https://docs.openclaw.ai/channels/whatsapp) — Most popular; uses Baileys and requires QR pairing.
- [Yuanbao](https://docs.openclaw.ai/channels/yuanbao) — Tencent Yuanbao bot (external plugin).
- [Zalo](https://docs.openclaw.ai/channels/zalo) — Zalo Bot API; Vietnam’s popular messenger (bundled plugin).
- [Zalo Personal](https://docs.openclaw.ai/channels/zalouser) — Zalo personal account via QR login (bundled plugin).

## [​](https://docs.openclaw.ai/channels\#notes)  Notes

- Channels can run simultaneously; configure multiple and OpenClaw will route per chat.
- Fastest setup is usually **Telegram** (simple bot token). WhatsApp requires QR pairing and
stores more state on disk.
- Group behavior varies by channel; see [Groups](https://docs.openclaw.ai/channels/groups).
- DM pairing and allowlists are enforced for safety; see [Security](https://docs.openclaw.ai/gateway/security).
- Troubleshooting: [Channel troubleshooting](https://docs.openclaw.ai/channels/troubleshooting).
- Model providers are documented separately; see [Model Providers](https://docs.openclaw.ai/providers/models).

[Discord](https://docs.openclaw.ai/channels/discord)

Ctrl+I