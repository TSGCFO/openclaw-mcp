---
source_url: https://docs.openclaw.ai/platforms/macos
title: "macOS app - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/platforms/macos#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Platforms overview

macOS app

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [What it does](https://docs.openclaw.ai/platforms/macos#what-it-does)
- [Local vs remote mode](https://docs.openclaw.ai/platforms/macos#local-vs-remote-mode)
- [Launchd control](https://docs.openclaw.ai/platforms/macos#launchd-control)
- [Node capabilities (mac)](https://docs.openclaw.ai/platforms/macos#node-capabilities-mac)
- [Exec approvals (system.run)](https://docs.openclaw.ai/platforms/macos#exec-approvals-system-run)
- [Deep links](https://docs.openclaw.ai/platforms/macos#deep-links)
- [openclaw://agent](https://docs.openclaw.ai/platforms/macos#openclaw%2F%2Fagent)
- [Onboarding flow (typical)](https://docs.openclaw.ai/platforms/macos#onboarding-flow-typical)
- [State dir placement (macOS)](https://docs.openclaw.ai/platforms/macos#state-dir-placement-macos)
- [Build & dev workflow (native)](https://docs.openclaw.ai/platforms/macos#build-%26-dev-workflow-native)
- [Debug gateway connectivity (macOS CLI)](https://docs.openclaw.ai/platforms/macos#debug-gateway-connectivity-macos-cli)
- [Remote connection plumbing (SSH tunnels)](https://docs.openclaw.ai/platforms/macos#remote-connection-plumbing-ssh-tunnels)
- [Control tunnel (Gateway WebSocket port)](https://docs.openclaw.ai/platforms/macos#control-tunnel-gateway-websocket-port)
- [Related docs](https://docs.openclaw.ai/platforms/macos#related-docs)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

The macOS app is the **menu‑bar companion** for OpenClaw. It owns permissions,
manages/attaches to the Gateway locally (launchd or manual), and exposes macOS
capabilities to the agent as a node.

## [​](https://docs.openclaw.ai/platforms/macos\#what-it-does)  What it does

- Shows native notifications and status in the menu bar.
- Owns TCC prompts (Notifications, Accessibility, Screen Recording, Microphone,
Speech Recognition, Automation/AppleScript).
- Runs or connects to the Gateway (local or remote).
- Exposes macOS‑only tools (Canvas, Camera, Screen Recording, `system.run`).
- Starts the local node host service in **remote** mode (launchd), and stops it in **local** mode.
- Optionally hosts **PeekabooBridge** for UI automation.
- Installs the global CLI (`openclaw`) on request via npm, pnpm, or bun (the app prefers npm, then pnpm, then bun; Node remains the recommended Gateway runtime).

## [​](https://docs.openclaw.ai/platforms/macos\#local-vs-remote-mode)  Local vs remote mode

- **Local** (default): the app attaches to a running local Gateway if present;
otherwise it enables the launchd service via `openclaw gateway install`.
- **Remote**: the app connects to a Gateway over SSH/Tailscale and never starts
a local process.
The app starts the local **node host service** so the remote Gateway can reach this Mac.
The app does not spawn the Gateway as a child process.
Gateway discovery now prefers Tailscale MagicDNS names over raw tailnet IPs,
so the Mac app recovers more reliably when tailnet IPs change.

## [​](https://docs.openclaw.ai/platforms/macos\#launchd-control)  Launchd control

The app manages a per‑user LaunchAgent labeled `ai.openclaw.gateway`
(or `ai.openclaw.<profile>` when using `--profile`/`OPENCLAW_PROFILE`; legacy `com.openclaw.*` still unloads).

```
launchctl kickstart -k gui/$UID/ai.openclaw.gateway
launchctl bootout gui/$UID/ai.openclaw.gateway
```

Replace the label with `ai.openclaw.<profile>` when running a named profile.If the LaunchAgent isn’t installed, enable it from the app or run
`openclaw gateway install`.

## [​](https://docs.openclaw.ai/platforms/macos\#node-capabilities-mac)  Node capabilities (mac)

The macOS app presents itself as a node. Common commands:

- Canvas: `canvas.present`, `canvas.navigate`, `canvas.eval`, `canvas.snapshot`, `canvas.a2ui.*`
- Camera: `camera.snap`, `camera.clip`
- Screen: `screen.snapshot`, `screen.record`
- System: `system.run`, `system.notify`

The node reports a `permissions` map so agents can decide what’s allowed.Node service + app IPC:

- When the headless node host service is running (remote mode), it connects to the Gateway WS as a node.
- `system.run` executes in the macOS app (UI/TCC context) over a local Unix socket; prompts + output stay in-app.

Diagram (SCI):

```
Gateway -> Node Service (WS)
                 |  IPC (UDS + token + HMAC + TTL)
                 v
             Mac App (UI + TCC + system.run)
```

## [​](https://docs.openclaw.ai/platforms/macos\#exec-approvals-system-run)  Exec approvals (system.run)

`system.run` is controlled by **Exec approvals** in the macOS app (Settings → Exec approvals).
Security + ask + allowlist are stored locally on the Mac in:

```
~/.openclaw/exec-approvals.json
```

Example:

```
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

Notes:

- `allowlist` entries are glob patterns for resolved binary paths, or bare command names for PATH-invoked commands.
- Raw shell command text that contains shell control or expansion syntax (`&&`, `||`, `;`, `|`, `````, `$`, `<`, `>`, `(`, `)`) is treated as an allowlist miss and requires explicit approval (or allowlisting the shell binary).
- Choosing “Always Allow” in the prompt adds that command to the allowlist.
- `system.run` environment overrides are filtered (drops `PATH`, `DYLD_*`, `LD_*`, `NODE_OPTIONS`, `PYTHON*`, `PERL*`, `RUBYOPT`, `SHELLOPTS`, `PS4`) and then merged with the app’s environment.
- For shell wrappers (`bash|sh|zsh ... -c/-lc`), request-scoped environment overrides are reduced to a small explicit allowlist (`TERM`, `LANG`, `LC_*`, `COLORTERM`, `NO_COLOR`, `FORCE_COLOR`).
- For allow-always decisions in allowlist mode, known dispatch wrappers (`env`, `nice`, `nohup`, `stdbuf`, `timeout`) persist inner executable paths instead of wrapper paths. If unwrapping is not safe, no allowlist entry is persisted automatically.

## [​](https://docs.openclaw.ai/platforms/macos\#deep-links)  Deep links

The app registers the `openclaw://` URL scheme for local actions.

### [​](https://docs.openclaw.ai/platforms/macos\#openclaw//agent)  `openclaw://agent`

Triggers a Gateway `agent` request.

```
open 'openclaw://agent?message=Hello%20from%20deep%20link'
```

Query parameters:

- `message` (required)
- `sessionKey` (optional)
- `thinking` (optional)
- `deliver` / `to` / `channel` (optional)
- `timeoutSeconds` (optional)
- `key` (optional unattended mode key)

Safety:

- Without `key`, the app prompts for confirmation.
- Without `key`, the app enforces a short message limit for the confirmation prompt and ignores `deliver` / `to` / `channel`.
- With a valid `key`, the run is unattended (intended for personal automations).

## [​](https://docs.openclaw.ai/platforms/macos\#onboarding-flow-typical)  Onboarding flow (typical)

1. Install and launch **OpenClaw.app**.
2. Complete the permissions checklist (TCC prompts).
3. Ensure **Local** mode is active and the Gateway is running.
4. Install the CLI if you want terminal access.

## [​](https://docs.openclaw.ai/platforms/macos\#state-dir-placement-macos)  State dir placement (macOS)

Avoid putting your OpenClaw state dir in iCloud or other cloud-synced folders.
Sync-backed paths can add latency and occasionally cause file-lock/sync races for
sessions and credentials.Prefer a local non-synced state path such as:

```
OPENCLAW_STATE_DIR=~/.openclaw
```

If `openclaw doctor` detects state under:

- `~/Library/Mobile Documents/com~apple~CloudDocs/...`
- `~/Library/CloudStorage/...`

it will warn and recommend moving back to a local path.

## [​](https://docs.openclaw.ai/platforms/macos\#build-&-dev-workflow-native)  Build & dev workflow (native)

- `cd apps/macos && swift build`
- `swift run OpenClaw` (or Xcode)
- Package app: `scripts/package-mac-app.sh`

## [​](https://docs.openclaw.ai/platforms/macos\#debug-gateway-connectivity-macos-cli)  Debug gateway connectivity (macOS CLI)

Use the debug CLI to exercise the same Gateway WebSocket handshake and discovery
logic that the macOS app uses, without launching the app.

```
cd apps/macos
swift run openclaw-mac connect --json
swift run openclaw-mac discover --timeout 3000 --json
```

Connect options:

- `--url <ws://host:port>`: override config
- `--mode <local|remote>`: resolve from config (default: config or local)
- `--probe`: force a fresh health probe
- `--timeout <ms>`: request timeout (default: `15000`)
- `--json`: structured output for diffing

Discovery options:

- `--include-local`: include gateways that would be filtered as “local”
- `--timeout <ms>`: overall discovery window (default: `2000`)
- `--json`: structured output for diffing

Compare against `openclaw gateway discover --json` to see whether the macOS app’s discovery pipeline (`local.` plus the configured wide-area domain, with wide-area and Tailscale Serve fallbacks) differs from the Node CLI’s `dns-sd` based discovery.

## [​](https://docs.openclaw.ai/platforms/macos\#remote-connection-plumbing-ssh-tunnels)  Remote connection plumbing (SSH tunnels)

When the macOS app runs in **Remote** mode, it opens an SSH tunnel so local UI
components can talk to a remote Gateway as if it were on localhost.

### [​](https://docs.openclaw.ai/platforms/macos\#control-tunnel-gateway-websocket-port)  Control tunnel (Gateway WebSocket port)

- **Purpose:** health checks, status, Web Chat, config, and other control-plane calls.
- **Local port:** the Gateway port (default `18789`), always stable.
- **Remote port:** the same Gateway port on the remote host.
- **Behavior:** no random local port; the app reuses an existing healthy tunnel
or restarts it if needed.
- **SSH shape:**`ssh -N -L <local>:127.0.0.1:<remote>` with BatchMode +
ExitOnForwardFailure + keepalive options.
- **IP reporting:** the SSH tunnel uses loopback, so the gateway will see the node
IP as `127.0.0.1`. Use **Direct (ws/wss)** transport if you want the real client
IP to appear (see [macOS remote access](https://docs.openclaw.ai/platforms/mac/remote)).

For setup steps, see [macOS remote access](https://docs.openclaw.ai/platforms/mac/remote). For protocol
details, see [Gateway protocol](https://docs.openclaw.ai/gateway/protocol).

## [​](https://docs.openclaw.ai/platforms/macos\#related-docs)  Related docs

- [Gateway runbook](https://docs.openclaw.ai/gateway)
- [Gateway (macOS)](https://docs.openclaw.ai/platforms/mac/bundled-gateway)
- [macOS permissions](https://docs.openclaw.ai/platforms/mac/permissions)
- [Canvas](https://docs.openclaw.ai/platforms/mac/canvas)

[Platforms](https://docs.openclaw.ai/platforms) [Linux app](https://docs.openclaw.ai/platforms/linux)

Ctrl+I