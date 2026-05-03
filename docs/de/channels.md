---
source_url: https://docs.openclaw.ai/de/channels
title: "Chatkan\u00e4le - OpenClaw"
---

[Zum Hauptinhalt springen](https://docs.openclaw.ai/de/channels#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/de)

![DE](https://d3gk2c5xim1je2.cloudfront.net/flags/DE.svg)

Deutsch

Suchen...

Ctrl K

Suchen...

Navigation

Overview

Chatkanäle

[Get started](https://docs.openclaw.ai/de) [Install](https://docs.openclaw.ai/de/install) [Channels](https://docs.openclaw.ai/de/channels) [Agents](https://docs.openclaw.ai/de/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/de/tools) [Models](https://docs.openclaw.ai/de/providers) [Platforms](https://docs.openclaw.ai/de/platforms) [Gateway & Ops](https://docs.openclaw.ai/de/gateway) [Reference](https://docs.openclaw.ai/de/cli) [Help](https://docs.openclaw.ai/de/help)

Auf dieser Seite

- [Hinweise zur Zustellung](https://docs.openclaw.ai/de/channels#hinweise-zur-zustellung)
- [Unterstützte Kanäle](https://docs.openclaw.ai/de/channels#unterst%C3%BCtzte-kan%C3%A4le)
- [Hinweise](https://docs.openclaw.ai/de/channels#hinweise)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw kann in jeder Chat-App mit Ihnen sprechen, die Sie bereits nutzen. Jeder Kanal verbindet sich über den Gateway.
Text wird überall unterstützt; Medien und Reaktionen variieren je nach Kanal.

## [​](https://docs.openclaw.ai/de/channels\#hinweise-zur-zustellung)  Hinweise zur Zustellung

- Telegram-Antworten, die Markdown-Bildsyntax enthalten, wie `![alt](url)`,
werden nach Möglichkeit auf dem finalen ausgehenden Pfad in Medienantworten umgewandelt.
- Slack-Mehrpersonen-DMs werden als Gruppenchats geroutet, daher gelten Gruppenrichtlinien, Erwähnungsverhalten
und Gruppen-Sitzungsregeln für MPIM-Unterhaltungen.
- WhatsApp-Einrichtung erfolgt bei Bedarf: Das Onboarding kann den Einrichtungsablauf anzeigen, bevor
das Plugin-Paket installiert ist, und der Gateway lädt die WhatsApp-Runtime
nur, wenn der Kanal tatsächlich aktiv ist.

## [​](https://docs.openclaw.ai/de/channels\#unterst%C3%BCtzte-kan%C3%A4le)  Unterstützte Kanäle

- [BlueBubbles](https://docs.openclaw.ai/de/channels/bluebubbles) — **Empfohlen für iMessage**; verwendet die REST API des BlueBubbles-macOS-Servers mit vollständiger Funktionsunterstützung (gebündeltes Plugin; Bearbeiten, Zurücknehmen des Sendens, Effekte, Reaktionen, Gruppenverwaltung — Bearbeiten ist derzeit unter macOS 26 Tahoe defekt).
- [Discord](https://docs.openclaw.ai/de/channels/discord) — Discord Bot API + Gateway; unterstützt Server, Kanäle und DMs.
- [Feishu](https://docs.openclaw.ai/de/channels/feishu) — Feishu/Lark-Bot über WebSocket (gebündeltes Plugin).
- [Google Chat](https://docs.openclaw.ai/de/channels/googlechat) — Google Chat API-App über HTTP-Webhook.
- [iMessage (legacy)](https://docs.openclaw.ai/de/channels/imessage) — Legacy-macOS-Integration über imsg CLI (veraltet, verwenden Sie BlueBubbles für neue Einrichtungen).
- [IRC](https://docs.openclaw.ai/de/channels/irc) — Klassische IRC-Server; Kanäle + DMs mit Pairing-/Allowlist-Steuerung.
- [LINE](https://docs.openclaw.ai/de/channels/line) — LINE Messaging API-Bot (gebündeltes Plugin).
- [Matrix](https://docs.openclaw.ai/de/channels/matrix) — Matrix-Protokoll (gebündeltes Plugin).
- [Mattermost](https://docs.openclaw.ai/de/channels/mattermost) — Bot API + WebSocket; Kanäle, Gruppen, DMs (gebündeltes Plugin).
- [Microsoft Teams](https://docs.openclaw.ai/de/channels/msteams) — Bot Framework; Enterprise-Unterstützung (gebündeltes Plugin).
- [Nextcloud Talk](https://docs.openclaw.ai/de/channels/nextcloud-talk) — Selbst gehosteter Chat über Nextcloud Talk (gebündeltes Plugin).
- [Nostr](https://docs.openclaw.ai/de/channels/nostr) — Dezentrale DMs über NIP-04 (gebündeltes Plugin).
- [QQ Bot](https://docs.openclaw.ai/de/channels/qqbot) — QQ Bot API; privater Chat, Gruppenchat und Rich Media (gebündeltes Plugin).
- [Signal](https://docs.openclaw.ai/de/channels/signal) — signal-cli; datenschutzorientiert.
- [Slack](https://docs.openclaw.ai/de/channels/slack) — Bolt SDK; Workspace-Apps.
- [Synology Chat](https://docs.openclaw.ai/de/channels/synology-chat) — Synology NAS Chat über ausgehende+eingehende Webhooks (gebündeltes Plugin).
- [Telegram](https://docs.openclaw.ai/de/channels/telegram) — Bot API über grammY; unterstützt Gruppen.
- [Tlon](https://docs.openclaw.ai/de/channels/tlon) — Urbit-basierter Messenger (gebündeltes Plugin).
- [Twitch](https://docs.openclaw.ai/de/channels/twitch) — Twitch-Chat über IRC-Verbindung (gebündeltes Plugin).
- [Voice Call](https://docs.openclaw.ai/de/plugins/voice-call) — Telefonie über Plivo oder Twilio (Plugin, separat installiert).
- [WebChat](https://docs.openclaw.ai/de/web/webchat) — Gateway WebChat UI über WebSocket.
- [WeChat](https://docs.openclaw.ai/de/channels/wechat) — Tencent iLink Bot-Plugin über QR-Login; nur private Chats (externes Plugin).
- [WhatsApp](https://docs.openclaw.ai/de/channels/whatsapp) — Am beliebtesten; verwendet Baileys und erfordert QR-Pairing.
- [Yuanbao](https://docs.openclaw.ai/de/channels/yuanbao) — Tencent Yuanbao-Bot (externes Plugin).
- [Zalo](https://docs.openclaw.ai/de/channels/zalo) — Zalo Bot API; Vietnams beliebter Messenger (gebündeltes Plugin).
- [Zalo Personal](https://docs.openclaw.ai/de/channels/zalouser) — Persönliches Zalo-Konto über QR-Login (gebündeltes Plugin).

## [​](https://docs.openclaw.ai/de/channels\#hinweise)  Hinweise

- Kanäle können gleichzeitig laufen; konfigurieren Sie mehrere, und OpenClaw routet pro Chat.
- Die schnellste Einrichtung ist in der Regel **Telegram** (einfaches Bot-Token). WhatsApp erfordert QR-Pairing und
speichert mehr Zustand auf der Festplatte.
- Gruppenverhalten variiert je nach Kanal; siehe [Gruppen](https://docs.openclaw.ai/de/channels/groups).
- DM-Pairing und Allowlists werden aus Sicherheitsgründen erzwungen; siehe [Sicherheit](https://docs.openclaw.ai/de/gateway/security).
- Fehlerbehebung: [Kanal-Fehlerbehebung](https://docs.openclaw.ai/de/channels/troubleshooting).
- Model Provider sind separat dokumentiert; siehe [Model Provider](https://docs.openclaw.ai/de/providers/models).

[Discord](https://docs.openclaw.ai/de/channels/discord)

Ctrl+I