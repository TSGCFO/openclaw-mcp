# Openclaw-Mcp_Docs - Platforms

**Pages:** 23

---

## Gateway on macOS

**URL:** https://docs.openclaw.ai/platforms/mac/bundled-gateway

**Contents:**
- Gateway on macOS
- Documentation Index
- ​Install the CLI (required for local mode)
- ​Launchd (Gateway as LaunchAgent)
- ​Version compatibility
- ​Smoke check
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
npm install -g openclaw@<version>
```

Example 2 (sass):
```sass
openclaw --version

OPENCLAW_SKIP_CHANNELS=1 \
OPENCLAW_SKIP_CANVAS_HOST=1 \
openclaw gateway --port 18999 --bind loopback
```

Example 3 (json):
```json
openclaw gateway call health --url ws://127.0.0.1:18999 --timeout 3000
```

---

## iOS app

**URL:** https://docs.openclaw.ai/platforms/ios

**Contents:**
- iOS app
- Documentation Index
- ​What it does
- ​Requirements
- ​Quick start (pair + connect)
- ​Relay-backed push for official builds
- ​Background alive beacons
- ​Authentication and trust flow
- ​Discovery paths
  - ​Bonjour (LAN)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw gateway --port 18789
```

Example 2 (typescript):
```typescript
openclaw devices list
openclaw devices approve <requestId>
```

Example 3 (json):
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

Example 4 (unknown):
```unknown
openclaw nodes status
openclaw gateway call node.list --params "{}"
```

---

## Gateway lifecycle

**URL:** https://docs.openclaw.ai/platforms/mac/child-process

**Contents:**
- Gateway lifecycle
- Documentation Index
- ​Gateway lifecycle on macOS
- ​Default behavior (launchd)
- ​Unsigned dev builds
- ​Attach-only mode
- ​Remote mode
- ​Why we prefer launchd
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (bash):
```bash
launchctl kickstart -k gui/$UID/ai.openclaw.gateway
launchctl bootout gui/$UID/ai.openclaw.gateway
```

Example 2 (unknown):
```unknown
rm ~/.openclaw/disable-launchagent
```

---

## Platforms

**URL:** https://docs.openclaw.ai/platforms

**Contents:**
- Platforms
- Documentation Index
- ​Choose your OS
- ​VPS & hosting
- ​Common links
- ​Gateway service install (CLI)
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Android app

**URL:** https://docs.openclaw.ai/platforms/android

**Contents:**
- Android app
- Documentation Index
- ​Support snapshot
- ​System control
- ​Connection runbook
  - ​Prerequisites
  - ​1) Start the Gateway
  - ​2) Verify discovery (optional)
    - ​Tailnet (Vienna ⇄ London) discovery via unicast DNS-SD
  - ​3) Connect from Android

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw gateway --port 18789 --verbose
```

Example 2 (unknown):
```unknown
openclaw gateway --tailscale serve
```

Example 3 (unknown):
```unknown
dns-sd -B _openclaw-gw._tcp local.
```

Example 4 (unknown):
```unknown
openclaw gateway discover --json
```

---

## Menu bar

**URL:** https://docs.openclaw.ai/platforms/mac/menu-bar

**Contents:**
- Menu bar
- Documentation Index
- ​Menu Bar Status Logic
- ​What is shown
- ​State model
- ​IconState enum (Swift)
  - ​ActivityKind → glyph
  - ​Visual mapping
- ​Context submenu
- ​Status row text (menu)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## macOS signing

**URL:** https://docs.openclaw.ai/platforms/mac/signing

**Contents:**
- macOS signing
- Documentation Index
- ​mac signing (debug builds)
- ​Usage
  - ​Ad-hoc Signing Note
- ​Build metadata for About
- ​Why
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sass):
```sass
# from repo root
scripts/package-mac-app.sh               # auto-selects identity; errors if none found
SIGN_IDENTITY="Developer ID Application: Your Name" scripts/package-mac-app.sh   # real cert
ALLOW_ADHOC_SIGNING=1 scripts/package-mac-app.sh    # ad-hoc (permissions will not stick)
SIGN_IDENTITY="-" scripts/package-mac-app.sh        # explicit ad-hoc (same caveat)
DISABLE_LIBRARY_VALIDATION=1 scripts/package-mac-app.sh   # dev-only Sparkle Team ID mismatch workaround
```

---

## macOS dev setup

**URL:** https://docs.openclaw.ai/platforms/mac/dev-setup

**Contents:**
- macOS dev setup
- Documentation Index
- ​macOS developer setup
- ​Prerequisites
- ​1. Install Dependencies
- ​2. Build and Package the App
- ​3. Install the CLI
- ​Troubleshooting
  - ​Build fails: toolchain or SDK mismatch
  - ​App crashes on permission grant

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
pnpm install
```

Example 2 (unknown):
```unknown
./scripts/package-mac-app.sh
```

Example 3 (typescript):
```typescript
npm install -g openclaw@<version>
```

Example 4 (unknown):
```unknown
xcodebuild -version
xcrun swift --version
```

---

## WebChat (macOS)

**URL:** https://docs.openclaw.ai/platforms/mac/webchat

**Contents:**
- WebChat (macOS)
- Documentation Index
- ​Launch & debugging
- ​How it is wired
- ​Security surface
- ​Known limitations
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
dist/OpenClaw.app/Contents/MacOS/OpenClaw --webchat
```

---

## Windows

**URL:** https://docs.openclaw.ai/platforms/windows

**Contents:**
- Windows
- Documentation Index
- ​WSL2 (recommended)
- ​Native Windows status
- ​Gateway
- ​Gateway service install (CLI)
- ​Gateway auto-start before Windows login
  - ​1) Keep user services running without login
  - ​2) Install the OpenClaw gateway user service
  - ​3) Start WSL automatically at Windows boot

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw agent --local --agent main --thinking low -m "Reply with exactly WINDOWS-HATCH-OK."
```

Example 2 (unknown):
```unknown
openclaw onboard --non-interactive --skip-health
openclaw gateway run
```

Example 3 (unknown):
```unknown
openclaw gateway install
openclaw gateway status --json
```

Example 4 (unknown):
```unknown
openclaw onboard --install-daemon
```

---

## macOS app

**URL:** https://docs.openclaw.ai/platforms/macos

**Contents:**
- macOS app
- Documentation Index
- ​What it does
- ​Local vs remote mode
- ​Launchd control
- ​Node capabilities (mac)
- ​Exec approvals (system.run)
- ​Deep links
  - ​openclaw://agent
- ​Onboarding flow (typical)

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (bash):
```bash
launchctl kickstart -k gui/$UID/ai.openclaw.gateway
launchctl bootout gui/$UID/ai.openclaw.gateway
```

Example 2 (perl):
```perl
Gateway -> Node Service (WS)
                 |  IPC (UDS + token + HMAC + TTL)
                 v
             Mac App (UI + TCC + system.run)
```

Example 3 (unknown):
```unknown
~/.openclaw/exec-approvals.json
```

Example 4 (json):
```json
{
  "version": 1,
  "defaults": {
    "security": "deny",
    "ask": "on-miss"
  },
  "agents": {
    "main": {
      "security": "allowlist",
      "ask": "on-miss",
      "allowlist": [{ "pattern": "/opt/homebrew/bin/rg" }]
    }
  }
}
```

---

## Voice wake (macOS)

**URL:** https://docs.openclaw.ai/platforms/mac/voicewake

**Contents:**
- Voice wake (macOS)
- Documentation Index
- ​Voice Wake & Push-to-Talk
- ​Modes
- ​Runtime behavior (wake-word)
- ​Lifecycle invariants
- ​Sticky overlay failure mode (previous)
- ​Push-to-talk specifics
- ​User-facing settings
- ​Forwarding behavior

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Menu bar icon

**URL:** https://docs.openclaw.ai/platforms/mac/icon

**Contents:**
- Menu bar icon
- Documentation Index
- ​Menu Bar Icon States
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## Voice overlay

**URL:** https://docs.openclaw.ai/platforms/mac/voice-overlay

**Contents:**
- Voice overlay
- Documentation Index
- ​Voice Overlay Lifecycle (macOS)
- ​Current intent
- ​Implemented (Dec 9, 2025)
- ​Next steps
- ​Debugging checklist
- ​Migration steps (suggested)
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
sudo log stream --predicate 'subsystem == "ai.openclaw" AND category CONTAINS "voicewake"' --level info --style compact
```

---

## Health checks (macOS)

**URL:** https://docs.openclaw.ai/platforms/mac/health

**Contents:**
- Health checks (macOS)
- Documentation Index
- ​Health Checks on macOS
- ​Menu bar
- ​Settings
- ​How the probe works
- ​When in doubt
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

---

## macOS IPC

**URL:** https://docs.openclaw.ai/platforms/mac/xpc

**Contents:**
- macOS IPC
- Documentation Index
- ​OpenClaw macOS IPC architecture
- ​Goals
- ​How it works
  - ​Gateway + node transport
  - ​Node service + app IPC
  - ​PeekabooBridge (UI automation)
- ​Operational flows
- ​Hardening notes

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (perl):
```perl
Agent -> Gateway -> Node Service (WS)
                      |  IPC (UDS + token + HMAC + TTL)
                      v
                  Mac App (UI + TCC + system.run)
```

---

## Linux app

**URL:** https://docs.openclaw.ai/platforms/linux

**Contents:**
- Linux app
- Documentation Index
- ​Beginner quick path (VPS)
- ​Install
- ​Gateway
- ​Gateway service install (CLI)
- ​System control (systemd user unit)
- ​Memory pressure and OOM kills
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
openclaw onboard --install-daemon
```

Example 2 (unknown):
```unknown
openclaw gateway install
```

Example 3 (unknown):
```unknown
openclaw configure
```

Example 4 (unknown):
```unknown
openclaw doctor
```

---

## macOS permissions

**URL:** https://docs.openclaw.ai/platforms/mac/permissions

**Contents:**
- macOS permissions
- Documentation Index
- ​Requirements for stable permissions
- ​Recovery checklist when prompts disappear
- ​Files and folders permissions (Desktop/Documents/Downloads)
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
sudo tccutil reset Accessibility ai.openclaw.mac
sudo tccutil reset ScreenCapture ai.openclaw.mac
sudo tccutil reset AppleEvents
```

---

## Canvas

**URL:** https://docs.openclaw.ai/platforms/mac/canvas

**Contents:**
- Canvas
- Documentation Index
- ​Where Canvas lives
- ​Panel behavior
- ​Agent API surface
- ​A2UI in Canvas
  - ​A2UI commands (v0.8)
- ​Triggering agent runs from Canvas
- ​Security notes
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
openclaw nodes canvas present --node <id>
openclaw nodes canvas navigate --node <id> --url "/"
openclaw nodes canvas eval --node <id> --js "document.title"
openclaw nodes canvas snapshot --node <id>
```

Example 2 (yaml):
```yaml
http://<gateway-host>:18789/__openclaw__/a2ui/
```

Example 3 (json):
```json
cat > /tmp/a2ui-v0.8.jsonl <<'EOFA2'
{"surfaceUpdate":{"surfaceId":"main","components":[{"id":"root","component":{"Column":{"children":{"explicitList":["title","content"]}}}},{"id":"title","component":{"Text":{"text":{"literalString":"Canvas (A2UI v0.8)"},"usageHint":"h1"}}},{"id":"content","component":{"Text":{"text":{"literalString":"If you can read this, A2UI push works."},"usageHint":"body"}}}]}}
{"beginRendering":{"surfaceId":"main","root":"root"}}
EOFA2

openclaw nodes canvas a2ui push --jsonl /tmp/a2ui-v0.8.jsonl --node <id>
```

Example 4 (sql):
```sql
openclaw nodes canvas a2ui push --node <id> --text "Hello from A2UI"
```

---

## macOS logging

**URL:** https://docs.openclaw.ai/platforms/mac/logging

**Contents:**
- macOS logging
- Documentation Index
- ​Logging (macOS)
- ​Rolling diagnostics file log (Debug pane)
- ​Unified logging private data on macOS
- ​Enable for OpenClaw (ai.openclaw)
- ​Disable after debugging
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (xml):
```xml
cat <<'EOF' >/tmp/ai.openclaw.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>DEFAULT-OPTIONS</key>
    <dict>
        <key>Enable-Private-Data</key>
        <true/>
    </dict>
</dict>
</plist>
EOF
sudo install -m 644 -o root -g wheel /tmp/ai.openclaw.plist /Library/Preferences/Logging/Subsystems/ai.openclaw.plist
```

---

## Remote control

**URL:** https://docs.openclaw.ai/platforms/mac/remote

**Contents:**
- Remote control
- Documentation Index
- ​Remote OpenClaw (macOS ⇄ remote host)
- ​Modes
- ​Remote transports
- ​Prereqs on the remote host
- ​macOS app setup
- ​Web Chat
- ​Permissions
- ​Security notes

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (typescript):
```typescript
openclaw nodes notify --node <id> --title "Ping" --body "Remote gateway ready" --sound Glass
```

---

## Linux server

**URL:** https://docs.openclaw.ai/vps

**Contents:**
- Linux server
- Documentation Index
- ​Pick a provider
- Railway
- Northflank
- DigitalOcean
- Oracle Cloud
- Fly.io
- Hetzner
- Hostinger

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (sass):
```sass
grep -q 'NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache' ~/.bashrc || cat >> ~/.bashrc <<'EOF'
export NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache
mkdir -p /var/tmp/openclaw-compile-cache
export OPENCLAW_NO_RESPAWN=1
EOF
source ~/.bashrc
```

Example 2 (unknown):
```unknown
systemctl --user edit openclaw-gateway.service
```

Example 3 (sass):
```sass
[Service]
Environment=OPENCLAW_NO_RESPAWN=1
Environment=NODE_COMPILE_CACHE=/var/tmp/openclaw-compile-cache
Restart=always
RestartSec=2
TimeoutStartSec=90
```

---

## Peekaboo bridge

**URL:** https://docs.openclaw.ai/platforms/mac/peekaboo

**Contents:**
- Peekaboo bridge
- Documentation Index
- ​What this is (and is not)
- ​Relationship to Computer Use
- ​Enable the bridge
- ​Client discovery order
- ​Security & permissions
- ​Snapshot behavior (automation)
- ​Troubleshooting
- ​Related

Fetch the complete documentation index at: https://docs.openclaw.ai/llms.txt

Use this file to discover all available pages before exploring further.

**Examples:**

Example 1 (unknown):
```unknown
export PEEKABOO_BRIDGE_SOCKET=/path/to/bridge.sock
```

---
