# Openclaw-Mcp_Docs - Channels

**Pages:** 39

---

## Pairing

**URL:** https://docs.openclaw.ai/cli/pairing

**Contents:**
- Pairing
- Documentation Index
- ​openclaw pairing
- ​Commands
- ​pairing list
- ​pairing approve
- ​Notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
openclaw pairing list telegram
openclaw pairing list --channel telegram --account work
openclaw pairing list telegram --json

openclaw pairing approve <code>
openclaw pairing approve telegram <code>
openclaw pairing approve --channel telegram --account work <code> --notify
```

---

## Gateway protocol

**URL:** https://docs.openclaw.ai/gateway/protocol

**Contents:**
- Gateway protocol
- Documentation Index
- ​Transport
- ​Handshake (connect)
  - ​Node example
- ​Framing
- ​Roles + scopes
  - ​Roles
  - ​Scopes (operator)
  - ​Caps/commands/permissions (node)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Channels and login helpers

Secrets, config, update, and wizard

Agent and workspace helpers

Device pairing and device tokens

Node pairing, invoke, and pending work

Automation, skills, and tools

**Examples:**

Example 1 (json):
```json
{
  "type": "event",
  "event": "connect.challenge",
  "payload": { "nonce": "…", "ts": 1737264000000 }
}
```

Example 2 (json):
```json
{
  "type": "req",
  "id": "…",
  "method": "connect",
  "params": {
    "minProtocol": 3,
    "maxProtocol": 3,
    "client": {
      "id": "cli",
      "version": "1.2.3",
      "platform": "macos",
      "mode": "operator"
    },
    "role": "operator",
    "scopes": ["operator.read", "operator.write"],
    "caps": [],
    "commands": [],
    "permissions": {},
    "auth": { "token": "…" },
    "locale": "en-US",
    "userAgent": "openclaw-cli/1.2.3",
    "device": {
      "id": "device_fingerprint",
      "publicKey": "…",
      "signature": "…",
      "signedAt": 1737264000000,
      "nonce": "…"
    }
  }
}
```

Example 3 (json):
```json
{
  "type": "res",
  "id": "…",
  "ok": true,
  "payload": {
    "type": "hello-ok",
    "protocol": 3,
    "server": { "version": "…", "connId": "…" },
    "features": { "methods": ["…"], "events": ["…"] },
    "snapshot": { "…": "…" },
    "auth": {
      "role": "operator",
      "scopes": ["operator.read", "operator.write"]
    },
    "policy": {
      "maxPayload": 26214400,
      "maxBufferedBytes": 52428800,
      "tickIntervalMs": 15000
    }
  }
}
```

Example 4 (json):
```json
{
  "auth": {
    "role": "operator",
    "scopes": ["operator.read", "operator.write"]
  }
}
```

---

## Matrix

**URL:** https://docs.openclaw.ai/channels/matrix

**Contents:**
- Matrix
- Documentation Index
- ​Install
- ​Setup
  - ​Interactive setup
  - ​Minimal config
  - ​Auto-join
  - ​Allowlist target formats
  - ​Account ID normalization
  - ​Cached credentials

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Deleted or invalid Matrix device

**Examples:**

Example 1 (elixir):
```elixir
openclaw plugins install @openclaw/matrix
```

Example 2 (unknown):
```unknown
openclaw plugins install ./path/to/local/matrix-plugin
```

Example 3 (unknown):
```unknown
openclaw channels add
openclaw configure --section channels
```

Example 4 (json):
```json
{
  channels: {
    matrix: {
      enabled: true,
      homeserver: "https://matrix.example.org",
      accessToken: "syt_xxx",
      dm: { policy: "pairing" },
    },
  },
}
```

---

## Gateway-owned pairing

**URL:** https://docs.openclaw.ai/gateway/pairing

**Contents:**
- Gateway-owned pairing
- Documentation Index
- ​Concepts
- ​How pairing works
- ​CLI workflow (headless friendly)
- ​API surface (gateway protocol)
- ​Node command gating (2026.3.31+)
- ​Node event trust boundaries (2026.3.31+)
- ​Auto-approval (macOS app)
- ​Trusted-CIDR device auto-approval

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
openclaw nodes pending
openclaw nodes approve <requestId>
openclaw nodes reject <requestId>
openclaw nodes status
openclaw nodes remove --node <id|name|ip>
openclaw nodes rename --node <id|name|ip> --name "Living Room iPad"
```

Example 2 (json):
```json
{
  gateway: {
    nodes: {
      pairing: {
        autoApproveCidrs: ["192.168.1.0/24"],
      },
    },
  },
}
```

---

## Pairing

**URL:** https://docs.openclaw.ai/channels/pairing

**Contents:**
- Pairing
- Documentation Index
- ​1) DM pairing (inbound chat access)
  - ​Approve a sender
  - ​Reusable sender groups
  - ​Where the state lives
- ​2) Node device pairing (iOS/Android/macOS/headless nodes)
  - ​Pair via Telegram (recommended for iOS)
  - ​Approve a node device
  - ​Optional trusted-CIDR node auto-approve

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
openclaw pairing list telegram
openclaw pairing approve telegram <CODE>
```

Example 2 (sass):
```sass
{
  accessGroups: {
    operators: {
      type: "message.senders",
      members: {
        discord: ["discord:123456789012345678"],
        telegram: ["987654321"],
        whatsapp: ["+15551234567"],
      },
    },
  },
  channels: {
    telegram: { dmPolicy: "allowlist", allowFrom: ["accessGroup:operators"] },
    whatsapp: { groupPolicy: "allowlist", groupAllowFrom: ["accessGroup:operators"] },
  },
}
```

Example 3 (typescript):
```typescript
openclaw devices list
openclaw devices approve <requestId>
openclaw devices reject <requestId>
```

Example 4 (json):
```json
{
  gateway: {
    nodes: {
      pairing: {
        autoApproveCidrs: ["192.168.1.0/24"],
      },
    },
  },
}
```

---

## LINE

**URL:** https://docs.openclaw.ai/channels/line

**Contents:**
- LINE
- Documentation Index
- ​Install
- ​Setup
- ​Configure
- ​Access control
- ​Message behavior
- ​Channel data (rich messages)
- ​ACP support
- ​Outbound media

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (elixir):
```elixir
openclaw plugins install @openclaw/line
```

Example 2 (unknown):
```unknown
openclaw plugins install ./path/to/local/line-plugin
```

Example 3 (yaml):
```yaml
https://gateway-host/line/webhook
```

Example 4 (json):
```json
{
  channels: {
    line: {
      enabled: true,
      channelAccessToken: "LINE_CHANNEL_ACCESS_TOKEN",
      channelSecret: "LINE_CHANNEL_SECRET",
      dmPolicy: "pairing",
    },
  },
}
```

---

## Chat channels

**URL:** https://docs.openclaw.ai/channels

**Contents:**
- Chat channels
- Documentation Index
- ​Delivery notes
- ​Supported channels
- ​Notes

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## iMessage

**URL:** https://docs.openclaw.ai/channels/imessage

**Contents:**
- iMessage
- Documentation Index
- BlueBubbles (recommended)
- Pairing
- Configuration reference
- ​Quick setup
- ​Requirements and permissions (macOS)
- ​Access control and routing
- ​ACP conversation bindings
- ​Deployment patterns

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Install and verify imsg

Approve first DM pairing (default dmPolicy)

Dedicated bot macOS user (separate iMessage identity)

Remote Mac over Tailscale (example)

Multi-account pattern

Attachments and media

imsg not found or RPC unsupported

Group messages are ignored

Remote attachments fail

macOS permission prompts were missed

**Examples:**

Example 1 (unknown):
```unknown
brew install steipete/tap/imsg
imsg rpc --help
```

Example 2 (json):
```json
{
  channels: {
    imessage: {
      enabled: true,
      cliPath: "/usr/local/bin/imsg",
      dbPath: "/Users/user/Library/Messages/chat.db",
    },
  },
}
```

Example 3 (unknown):
```unknown
openclaw gateway
```

Example 4 (typescript):
```typescript
openclaw pairing list imessage
openclaw pairing approve imessage <CODE>
```

---

## Synology Chat

**URL:** https://docs.openclaw.ai/channels/synology-chat

**Contents:**
- Synology Chat
- Documentation Index
- ​Bundled plugin
- ​Quick setup
- ​Environment variables
- ​DM policy and access control
- ​Outbound delivery
- ​Multi-account
- ​Security notes
- ​Troubleshooting

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw plugins install ./path/to/local/synology-chat-plugin
```

Example 2 (sass):
```sass
{
  channels: {
    "synology-chat": {
      enabled: true,
      token: "synology-outgoing-token",
      incomingUrl: "https://nas.example.com/webapi/entry.cgi?api=SYNO.Chat.External&method=incoming&version=2&token=...",
      webhookPath: "/webhook/synology",
      dmPolicy: "allowlist",
      allowedUserIds: ["123456"],
      rateLimitPerMinute: 30,
      allowInsecureSsl: false,
    },
  },
}
```

Example 3 (sql):
```sql
openclaw message send --channel synology-chat --target 123456 --text "Hello from OpenClaw"
openclaw message send --channel synology-chat --target synology-chat:123456 --text "Hello again"
openclaw message send --channel synology-chat --target synology:123456 --text "Short prefix"
```

Example 4 (lua):
```lua
{
  channels: {
    "synology-chat": {
      enabled: true,
      accounts: {
        default: {
          token: "token-a",
          incomingUrl: "https://nas-a.example.com/...token=...",
        },
        alerts: {
          token: "token-b",
          incomingUrl: "https://nas-b.example.com/...token=...",
          webhookPath: "/webhook/synology-alerts",
          dmPolicy: "allowlist",
          allowedUserIds: ["987654"],
        },
      },
    },
  },
}
```

---

## Groups

**URL:** https://docs.openclaw.ai/channels/groups

**Contents:**
- Groups
- Documentation Index
- ​Beginner intro (2 minutes)
- ​Visible replies
- ​Context visibility and allowlists
- ​Session keys
- ​Pattern: personal DMs + public groups (single agent)
- ​Display labels
- ​Group policy
- ​Mention gating (default)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Current behavior is channel-specific

Hardening direction (planned)

Default toolsBySender

**Examples:**

Example 1 (swift):
```swift
groupPolicy? disabled -> drop
groupPolicy? allowlist -> group allowed? no -> drop
requireMention? yes -> mentioned? no -> store for context only
otherwise -> reply
```

Example 2 (json):
```json
{
  messages: {
    groupChat: {
      visibleReplies: "automatic",
    },
  },
}
```

Example 3 (json):
```json
{
  messages: {
    visibleReplies: "message_tool",
  },
}
```

Example 4 (json):
```json
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main", // groups/channels are non-main -> sandboxed
        scope: "session", // strongest isolation (one container per group/channel)
        workspaceAccess: "none",
      },
    },
  },
  tools: {
    sandbox: {
      tools: {
        // If allow is non-empty, everything else is blocked (deny still wins).
        allow: ["group:messaging", "group:sessions"],
        deny: ["group:runtime", "group:fs", "group:ui", "nodes", "cron", "gateway"],
      },
    },
  },
}
```

---

## Google Chat

**URL:** https://docs.openclaw.ai/channels/googlechat

**Contents:**
- Google Chat
- Documentation Index
- ​Install
- ​Quick setup (beginner)
- ​Add to Google Chat
- ​Public URL (Webhook-only)
  - ​Option A: Tailscale Funnel (Recommended)
  - ​Option B: Reverse Proxy (Caddy)
  - ​Option C: Cloudflare Tunnel
- ​How it works

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (elixir):
```elixir
openclaw plugins install @openclaw/googlechat
```

Example 2 (unknown):
```unknown
openclaw plugins install ./path/to/local/googlechat-plugin
```

Example 3 (unknown):
```unknown
ss -tlnp | grep 18789
```

Example 4 (markdown):
```markdown
# If bound to localhost (127.0.0.1 or 0.0.0.0):
tailscale serve --bg --https 8443 http://127.0.0.1:18789

# If bound to Tailscale IP only (e.g., 100.106.161.80):
tailscale serve --bg --https 8443 http://100.106.161.80:18789
```

---

## IRC

**URL:** https://docs.openclaw.ai/channels/irc

**Contents:**
- IRC
- Documentation Index
- ​Quick start
- ​Security defaults
- ​Access control
  - ​Common gotcha: allowFrom is for DMs, not channels
- ​Reply triggering (mentions)
- ​Security note (recommended for public channels)
  - ​Same tools for everyone in the channel
  - ​Different tools per sender (owner gets more power)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  channels: {
    irc: {
      enabled: true,
      host: "irc.example.com",
      port: 6697,
      tls: true,
      nick: "openclaw-bot",
      channels: ["#openclaw"],
    },
  },
}
```

Example 2 (unknown):
```unknown
openclaw gateway run
```

Example 3 (json):
```json
{
  channels: {
    irc: {
      groupPolicy: "allowlist",
      groups: {
        "#tuirc-dev": { allowFrom: ["*"] },
      },
    },
  },
}
```

Example 4 (json):
```json
{
  channels: {
    irc: {
      groupPolicy: "allowlist",
      groups: {
        "#tuirc-dev": {
          requireMention: false,
          allowFrom: ["*"],
        },
      },
    },
  },
}
```

---

## Zalo

**URL:** https://docs.openclaw.ai/channels/zalo

**Contents:**
- Zalo
- Documentation Index
- ​Bundled plugin
- ​Quick setup (beginner)
- ​What it is
- ​Setup (fast path)
  - ​1) Create a bot token (Zalo Bot Platform)
  - ​2) Configure the token (env or config)
- ​How it works (behavior)
- ​Limits

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  channels: {
    zalo: {
      enabled: true,
      accounts: {
        default: {
          botToken: "12345689:abc-xyz",
          dmPolicy: "pairing",
        },
      },
    },
  },
}
```

Example 2 (json):
```json
{
  channels: {
    zalo: {
      enabled: true,
      accounts: {
        default: {
          botToken: "12345689:abc-xyz",
          dmPolicy: "pairing",
        },
      },
    },
  },
}
```

---

## WeChat

**URL:** https://docs.openclaw.ai/channels/wechat

**Contents:**
- WeChat
- Documentation Index
- ​Naming
- ​How it works
- ​Install
- ​Login
- ​Access control
- ​Compatibility
- ​Sidecar process
- ​Troubleshooting

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (elixir):
```elixir
npx -y @tencent-weixin/openclaw-weixin-cli install
```

Example 2 (elixir):
```elixir
openclaw plugins install "@tencent-weixin/openclaw-weixin"
openclaw config set plugins.entries.openclaw-weixin.enabled true
```

Example 3 (unknown):
```unknown
openclaw gateway restart
```

Example 4 (unknown):
```unknown
openclaw channels login --channel openclaw-weixin
```

---

## Mattermost

**URL:** https://docs.openclaw.ai/channels/mattermost

**Contents:**
- Mattermost
- Documentation Index
- ​Install
- ​Quick setup
- ​Native slash commands
- ​Environment variables (default account)
- ​Chat modes
- ​Threading and sessions
- ​Access control (DMs)
- ​Channels (groups)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Ensure plugin is available

Create a Mattermost bot

Configure OpenClaw and start the gateway

Reachability requirement

Mattermost egress allowlist

Streaming behavior notes

Buttons replaced with confirmation

Agent receives the selection

Config and reachability

Derive the secret from the bot token

Build the context object

Serialize with sorted keys

No replies in channels

Auth or multi-account errors

Native slash commands fail

**Examples:**

Example 1 (elixir):
```elixir
openclaw plugins install @openclaw/mattermost
```

Example 2 (unknown):
```unknown
openclaw plugins install ./path/to/local/mattermost-plugin
```

Example 3 (json):
```json
{
  channels: {
    mattermost: {
      enabled: true,
      botToken: "mm-token",
      baseUrl: "https://chat.example.com",
      dmPolicy: "pairing",
    },
  },
}
```

Example 4 (json):
```json
{
  channels: {
    mattermost: {
      commands: {
        native: true,
        nativeSkills: true,
        callbackPath: "/api/channels/mattermost/command",
        // Use when Mattermost cannot reach the gateway directly (reverse proxy/public URL).
        callbackUrl: "https://gateway.example.com/api/channels/mattermost/command",
      },
    },
  },
}
```

---

## Signal

**URL:** https://docs.openclaw.ai/channels/signal

**Contents:**
- Signal
- Documentation Index
- ​Prerequisites
- ​Quick setup (beginner)
- ​What it is
- ​Config writes
- ​The number model (important)
- ​Setup path A: link existing Signal account (QR)
- ​Setup path B: register dedicated bot number (SMS, Linux)
- ​External daemon mode (httpUrl)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sass):
```sass
{
  channels: {
    signal: {
      enabled: true,
      account: "+15551234567",
      cliPath: "signal-cli",
      dmPolicy: "pairing",
      allowFrom: ["+15557654321"],
    },
  },
}
```

Example 2 (json):
```json
{
  channels: { signal: { configWrites: false } },
}
```

Example 3 (sass):
```sass
{
  channels: {
    signal: {
      enabled: true,
      account: "+15551234567",
      cliPath: "signal-cli",
      dmPolicy: "pairing",
      allowFrom: ["+15557654321"],
    },
  },
}
```

Example 4 (bash):
```bash
VERSION=$(curl -Ls -o /dev/null -w %{url_effective} https://github.com/AsamK/signal-cli/releases/latest | sed -e 's/^.*\/v//')
curl -L -O "https://github.com/AsamK/signal-cli/releases/download/v${VERSION}/signal-cli-${VERSION}-Linux-native.tar.gz"
sudo tar xf "signal-cli-${VERSION}-Linux-native.tar.gz" -C /opt
sudo ln -sf /opt/signal-cli /usr/local/bin/
signal-cli --version
```

---

## Channel location parsing

**URL:** https://docs.openclaw.ai/channels/location

**Contents:**
- Channel location parsing
- Documentation Index
- ​Text formatting
- ​Context fields
- ​Channel notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
Location (untrusted metadata):
```json
{
  "latitude": 48.858844,
  "longitude": 2.294351,
  "name": "Eiffel Tower",
  "address": "Champ de Mars, Paris",
  "caption": "Meet here"
}
```
```

---

## Zalo personal

**URL:** https://docs.openclaw.ai/channels/zalouser

**Contents:**
- Zalo personal
- Documentation Index
- ​Bundled plugin
- ​Quick setup (beginner)
- ​What it is
- ​Naming
- ​Finding IDs (directory)
- ​Limits
- ​Access control (DMs)
- ​Group access (optional)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  channels: {
    zalouser: {
      enabled: true,
      dmPolicy: "pairing",
    },
  },
}
```

Example 2 (swift):
```swift
openclaw directory self --channel zalouser
openclaw directory peers list --channel zalouser --query "name"
openclaw directory groups list --channel zalouser --query "work"
```

Example 3 (json):
```json
{
  channels: {
    zalouser: {
      groupPolicy: "allowlist",
      groupAllowFrom: ["1471383327500481391"],
      groups: {
        "123456789": { allow: true },
        "Work Chat": { allow: true },
      },
    },
  },
}
```

Example 4 (json):
```json
{
  channels: {
    zalouser: {
      groupPolicy: "allowlist",
      groups: {
        "*": { allow: true, requireMention: true },
        "Work Chat": { allow: true, requireMention: false },
      },
    },
  },
}
```

---

## Channel routing

**URL:** https://docs.openclaw.ai/channels/channel-routing

**Contents:**
- Channel routing
- Documentation Index
- ​Channels & routing
- ​Key terms
- ​Outbound target prefixes
- ​Session key shapes (examples)
- ​Main DM route pinning
- ​Guarded inbound recording
- ​Routing rules (how an agent is chosen)
- ​Broadcast groups (run multiple agents)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sass):
```sass
{
  broadcast: {
    strategy: "parallel",
    "120363403215116621@g.us": ["alfred", "baerbel"],
    "+15555550123": ["support", "logger"],
  },
}
```

Example 2 (json):
```json
{
  agents: {
    list: [{ id: "support", name: "Support", workspace: "~/.openclaw/workspace-support" }],
  },
  bindings: [
    { match: { channel: "slack", teamId: "T123" }, agentId: "support" },
    { match: { channel: "telegram", peer: { kind: "group", id: "-100123" } }, agentId: "support" },
  ],
}
```

---

## Channel troubleshooting

**URL:** https://docs.openclaw.ai/channels/troubleshooting

**Contents:**
- Channel troubleshooting
- Documentation Index
- ​Command ladder
- ​WhatsApp
  - ​WhatsApp failure signatures
- ​Telegram
  - ​Telegram failure signatures
- ​Discord
  - ​Discord failure signatures
- ​Slack

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

---

## Nostr

**URL:** https://docs.openclaw.ai/channels/nostr

**Contents:**
- Nostr
- Documentation Index
- ​Bundled plugin
  - ​Older/custom installs
  - ​Non-interactive setup
- ​Quick setup
- ​Configuration reference
- ​Profile metadata
- ​Access control
  - ​DM policies

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (elixir):
```elixir
openclaw plugins install @openclaw/nostr
```

Example 2 (unknown):
```unknown
openclaw plugins install --link <path-to-local-nostr-plugin>
```

Example 3 (bash):
```bash
openclaw channels add --channel nostr --private-key "$NOSTR_PRIVATE_KEY"
openclaw channels add --channel nostr --private-key "$NOSTR_PRIVATE_KEY" --relay-urls "wss://relay.damus.io,wss://relay.primal.net"
```

Example 4 (julia):
```julia
# Using nak
nak key generate
```

---

## Yuanbao

**URL:** https://docs.openclaw.ai/channels/yuanbao

**Contents:**
- Yuanbao
- Documentation Index
- ​Yuanbao
- ​Quick start
  - ​Interactive setup (alternative)
- ​Access control
  - ​Direct messages
  - ​Group chats
- ​Configuration examples
  - ​Basic setup with open DM policy

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Add the Yuanbao channel with your credentials

After setup completes, restart the gateway to apply the changes

**Examples:**

Example 1 (unknown):
```unknown
openclaw channels add --channel yuanbao --token "appKey:appSecret"
```

Example 2 (unknown):
```unknown
openclaw gateway restart
```

Example 3 (unknown):
```unknown
openclaw channels login --channel yuanbao
```

Example 4 (typescript):
```typescript
openclaw pairing list yuanbao
openclaw pairing approve yuanbao <CODE>
```

---

## Configuration — channels

**URL:** https://docs.openclaw.ai/gateway/config-channels

**Contents:**
- Configuration — channels
- Documentation Index
- ​Channels
  - ​DM and group access
  - ​Channel model overrides
  - ​Channel defaults and heartbeat
  - ​WhatsApp
  - ​Telegram
  - ​Discord
  - ​Google Chat

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Multi-account WhatsApp

iMessage SSH wrapper example

**Examples:**

Example 1 (json):
```json
{
  channels: {
    modelByChannel: {
      discord: {
        "123456789012345678": "anthropic/claude-opus-4-6",
      },
      slack: {
        C1234567890: "openai/gpt-4.1",
      },
      telegram: {
        "-1001234567890": "openai/gpt-4.1-mini",
        "-1001234567890:topic:99": "anthropic/claude-sonnet-4-6",
      },
    },
  },
}
```

Example 2 (json):
```json
{
  channels: {
    defaults: {
      groupPolicy: "allowlist", // open | allowlist | disabled
      contextVisibility: "all", // all | allowlist | allowlist_quote
      heartbeat: {
        showOk: false,
        showAlerts: true,
        useIndicator: true,
      },
    },
  },
}
```

Example 3 (sass):
```sass
{
  web: {
    enabled: true,
    heartbeatSeconds: 60,
    whatsapp: {
      keepAliveIntervalMs: 25000,
      connectTimeoutMs: 60000,
      defaultQueryTimeoutMs: 60000,
    },
    reconnect: {
      initialMs: 2000,
      maxMs: 120000,
      factor: 1.4,
      jitter: 0.2,
      maxAttempts: 0,
    },
  },
  channels: {
    whatsapp: {
      dmPolicy: "pairing", // pairing | allowlist | open | disabled
      allowFrom: ["+15555550123", "+447700900123"],
      textChunkLimit: 4000,
      chunkMode: "length", // length | newline
      mediaMaxMb: 50,
      sendReadReceipts: true, // blue ticks (false in self-chat mode)
      groups: {
        "*": { requireMention: true },
      },
      groupPolicy: "allowlist",
      groupAllowFrom: ["+15551234567"],
    },
  },
}
```

Example 4 (json):
```json
{
  channels: {
    whatsapp: {
      accounts: {
        default: {},
        personal: {},
        biz: {
          // authDir: "~/.openclaw/credentials/whatsapp/biz",
        },
      },
    },
  },
}
```

---

## QA channel

**URL:** https://docs.openclaw.ai/channels/qa-channel

**Contents:**
- QA channel
- Documentation Index
- ​What it does
- ​Config
- ​Runners
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  "channels": {
    "qa-channel": {
      "baseUrl": "http://127.0.0.1:43123",
      "botUserId": "openclaw",
      "botDisplayName": "OpenClaw QA",
      "allowFrom": ["*"],
      "pollTimeoutMs": 1000
    }
  }
}
```

Example 2 (unknown):
```unknown
pnpm qa:e2e
```

Example 3 (unknown):
```unknown
pnpm openclaw qa suite
```

Example 4 (unknown):
```unknown
pnpm qa:lab:up
```

---

## Tlon

**URL:** https://docs.openclaw.ai/channels/tlon

**Contents:**
- Tlon
- Documentation Index
- ​Bundled plugin
- ​Setup
- ​Private/LAN ships
- ​Group channels
- ​Access control
- ​Owner and approval system
- ​Auto-accept settings
- ​Delivery targets (CLI/cron)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (elixir):
```elixir
openclaw plugins install @openclaw/tlon
```

Example 2 (unknown):
```unknown
openclaw plugins install ./path/to/local/tlon-plugin
```

Example 3 (json):
```json
{
  channels: {
    tlon: {
      enabled: true,
      ship: "~sampel-palnet",
      url: "https://your-ship-host",
      code: "lidlut-tabwed-pillex-ridrup",
      ownerShip: "~your-main-ship", // recommended: your ship, always allowed
    },
  },
}
```

Example 4 (json):
```json
{
  channels: {
    tlon: {
      url: "http://localhost:8080",
      allowPrivateNetwork: true,
    },
  },
}
```

---

## Slack

**URL:** https://docs.openclaw.ai/channels/slack

**Contents:**
- Slack
- Documentation Index
- Pairing
- Slash commands
- Channel troubleshooting
- ​Quick setup
- ​Socket Mode transport tuning
- ​Manifest and scope checklist
  - ​Additional manifest settings
- ​Token model

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Create a new Slack app

Create a new Slack app

Optional native slash commands

Optional authorship scopes (write operations)

Optional user-token scopes (read operations)

Outbound text and files

High-signal Slack fields

No replies in channels

Socket mode not connecting

HTTP mode not receiving events

Native/slash commands not firing

**Examples:**

Example 1 (sass):
```sass
export SLACK_APP_TOKEN=xapp-...
export SLACK_BOT_TOKEN=xoxb-...
cat > slack.socket.patch.json5 <<'JSON5'
{
  channels: {
    slack: {
      enabled: true,
      mode: "socket",
      appToken: { source: "env", provider: "default", id: "SLACK_APP_TOKEN" },
      botToken: { source: "env", provider: "default", id: "SLACK_BOT_TOKEN" },
    },
  },
}
JSON5
openclaw config patch --file ./slack.socket.patch.json5 --dry-run
openclaw config patch --file ./slack.socket.patch.json5
```

Example 2 (lua):
```lua
SLACK_APP_TOKEN=xapp-...
SLACK_BOT_TOKEN=xoxb-...
```

Example 3 (unknown):
```unknown
openclaw gateway
```

Example 4 (sass):
```sass
export SLACK_BOT_TOKEN=xoxb-...
export SLACK_SIGNING_SECRET=...
cat > slack.http.patch.json5 <<'JSON5'
{
  channels: {
    slack: {
      enabled: true,
      mode: "http",
      botToken: { source: "env", provider: "default", id: "SLACK_BOT_TOKEN" },
      signingSecret: { source: "env", provider: "default", id: "SLACK_SIGNING_SECRET" },
      webhookPath: "/slack/events",
    },
  },
}
JSON5
openclaw config patch --file ./slack.http.patch.json5 --dry-run
openclaw config patch --file ./slack.http.patch.json5
```

---

## Matrix migration

**URL:** https://docs.openclaw.ai/channels/matrix-migration

**Contents:**
- Matrix migration
- Documentation Index
- ​What the migration does automatically
- ​What the migration cannot do automatically
- ​Recommended upgrade flow
- ​How encrypted migration works
- ​Common messages and what they mean
  - ​Upgrade and detection messages
  - ​Encrypted-state recovery messages
  - ​Manual recovery messages

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw doctor --fix
```

Example 2 (unknown):
```unknown
openclaw matrix verify status
openclaw matrix verify backup status
```

Example 3 (bash):
```bash
printf '%s\n' "$MATRIX_RECOVERY_KEY" | openclaw matrix verify backup restore --recovery-key-stdin
printf '%s\n' "$MATRIX_RECOVERY_KEY_ASSISTANT" | openclaw matrix verify backup restore --recovery-key-stdin --account assistant
```

Example 4 (bash):
```bash
printf '%s\n' "$MATRIX_RECOVERY_KEY" | openclaw matrix verify device --recovery-key-stdin
printf '%s\n' "$MATRIX_RECOVERY_KEY_ASSISTANT" | openclaw matrix verify device --recovery-key-stdin --account assistant
```

---

## Group messages

**URL:** https://docs.openclaw.ai/channels/group-messages

**Contents:**
- Group messages
- Documentation Index
- ​Current implementation (2025-12-03)
- ​Config example (WhatsApp)
  - ​Activation command (owner-only)
- ​How to use
- ​Testing / verification
- ​Known considerations
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  channels: {
    whatsapp: {
      groups: {
        "*": { requireMention: true },
      },
    },
  },
  agents: {
    list: [
      {
        id: "main",
        groupChat: {
          historyLimit: 50,
          mentionPatterns: ["@?openclaw", "\\+?15555550123"],
        },
      },
    ],
  },
}
```

---

## Matrix push rules for quiet previews

**URL:** https://docs.openclaw.ai/channels/matrix-push-rules

**Contents:**
- Matrix push rules for quiet previews
- Documentation Index
- ​Prerequisites
- ​Steps
- ​Multi-bot notes
- ​Homeserver notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Configure quiet previews

Get the recipient's access token

Install the override push rule

**Examples:**

Example 1 (json):
```json
{
  channels: {
    matrix: {
      streaming: "quiet",
    },
  },
}
```

Example 2 (json):
```json
curl -sS -X POST \
  "https://matrix.example.org/_matrix/client/v3/login" \
  -H "Content-Type: application/json" \
  --data '{
    "type": "m.login.password",
    "identifier": { "type": "m.id.user", "user": "@alice:example.org" },
    "password": "REDACTED"
  }'
```

Example 3 (bash):
```bash
curl -sS \
  -H "Authorization: Bearer $USER_ACCESS_TOKEN" \
  "https://matrix.example.org/_matrix/client/v3/pushers"
```

Example 4 (json):
```json
curl -sS -X PUT \
  "https://matrix.example.org/_matrix/client/v3/pushrules/global/override/openclaw-finalized-preview-botname" \
  -H "Authorization: Bearer $USER_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  --data '{
    "conditions": [
      { "kind": "event_match", "key": "type", "pattern": "m.room.message" },
      {
        "kind": "event_property_is",
        "key": "content.m\\.relates_to.rel_type",
        "value": "m.replace"
      },
      {
        "kind": "event_property_is",
        "key": "content.com\\.openclaw\\.finalized_preview",
        "value": true
      },
      { "kind": "event_match", "key": "sender", "pattern": "@bot:example.org" }
    ],
    "actions": [
      "notify",
      { "set_tweak": "sound", "value": "default" },
      { "set_tweak": "highlight", "value": false }
    ]
  }'
```

---

## Nextcloud Talk

**URL:** https://docs.openclaw.ai/channels/nextcloud-talk

**Contents:**
- Nextcloud Talk
- Documentation Index
- ​Bundled plugin
- ​Quick setup (beginner)
- ​Notes
- ​Access control (DMs)
- ​Rooms (groups)
- ​Capabilities
- ​Configuration reference (Nextcloud Talk)
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (elixir):
```elixir
openclaw plugins install @openclaw/nextcloud-talk
```

Example 2 (unknown):
```unknown
openclaw plugins install ./path/to/local/nextcloud-talk-plugin
```

Example 3 (unknown):
```unknown
./occ talk:bot:install "OpenClaw" "<shared-secret>" "<webhook-url>" --feature reaction
```

Example 4 (unknown):
```unknown
openclaw channels add --channel nextcloud-talk \
  --url https://cloud.example.com \
  --token "<shared-secret>"
```

---

## Discord

**URL:** https://docs.openclaw.ai/channels/discord

**Contents:**
- Discord
- Documentation Index
- Pairing
- Slash commands
- Channel troubleshooting
- ​Quick setup
- ​Recommended: Set up a guild workspace
- ​Runtime model
- ​Forum channels
- ​Interactive components

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Create a Discord application and bot

Enable privileged intents

Generate an invite URL and add the bot to your server

Enable Developer Mode and collect your IDs

Allow DMs from server members

Set your bot token securely (do not send it in chat)

Configure OpenClaw and pair

Approve first DM pairing

Add your server to the guild allowlist

Allow responses without @mention

Plan for memory in guild channels

Reply tags and native replies

History, context, and thread behavior

Thread-bound sessions for subagents

Persistent ACP channel bindings

Reaction notifications

Outbound mention aliases

Presence configuration

Used disallowed intents or bot sees no guild messages

Guild messages blocked unexpectedly

Require mention false but still blocked

Long-running Discord turns or duplicate replies

Gateway metadata lookup timeout warnings

Gateway READY timeout restarts

Permissions audit mismatches

DM and pairing issues

Voice STT drops with DecryptionFailed(...)

High-signal Discord fields

**Examples:**

Example 1 (json):
```json
export DISCORD_BOT_TOKEN="YOUR_BOT_TOKEN"
cat > discord.patch.json5 <<'JSON5'
{
  channels: {
    discord: {
      enabled: true,
      token: { source: "env", provider: "default", id: "DISCORD_BOT_TOKEN" },
    },
  },
}
JSON5
openclaw config patch --file ./discord.patch.json5 --dry-run
openclaw config patch --file ./discord.patch.json5
openclaw gateway
```

Example 2 (json):
```json
{
  channels: {
    discord: {
      enabled: true,
      token: {
        source: "env",
        provider: "default",
        id: "DISCORD_BOT_TOKEN",
      },
    },
  },
}
```

Example 3 (lua):
```lua
DISCORD_BOT_TOKEN=...
```

Example 4 (json):
```json
{
  channels: {
    discord: {
      enabled: true,
      accounts: {
        personal: {
          token: { source: "env", provider: "default", id: "DISCORD_PERSONAL_TOKEN" },
          applicationId: "111111111111111111",
        },
        work: {
          token: { source: "env", provider: "default", id: "DISCORD_WORK_TOKEN" },
          applicationId: "222222222222222222",
        },
      },
    },
  },
}
```

---

## Twitch

**URL:** https://docs.openclaw.ai/channels/twitch

**Contents:**
- Twitch
- Documentation Index
- ​Bundled plugin
- ​Quick setup (beginner)
- ​What it is
- ​Setup (detailed)
  - ​Generate credentials
  - ​Configure the bot
  - ​Access control (recommended)
- ​Token refresh (optional)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Ensure plugin is available

Create a Twitch bot account

Find your Twitch user ID

Bot does not respond to messages

Token refresh not working

**Examples:**

Example 1 (elixir):
```elixir
openclaw plugins install @openclaw/twitch
```

Example 2 (unknown):
```unknown
openclaw plugins install ./path/to/local/twitch-plugin
```

Example 3 (sql):
```sql
{
  channels: {
    twitch: {
      enabled: true,
      username: "openclaw", // Bot's Twitch account
      accessToken: "oauth:abc123...", // OAuth Access Token (or use OPENCLAW_TWITCH_ACCESS_TOKEN env var)
      clientId: "xyz789...", // Client ID from Token Generator
      channel: "vevisk", // Which Twitch channel's chat to join (required)
      allowFrom: ["123456789"], // (recommended) Your Twitch user ID only - get it from https://www.streamweasels.com/tools/convert-twitch-username-to-user-id/
    },
  },
}
```

Example 4 (lua):
```lua
OPENCLAW_TWITCH_ACCESS_TOKEN=oauth:abc123...
```

---

## BlueBubbles

**URL:** https://docs.openclaw.ai/channels/bluebubbles

**Contents:**
- BlueBubbles
- Documentation Index
- ​Overview
- ​Quick start
- ​Keeping Messages.app alive (VM / headless setups)
- ​Onboarding
- ​Access control (DMs + groups)
  - ​Contact name enrichment (macOS, optional)
  - ​Mention gating (groups)
  - ​Command gating

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Point webhooks at the gateway

Install a LaunchAgent

Config actually loaded

Debounce window wide enough for your setup

Session JSONL timestamps ≠ webhook arrival

Memory pressure slowing reply dispatch

Reply-quote sends are a different path

Connection and webhook

Delivery and chunking

**Examples:**

Example 1 (json):
```json
{
  channels: {
    bluebubbles: {
      enabled: true,
      serverUrl: "http://192.168.1.100:1234",
      password: "example-password",
      webhookPath: "/bluebubbles-webhook",
    },
  },
}
```

Example 2 (lua):
```lua
try
  tell application "Messages"
    if not running then
      launch
    end if

    -- Touch the scripting interface to keep the process responsive.
    set _chatCount to (count of chats)
  end tell
on error
  -- Ignore transient failures (first-run prompts, locked session, etc).
end try
```

Example 3 (xml):
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>Label</key>
    <string>com.user.poke-messages</string>

    <key>ProgramArguments</key>
    <array>
      <string>/bin/bash</string>
      <string>-lc</string>
      <string>/usr/bin/osascript &quot;$HOME/Scripts/poke-messages.scpt&quot;</string>
    </array>

    <key>RunAtLoad</key>
    <true/>

    <key>StartInterval</key>
    <integer>300</integer>

    <key>StandardOutPath</key>
    <string>/tmp/poke-messages.log</string>
    <key>StandardErrorPath</key>
    <string>/tmp/poke-messages.err</string>
  </dict>
</plist>
```

Example 4 (unknown):
```unknown
launchctl unload ~/Library/LaunchAgents/com.user.poke-messages.plist 2>/dev/null || true
launchctl load ~/Library/LaunchAgents/com.user.poke-messages.plist
```

---

## WhatsApp

**URL:** https://docs.openclaw.ai/channels/whatsapp

**Contents:**
- WhatsApp
- Documentation Index
- ​Install (on demand)
- Pairing
- Channel troubleshooting
- Gateway configuration
- ​Quick setup
- ​Deployment patterns
- ​Runtime model
- ​Plugin hooks and privacy

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

Configure WhatsApp access policy

Approve first pairing request (if using pairing mode)

Dedicated number (recommended)

Personal-number fallback

WhatsApp Web-only channel scope

Inbound envelope + reply context

Media placeholders and location/contact extraction

Pending group history injection

Outbound media behavior

Media size limits and fallback behavior

Account selection and defaults

Credential paths and legacy compatibility

Not linked (QR required)

Linked but disconnected / reconnect loop

QR login times out behind a proxy

No active listener when sending

Reply appears in transcript but not in WhatsApp

Group messages unexpectedly ignored

**Examples:**

Example 1 (elixir):
```elixir
openclaw plugins install @openclaw/whatsapp
```

Example 2 (sass):
```sass
{
  channels: {
    whatsapp: {
      dmPolicy: "pairing",
      allowFrom: ["+15551234567"],
      groupPolicy: "allowlist",
      groupAllowFrom: ["+15551234567"],
    },
  },
}
```

Example 3 (unknown):
```unknown
openclaw channels login --channel whatsapp
```

Example 4 (unknown):
```unknown
openclaw channels login --channel whatsapp --account work
```

---

## QQ bot

**URL:** https://docs.openclaw.ai/channels/qqbot

**Contents:**
- QQ bot
- Documentation Index
- ​Install
- ​Setup
- ​Configure
  - ​Multi-account setup
  - ​Group chats
  - ​Voice (STT / TTS)
- ​Target formats
- ​Slash commands

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (elixir):
```elixir
openclaw plugins install @openclaw/qqbot
```

Example 2 (unknown):
```unknown
openclaw channels add --channel qqbot --token "AppID:AppSecret"
```

Example 3 (unknown):
```unknown
openclaw channels add
openclaw configure --section channels
```

Example 4 (json):
```json
{
  channels: {
    qqbot: {
      enabled: true,
      appId: "YOUR_APP_ID",
      clientSecret: "YOUR_APP_SECRET",
    },
  },
}
```

---

## Access groups

**URL:** https://docs.openclaw.ai/channels/access-groups

**Contents:**
- Access groups
- Documentation Index
- ​Static message sender groups
- ​Reference groups from allowlists
- ​Supported message-channel paths
- ​Discord channel audiences
- ​Security notes
- ​Troubleshooting

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sass):
```sass
{
  accessGroups: {
    operators: {
      type: "message.senders",
      members: {
        "*": ["global-owner-id"],
        discord: ["discord:123456789012345678"],
        telegram: ["987654321"],
        whatsapp: ["+15551234567"],
      },
    },
  },
}
```

Example 2 (json):
```json
{
  accessGroups: {
    operators: {
      type: "message.senders",
      members: {
        discord: ["discord:123456789012345678"],
        telegram: ["987654321"],
      },
    },
  },
  channels: {
    discord: {
      dmPolicy: "allowlist",
      allowFrom: ["accessGroup:operators"],
    },
    telegram: {
      dmPolicy: "allowlist",
      allowFrom: ["accessGroup:operators"],
    },
  },
}
```

Example 3 (sass):
```sass
{
  accessGroups: {
    oncall: {
      type: "message.senders",
      members: {
        whatsapp: ["+15551234567"],
        googlechat: ["users/1234567890"],
      },
    },
  },
  channels: {
    whatsapp: {
      groupPolicy: "allowlist",
      groupAllowFrom: ["accessGroup:oncall"],
    },
    googlechat: {
      spaces: {
        "spaces/AAA": {
          users: ["accessGroup:oncall"],
        },
      },
    },
  },
}
```

Example 4 (json):
```json
{
  channels: {
    discord: {
      dmPolicy: "allowlist",
      allowFrom: ["accessGroup:operators", "discord:123456789012345678"],
    },
  },
}
```

---

## Channels

**URL:** https://docs.openclaw.ai/cli/channels

**Contents:**
- Channels
- Documentation Index
- ​openclaw channels
- ​Common commands
- ​Status / capabilities / resolve / logs
- ​Add / remove accounts
- ​Login and logout (interactive)
- ​Troubleshooting
- ​Capabilities probe
- ​Resolve names to IDs

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (elixir):
```elixir
openclaw channels list
openclaw channels status
openclaw channels capabilities
openclaw channels capabilities --channel discord --target channel:123
openclaw channels resolve --channel slack "#general" "@jane"
openclaw channels logs --channel all
```

Example 2 (bash):
```bash
openclaw channels add --channel telegram --token <bot-token>
openclaw channels add --channel nostr --private-key "$NOSTR_PRIVATE_KEY"
openclaw channels remove --channel telegram --delete
```

Example 3 (unknown):
```unknown
openclaw channels login --channel whatsapp
openclaw channels logout --channel whatsapp
```

Example 4 (json):
```json
openclaw channels capabilities
openclaw channels capabilities --channel discord --target channel:123
```

---

## Microsoft Teams

**URL:** https://docs.openclaw.ai/channels/msteams

**Contents:**
- Microsoft Teams
- Documentation Index
- ​Bundled plugin
- ​Quick setup
- ​Goals
- ​Config writes
- ​Access control (DMs + groups)
- ​Federated authentication (certificate plus managed identity)
  - ​Option A: Certificate-based authentication
  - ​Option B: Azure Managed Identity

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (elixir):
```elixir
openclaw plugins install @openclaw/msteams
```

Example 2 (unknown):
```unknown
openclaw plugins install ./path/to/local/msteams-plugin
```

Example 3 (elixir):
```elixir
npm install -g @microsoft/teams.cli@preview
teams login
teams status   # verify you're logged in and see your tenant info
```

Example 4 (vue):
```vue
# One-time setup (persistent URL across sessions):
devtunnel create my-openclaw-bot --allow-anonymous
devtunnel port create my-openclaw-bot -p 3978 --protocol auto

# Each dev session:
devtunnel host my-openclaw-bot
# Your endpoint: https://<tunnel-id>.devtunnels.ms/api/messages
```

---

## Channel presentation refactor plan

**URL:** https://docs.openclaw.ai/plan/ui-channels

**Contents:**
- Channel presentation refactor plan
- Documentation Index
- ​Status
- ​Problem
- ​Goals
- ​Non goals
- ​Target model
- ​Delivery metadata
- ​Runtime capability contract
- ​Channel mapping

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
type MessagePresentationTone = "neutral" | "info" | "success" | "warning" | "danger";

type MessagePresentation = {
  tone?: MessagePresentationTone;
  title?: string;
  blocks: MessagePresentationBlock[];
};

type MessagePresentationBlock =
  | { type: "text"; text: string }
  | { type: "context"; text: string }
  | { type: "divider" }
  | { type: "buttons"; buttons: MessagePresentationButton[] }
  | { type: "select"; placeholder?: string; options: MessagePresentationOption[] };

type MessagePresentationButton = {
  label: string;
  value?: string;
  url?: string;
  style?: "primary" | "secondary" | "success" | "danger";
};

type MessagePresentationOption = {
  label: string;
  value: string;
};
```

Example 2 (typescript):
```typescript
type ReplyPayloadDelivery = {
  pin?:
    | boolean
    | {
        enabled: boolean;
        notify?: boolean;
        required?: boolean;
      };
};
```

Example 3 (typescript):
```typescript
type ChannelPresentationCapabilities = {
  supported: boolean;
  buttons?: boolean;
  selects?: boolean;
  context?: boolean;
  divider?: boolean;
  tones?: MessagePresentationTone[];
};

type ChannelDeliveryCapabilities = {
  pinSentMessage?: boolean;
};

type ChannelOutboundAdapter = {
  presentationCapabilities?: ChannelPresentationCapabilities;

  renderPresentation?: (params: {
    payload: ReplyPayload;
    presentation: MessagePresentation;
    ctx: ChannelOutboundSendContext;
  }) => ReplyPayload | null;

  deliveryCapabilities?: ChannelDeliveryCapabilities;

  pinDeliveredMessage?: (params: {
    cfg: OpenClawConfig;
    accountId?: string | null;
    to: string;
    threadId?: string | number | null;
    messageId: string;
    notify: boolean;
  }) => Promise<void>;
};
```

---
