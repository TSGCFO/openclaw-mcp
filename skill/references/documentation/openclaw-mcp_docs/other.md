# Openclaw-Mcp_Docs - Other

**Pages:** 14

---

## OpenClaw

**URL:** https://docs.openclaw.ai/es

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- Comenzar
- Ejecutar incorporación
- Abrir la IU de control
- ​¿Qué es OpenClaw?
- ​Cómo funciona
- ​Capacidades clave
- Gateway multicanal

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Gateway para agentes de IA en cualquier sistema operativo a través de Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo y más. Envía un mensaje y recibe una respuesta de un agente desde tu bolsillo. Ejecuta un Gateway en canales integrados, plugins de canal incluidos, WebChat y nodos móviles.

Realizar la incorporación e instalar el servicio

**Examples:**

Example 1 (elixir):
```elixir
npm install -g openclaw@latest
```

Example 2 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 3 (unknown):
```unknown
openclaw dashboard
```

Example 4 (sass):
```sass
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

---

## Date and time

**URL:** https://docs.openclaw.ai/date-time

**Contents:**
- Date and time
- Documentation Index
- ​Date & Time
- ​Message envelopes (local by default)
  - ​Examples
- ​System prompt: Current Date & Time
- ​System event lines (local by default)
  - ​Configure user timezone + format
- ​Time format detection (auto)
- ​Tool payloads + connectors (raw provider time + normalized fields)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
[Provider ... 2026-01-05 16:26 PST] message text
```

Example 2 (json):
```json
{
  agents: {
    defaults: {
      envelopeTimezone: "local", // "utc" | "local" | "user" | IANA timezone
      envelopeTimestamp: "on", // "on" | "off"
      envelopeElapsed: "on", // "on" | "off"
    },
  },
}
```

Example 3 (sass):
```sass
[WhatsApp +1555 2026-01-18 00:19 PST] hello
```

Example 4 (sass):
```sass
[WhatsApp +1555 2026-01-18 00:19 CST] hello
```

---

## OpenClaw

**URL:** https://docs.openclaw.ai/de

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- Erste Schritte
- Onboarding ausführen
- Die Control UI öffnen
- ​Was ist OpenClaw?
- ​So funktioniert es
- ​Hauptfunktionen
- Multi-Channel-Gateway

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Gateway für KI-Agenten auf jedem Betriebssystem über Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo und mehr. Senden Sie eine Nachricht und erhalten Sie eine Agentenantwort direkt in Ihre Tasche. Betreiben Sie ein einziges Gateway über integrierte Channels, gebündelte Channel-Plugins, WebChat und mobile Nodes.

OpenClaw installieren

Onboarding durchführen und den Dienst installieren

**Examples:**

Example 1 (elixir):
```elixir
npm install -g openclaw@latest
```

Example 2 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 3 (unknown):
```unknown
openclaw dashboard
```

Example 4 (sass):
```sass
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

---

## OpenClaw

**URL:** https://docs.openclaw.ai/ja-JP

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- はじめに
- オンボーディングを実行
- Control UIを開く
- ​OpenClawとは？
- ​仕組み
- ​主な機能
- マルチチャネルGateway

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Discord、Google Chat、iMessage、Matrix、Microsoft Teams、Signal、Slack、Telegram、WhatsApp、ZaloなどにまたがるAIエージェント向けの、あらゆるOSで動作するGatewayです。 メッセージを送ると、ポケットからエージェントの応答を受け取れます。組み込みチャネル、バンドルされたチャネルPlugin、WebChat、モバイルNodeをまたいで、1つのGatewayを実行できます。

オンボーディングを実行してサービスをインストール

**Examples:**

Example 1 (elixir):
```elixir
npm install -g openclaw@latest
```

Example 2 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 3 (unknown):
```unknown
openclaw dashboard
```

Example 4 (sass):
```sass
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

---

## OpenClaw

**URL:** https://docs.openclaw.ai/ko

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- 시작하기
- 온보딩 실행
- Control UI 열기
- ​OpenClaw이란?
- ​작동 방식
- ​주요 기능
- 멀티채널 Gateway

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo 등 전반에서 AI 에이전트를 위한 모든 OS용 Gateway. 메시지를 보내면 주머니 속에서 에이전트 응답을 받을 수 있습니다. 내장 채널, 번들 채널 Plugin, WebChat, 모바일 Node 전반에 걸쳐 하나의 Gateway를 실행하세요.

**Examples:**

Example 1 (elixir):
```elixir
npm install -g openclaw@latest
```

Example 2 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 3 (unknown):
```unknown
openclaw dashboard
```

Example 4 (sass):
```sass
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

---

## OpenClaw

**URL:** https://docs.openclaw.ai/id

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- Get Started
- Run Onboarding
- Open the Control UI
- ​Apa itu OpenClaw?
- ​Cara kerjanya
- ​Kemampuan utama
- Multi-channel gateway

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Gateway lintas sistem operasi untuk agen AI di Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, dan lainnya. Kirim pesan, dapatkan respons agen dari saku Anda. Jalankan satu Gateway di berbagai channel bawaan, plugin channel bawaan, WebChat, dan Node seluler.

Onboard and install the service

**Examples:**

Example 1 (elixir):
```elixir
npm install -g openclaw@latest
```

Example 2 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 3 (unknown):
```unknown
openclaw dashboard
```

Example 4 (sass):
```sass
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

---

## OpenClaw

**URL:** https://docs.openclaw.ai/it

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- Per iniziare
- Esegui l'onboarding
- Apri la UI di controllo
- ​Che cos’è OpenClaw?
- ​Come funziona
- ​Capacità principali
- Gateway multicanale

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Gateway per qualsiasi sistema operativo per agenti AI su Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo e altro ancora. Invia un messaggio, ricevi una risposta da un agente dal tuo taschino. Esegui un Gateway su canali integrati, plugin di canale inclusi, WebChat e Node mobili.

Esegui onboarding e installa il servizio

**Examples:**

Example 1 (elixir):
```elixir
npm install -g openclaw@latest
```

Example 2 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 3 (unknown):
```unknown
openclaw dashboard
```

Example 4 (sass):
```sass
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

---

## OpenClaw

**URL:** https://docs.openclaw.ai/zh-TW

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- 開始使用
- 執行新手導引
- 開啟控制 UI
- ​什麼是 OpenClaw？
- ​運作方式
- ​主要功能
- 多頻道 Gateway

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

適用於任何作業系統的 AI agent Gateway，橫跨 Discord、Google Chat、iMessage、Matrix、Microsoft Teams、Signal、Slack、Telegram、WhatsApp、Zalo 等平台。 傳送訊息，即可從口袋裡取得 agent 回應。透過一個 Gateway，跨內建頻道、隨附頻道 Plugin、WebChat 和行動節點執行。

**Examples:**

Example 1 (elixir):
```elixir
npm install -g openclaw@latest
```

Example 2 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 3 (unknown):
```unknown
openclaw dashboard
```

Example 4 (sass):
```sass
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

---

## OpenClaw

**URL:** https://docs.openclaw.ai/pl

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- Pierwsze kroki
- Uruchom onboarding
- Otwórz interfejs sterowania
- ​Czym jest OpenClaw?
- ​Jak to działa
- ​Kluczowe możliwości
- Wielokanałowa Gateway

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Brama dla agentów AI działająca na każdym systemie operacyjnym w Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo i innych kanałach. Wyślij wiadomość i otrzymaj odpowiedź agenta z kieszeni. Uruchom jedną Gateway dla wbudowanych kanałów, dołączonych pluginów kanałów, WebChat i mobilnych węzłów.

Przejdź onboarding i zainstaluj usługę

**Examples:**

Example 1 (elixir):
```elixir
npm install -g openclaw@latest
```

Example 2 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 3 (unknown):
```unknown
openclaw dashboard
```

Example 4 (sass):
```sass
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

---

## OpenProse

**URL:** https://docs.openclaw.ai/prose

**Contents:**
- OpenProse
- Documentation Index
- ​What it can do
- ​Install + enable
- ​Slash command
- ​Example: a simple .prose file
- ​File locations
- ​State modes
- ​Remote programs
- ​OpenClaw runtime mapping

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw plugins enable open-prose
```

Example 2 (sql):
```sql
/prose help
/prose run <file.prose>
/prose run <handle/slug>
/prose run <https://example.com/file.prose>
/prose compile <file.prose>
/prose examples
/prose update
```

Example 3 (yaml):
```yaml
# Research + synthesis with two agents running in parallel.

input topic: "What should we research?"

agent researcher:
  model: sonnet
  prompt: "You research thoroughly and cite sources."

agent writer:
  model: opus
  prompt: "You write a concise summary."

parallel:
  findings = session: researcher
    prompt: "Research {topic}."
  draft = session: writer
    prompt: "Summarize {topic}."

session "Merge the findings + draft into a final answer."
context: { findings, draft }
```

Example 4 (unknown):
```unknown
.prose/
├── .env
├── runs/
│   └── {YYYYMMDD}-{HHMMSS}-{random}/
│       ├── program.prose
│       ├── state.md
│       ├── bindings/
│       └── agents/
└── agents/
```

---

## OpenClaw

**URL:** https://docs.openclaw.ai/vi

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- Bắt đầu
- Chạy quy trình nhập môn
- Mở giao diện điều khiển
- ​OpenClaw là gì?
- ​Cách hoạt động
- ​Khả năng chính
- Gateway đa kênh

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Gateway cho mọi hệ điều hành, dành cho các tác tử AI trên Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, v.v. Gửi một tin nhắn, nhận phản hồi từ tác tử ngay trong túi bạn. Chạy một Gateway cho các kênh tích hợp sẵn, Plugin kênh đi kèm, WebChat và các nút di động.

Nhập môn và cài đặt dịch vụ

**Examples:**

Example 1 (elixir):
```elixir
npm install -g openclaw@latest
```

Example 2 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 3 (unknown):
```unknown
openclaw dashboard
```

Example 4 (sass):
```sass
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

---

## OpenClaw

**URL:** https://docs.openclaw.ai/tr

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- Başlayın
- Onboarding'i Çalıştırın
- Control UI'ı Açın
- ​OpenClaw nedir?
- ​Nasıl çalışır
- ​Temel yetenekler
- Çok kanallı gateway

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo ve daha fazlasında AI ajanları için her işletim sisteminde çalışan Gateway. Bir mesaj gönderin, cebinizden bir ajan yanıtı alın. Yerleşik kanallar, paketle gelen kanal plugin’leri, WebChat ve mobil Node’lar üzerinden tek bir Gateway çalıştırın.

Onboarding'i çalıştırın ve hizmeti kurun

**Examples:**

Example 1 (elixir):
```elixir
npm install -g openclaw@latest
```

Example 2 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 3 (unknown):
```unknown
openclaw dashboard
```

Example 4 (sass):
```sass
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

---

## Network

**URL:** https://docs.openclaw.ai/network

**Contents:**
- Network
- Documentation Index
- ​Network hub
- ​Core model
- ​Pairing + identity
- ​Discovery + transports
- ​Nodes + transports
- ​Security
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## OpenClaw

**URL:** https://docs.openclaw.ai/zh-CN

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- 入门指南
- 运行新手引导
- 打开控制 UI
- ​OpenClaw 是什么？
- ​工作原理
- ​关键能力
- 多渠道 Gateway 网关

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

适用于 AI 智能体的任意操作系统 Gateway 网关，覆盖 Discord、Google Chat、iMessage、Matrix、Microsoft Teams、Signal、Slack、Telegram、WhatsApp、Zalo 等渠道。 发一条消息，就能随时随地收到智能体回复。通过一个 Gateway 网关即可运行内置渠道、内置渠道插件、WebChat 和移动节点。

**Examples:**

Example 1 (elixir):
```elixir
npm install -g openclaw@latest
```

Example 2 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 3 (unknown):
```unknown
openclaw dashboard
```

Example 4 (sass):
```sass
{
  channels: {
    whatsapp: {
      allowFrom: ["+15555550123"],
      groups: { "*": { requireMention: true } },
    },
  },
  messages: { groupChat: { mentionPatterns: ["@openclaw"] } },
}
```

---
