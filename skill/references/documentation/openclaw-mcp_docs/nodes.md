# Openclaw-Mcp_Docs - Nodes

**Pages:** 12

---

## Talk mode

**URL:** https://docs.openclaw.ai/nodes/talk

**Contents:**
- Talk mode
- Documentation Index
- ​Behavior (macOS)
- ​Voice directives in replies
- ​Config (~/.openclaw/openclaw.json)
- ​macOS UI
- ​Android UI
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{ "voice": "<voice-id>", "once": true }
```

Example 2 (json):
```json
{
  talk: {
    provider: "elevenlabs",
    providers: {
      elevenlabs: {
        voiceId: "elevenlabs_voice_id",
        modelId: "eleven_v3",
        outputFormat: "mp3_44100_128",
        apiKey: "elevenlabs_api_key",
      },
      mlx: {
        modelId: "mlx-community/Soprano-80M-bf16",
      },
      system: {},
    },
    speechLocale: "ru-RU",
    silenceTimeoutMs: 1500,
    interruptOnSpeech: true,
  },
}
```

---

## Nodes

**URL:** https://docs.openclaw.ai/nodes

**Contents:**
- Nodes
- Documentation Index
- ​Pairing + status
- ​Remote node host (system.run)
  - ​What runs where
  - ​Start a node host (foreground)
  - ​Remote gateway via SSH tunnel (loopback bind)
  - ​Start a node host (service)
  - ​Pair + name
  - ​Allowlist the commands

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
openclaw devices list
openclaw devices approve <requestId>
openclaw devices reject <requestId>
openclaw nodes status
openclaw nodes describe --node <idOrNameOrIp>
```

Example 2 (unknown):
```unknown
openclaw node run --host <gateway-host> --port 18789 --display-name "Build Node"
```

Example 3 (elixir):
```elixir
# Terminal A (keep running): forward local 18790 -> gateway 127.0.0.1:18789
ssh -N -L 18790:127.0.0.1:18789 user@gateway-host

# Terminal B: export the gateway token and connect through the tunnel
export OPENCLAW_GATEWAY_TOKEN="<gateway-token>"
openclaw node run --host 127.0.0.1 --port 18790 --display-name "Build Node"
```

Example 4 (unknown):
```unknown
openclaw node install --host <gateway-host> --port 18789 --display-name "Build Node"
openclaw node start
openclaw node restart
```

---

## Audio and voice notes

**URL:** https://docs.openclaw.ai/nodes/audio

**Contents:**
- Audio and voice notes
- Documentation Index
- ​Audio / Voice Notes (2026-01-17)
- ​What works
- ​Auto-detection (default)
- ​Config examples
  - ​Provider + CLI fallback (OpenAI + Whisper CLI)
  - ​Provider-only with scope gating
  - ​Provider-only (Deepgram)
  - ​Provider-only (Mistral Voxtral)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  tools: {
    media: {
      audio: {
        enabled: true,
        maxBytes: 20971520,
        models: [
          { provider: "openai", model: "gpt-4o-mini-transcribe" },
          {
            type: "cli",
            command: "whisper",
            args: ["--model", "base", "{{MediaPath}}"],
            timeoutSeconds: 45,
          },
        ],
      },
    },
  },
}
```

Example 2 (json):
```json
{
  tools: {
    media: {
      audio: {
        enabled: true,
        scope: {
          default: "allow",
          rules: [{ action: "deny", match: { chatType: "group" } }],
        },
        models: [{ provider: "openai", model: "gpt-4o-mini-transcribe" }],
      },
    },
  },
}
```

Example 3 (json):
```json
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "deepgram", model: "nova-3" }],
      },
    },
  },
}
```

Example 4 (json):
```json
{
  tools: {
    media: {
      audio: {
        enabled: true,
        models: [{ provider: "mistral", model: "voxtral-mini-latest" }],
      },
    },
  },
}
```

---

## OpenClaw

**URL:** https://docs.openclaw.ai/ar

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- ابدأ
- شغّل الإعداد الأولي
- افتح واجهة التحكم
- ​ما هو OpenClaw؟
- ​كيف يعمل
- ​الإمكانات الأساسية
- Gateway متعدد القنوات

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Gateway لأي نظام تشغيل لوكلاء الذكاء الاصطناعي عبر Discord وGoogle Chat وiMessage وMatrix وMicrosoft Teams وSignal وSlack وTelegram وWhatsApp وZalo والمزيد. أرسل رسالة، واحصل على استجابة من وكيل من جيبك. شغّل Gateway واحدًا عبر القنوات المضمنة وPlugin القنوات المجمعة وWebChat وNodes الأجهزة المحمولة.

نفّذ الإعداد الأولي وثبّت الخدمة

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

## Node + tsx crash

**URL:** https://docs.openclaw.ai/debug/node-issue

**Contents:**
- Node + tsx crash
- Documentation Index
- ​Node + tsx “__name is not a function” crash
- ​Summary
- ​Environment
- ​Repro (Node-only)
- ​Minimal repro in repo
- ​Node version check
- ​Notes / hypothesis
- ​Regression history

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (lua):
```lua
[openclaw] Failed to start CLI: TypeError: __name is not a function
    at createSubsystemLogger (.../src/logging/subsystem.ts:203:25)
    at .../src/agents/auth-profiles/constants.ts:25:20
```

Example 2 (markdown):
```markdown
# in repo root
node --version
pnpm install
node --import tsx src/entry.ts status
```

Example 3 (python):
```python
node --import tsx scripts/repro/tsx-name-repro.ts
```

Example 4 (unknown):
```unknown
pnpm tsgo
node openclaw.mjs status
```

---

## Image and media support

**URL:** https://docs.openclaw.ai/nodes/images

**Contents:**
- Image and media support
- Documentation Index
- ​Image & Media Support (2025-12-05)
- ​Goals
- ​CLI Surface
- ​WhatsApp Web channel behavior
- ​Auto-Reply Pipeline
- ​Inbound media to commands (Pi)
- ​Limits & Errors
- ​Notes for Tests

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## OpenClaw

**URL:** https://docs.openclaw.ai/uk

**Contents:**
- OpenClaw
- Documentation Index
- ​OpenClaw 🦞
- Почати
- Запустити онбординг
- Відкрити Control UI
- ​Що таке OpenClaw?
- ​Як це працює
- ​Ключові можливості
- Багатоканальний Gateway

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Gateway для AI-агентів на будь-якій ОС у Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo тощо. Надішліть повідомлення й отримайте відповідь агента просто зі своєї кишені. Запустіть один Gateway для вбудованих каналів, комплектних channel plugins, WebChat і mobile nodes.

Пройдіть онбординг і встановіть службу

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

## Voice wake

**URL:** https://docs.openclaw.ai/nodes/voicewake

**Contents:**
- Voice wake
- Documentation Index
- ​Storage (Gateway host)
- ​Protocol
  - ​Methods
  - ​Routing methods (trigger → target)
  - ​Events
- ​Client behavior
  - ​macOS app
  - ​iOS node

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{ "triggers": ["openclaw", "claude", "computer"], "updatedAtMs": 1730000000000 }
```

Example 2 (json):
```json
{
  "version": 1,
  "defaultTarget": { "mode": "current" },
  "routes": [{ "trigger": "robot wake", "target": { "sessionKey": "agent:main:main" } }],
  "updatedAtMs": 1730000000000
}
```

---

## Node troubleshooting

**URL:** https://docs.openclaw.ai/nodes/troubleshooting

**Contents:**
- Node troubleshooting
- Documentation Index
- ​Command ladder
- ​Foreground requirements
- ​Permissions matrix
- ​Pairing versus approvals
- ​Common node error codes
- ​Fast recovery loop
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw status
openclaw gateway status
openclaw logs --follow
openclaw doctor
openclaw channels status --probe
```

Example 2 (typescript):
```typescript
openclaw nodes status
openclaw nodes describe --node <idOrNameOrIp>
openclaw approvals get --node <idOrNameOrIp>
```

Example 3 (typescript):
```typescript
openclaw nodes describe --node <idOrNameOrIp>
openclaw nodes canvas snapshot --node <idOrNameOrIp>
openclaw logs --follow
```

Example 4 (typescript):
```typescript
openclaw devices list
openclaw nodes status
openclaw approvals get --node <idOrNameOrIp>
openclaw approvals allowlist add --node <idOrNameOrIp> "/usr/bin/uname"
```

---

## Camera capture

**URL:** https://docs.openclaw.ai/nodes/camera

**Contents:**
- Camera capture
- Documentation Index
- ​iOS node
  - ​User setting (default on)
  - ​Commands (via Gateway node.invoke)
  - ​Foreground requirement
  - ​CLI helper (temp files + MEDIA)
- ​Android node
  - ​Android user setting (default on)
  - ​Permissions

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
openclaw nodes camera snap --node <id>               # default: both front + back (2 MEDIA lines)
openclaw nodes camera snap --node <id> --facing front
openclaw nodes camera clip --node <id> --duration 3000
openclaw nodes camera clip --node <id> --no-audio
```

Example 2 (typescript):
```typescript
openclaw nodes camera list --node <id>            # list camera ids
openclaw nodes camera snap --node <id>            # prints MEDIA:<path>
openclaw nodes camera snap --node <id> --max-width 1280
openclaw nodes camera snap --node <id> --delay-ms 2000
openclaw nodes camera snap --node <id> --device-id <id>
openclaw nodes camera clip --node <id> --duration 10s          # prints MEDIA:<path>
openclaw nodes camera clip --node <id> --duration-ms 3000      # prints MEDIA:<path> (legacy flag)
openclaw nodes camera clip --node <id> --device-id <id>
openclaw nodes camera clip --node <id> --no-audio
```

Example 3 (typescript):
```typescript
openclaw nodes screen record --node <id> --duration 10s --fps 15   # prints MEDIA:<path>
```

---

## Location command

**URL:** https://docs.openclaw.ai/nodes/location-command

**Contents:**
- Location command
- Documentation Index
- ​TL;DR
- ​Why a selector (not just a switch)
- ​Settings model
- ​Permissions mapping (node.permissions)
- ​Command: location.get
- ​Background behavior
- ​Model/tooling integration
- ​UX copy (suggested)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  "timeoutMs": 10000,
  "maxAgeMs": 15000,
  "desiredAccuracy": "coarse|balanced|precise"
}
```

Example 2 (json):
```json
{
  "lat": 48.20849,
  "lon": 16.37208,
  "accuracyMeters": 12.5,
  "altitudeMeters": 182.0,
  "speedMps": 0.0,
  "headingDeg": 270.0,
  "timestamp": "2026-01-03T12:34:56.000Z",
  "isPrecise": true,
  "source": "gps|wifi|cell|unknown"
}
```

---

## Nodes

**URL:** https://docs.openclaw.ai/nodes/index

**Contents:**
- Nodes
- Documentation Index
- ​Pairing + status
- ​Remote node host (system.run)
  - ​What runs where
  - ​Start a node host (foreground)
  - ​Remote gateway via SSH tunnel (loopback bind)
  - ​Start a node host (service)
  - ​Pair + name
  - ​Allowlist the commands

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
openclaw devices list
openclaw devices approve <requestId>
openclaw devices reject <requestId>
openclaw nodes status
openclaw nodes describe --node <idOrNameOrIp>
```

Example 2 (unknown):
```unknown
openclaw node run --host <gateway-host> --port 18789 --display-name "Build Node"
```

Example 3 (elixir):
```elixir
# Terminal A (keep running): forward local 18790 -> gateway 127.0.0.1:18789
ssh -N -L 18790:127.0.0.1:18789 user@gateway-host

# Terminal B: export the gateway token and connect through the tunnel
export OPENCLAW_GATEWAY_TOKEN="<gateway-token>"
openclaw node run --host 127.0.0.1 --port 18790 --display-name "Build Node"
```

Example 4 (unknown):
```unknown
openclaw node install --host <gateway-host> --port 18789 --display-name "Build Node"
openclaw node start
openclaw node restart
```

---
