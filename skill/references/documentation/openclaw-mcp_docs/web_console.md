# Openclaw-Mcp_Docs - Web Console

**Pages:** 4

---

## Dashboard

**URL:** https://docs.openclaw.ai/web/dashboard

**Contents:**
- Dashboard
- Documentation Index
- ​Fast path (recommended)
- ​Auth basics (local vs remote)
- ​If you see “unauthorized” / 1008
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Web

**URL:** https://docs.openclaw.ai/web

**Contents:**
- Web
- Documentation Index
- ​Webhooks
- ​Config (default-on)
- ​Tailscale access
  - ​Integrated Serve (recommended)
  - ​Tailnet bind + token
  - ​Public internet (Funnel)
- ​Security notes
- ​Building the UI

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (json):
```json
{
  gateway: {
    controlUi: { enabled: true, basePath: "/openclaw" }, // basePath optional
  },
}
```

Example 2 (json):
```json
{
  gateway: {
    bind: "loopback",
    tailscale: { mode: "serve" },
  },
}
```

Example 3 (unknown):
```unknown
openclaw gateway
```

Example 4 (json):
```json
{
  gateway: {
    bind: "tailnet",
    controlUi: { enabled: true },
    auth: { mode: "token", token: "your-token" },
  },
}
```

---

## WebChat

**URL:** https://docs.openclaw.ai/web/webchat

**Contents:**
- WebChat
- Documentation Index
- ​What it is
- ​Quick start
- ​How it works (behavior)
- ​Control UI agents tools panel
- ​Remote use
- ​Configuration reference (WebChat)
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## TUI

**URL:** https://docs.openclaw.ai/web/tui

**Contents:**
- TUI
- Documentation Index
- ​Quick start
  - ​Gateway mode
  - ​Local mode
- ​What you see
- ​Mental model: agents + sessions
- ​Sending + delivery
- ​Pickers + overlays
- ​Keyboard shortcuts

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw gateway
```

Example 2 (unknown):
```unknown
openclaw tui
```

Example 3 (typescript):
```typescript
openclaw tui --url ws://<host>:<port> --token <gateway-token>
```

Example 4 (markdown):
```markdown
openclaw chat
# or
openclaw tui --local
```

---
