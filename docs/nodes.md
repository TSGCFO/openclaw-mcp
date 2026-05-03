---
source_url: https://docs.openclaw.ai/nodes
title: "Nodes - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/nodes#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Nodes and media

Nodes

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Pairing + status](https://docs.openclaw.ai/nodes#pairing-%2B-status)
- [Remote node host (system.run)](https://docs.openclaw.ai/nodes#remote-node-host-system-run)
- [What runs where](https://docs.openclaw.ai/nodes#what-runs-where)
- [Start a node host (foreground)](https://docs.openclaw.ai/nodes#start-a-node-host-foreground)
- [Remote gateway via SSH tunnel (loopback bind)](https://docs.openclaw.ai/nodes#remote-gateway-via-ssh-tunnel-loopback-bind)
- [Start a node host (service)](https://docs.openclaw.ai/nodes#start-a-node-host-service)
- [Pair + name](https://docs.openclaw.ai/nodes#pair-%2B-name)
- [Allowlist the commands](https://docs.openclaw.ai/nodes#allowlist-the-commands)
- [Point exec at the node](https://docs.openclaw.ai/nodes#point-exec-at-the-node)
- [Invoking commands](https://docs.openclaw.ai/nodes#invoking-commands)
- [Command policy](https://docs.openclaw.ai/nodes#command-policy)
- [Screenshots (canvas snapshots)](https://docs.openclaw.ai/nodes#screenshots-canvas-snapshots)
- [Canvas controls](https://docs.openclaw.ai/nodes#canvas-controls)
- [A2UI (Canvas)](https://docs.openclaw.ai/nodes#a2ui-canvas)
- [Photos + videos (node camera)](https://docs.openclaw.ai/nodes#photos-%2B-videos-node-camera)
- [Screen recordings (nodes)](https://docs.openclaw.ai/nodes#screen-recordings-nodes)
- [Location (nodes)](https://docs.openclaw.ai/nodes#location-nodes)
- [SMS (Android nodes)](https://docs.openclaw.ai/nodes#sms-android-nodes)
- [Android device + personal data commands](https://docs.openclaw.ai/nodes#android-device-%2B-personal-data-commands)
- [System commands (node host / mac node)](https://docs.openclaw.ai/nodes#system-commands-node-host-%2F-mac-node)
- [Exec node binding](https://docs.openclaw.ai/nodes#exec-node-binding)
- [Permissions map](https://docs.openclaw.ai/nodes#permissions-map)
- [Headless node host (cross-platform)](https://docs.openclaw.ai/nodes#headless-node-host-cross-platform)
- [Mac node mode](https://docs.openclaw.ai/nodes#mac-node-mode)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

A **node** is a companion device (macOS/iOS/Android/headless) that connects to the Gateway **WebSocket** (same port as operators) with `role: "node"` and exposes a command surface (e.g. `canvas.*`, `camera.*`, `device.*`, `notifications.*`, `system.*`) via `node.invoke`. Protocol details: [Gateway protocol](https://docs.openclaw.ai/gateway/protocol).Legacy transport: [Bridge protocol](https://docs.openclaw.ai/gateway/bridge-protocol) (TCP JSONL;
historical only for current nodes).macOS can also run in **node mode**: the menubar app connects to the Gateway’s
WS server and exposes its local canvas/camera commands as a node (so
`openclaw nodes …` works against this Mac). In remote gateway mode, browser
automation is handled by the CLI node host (`openclaw node run` or the
installed node service), not by the native app node.Notes:

- Nodes are **peripherals**, not gateways. They don’t run the gateway service.
- Telegram/WhatsApp/etc. messages land on the **gateway**, not on nodes.
- Troubleshooting runbook: [/nodes/troubleshooting](https://docs.openclaw.ai/nodes/troubleshooting)

## [​](https://docs.openclaw.ai/nodes\#pairing-+-status)  Pairing + status

**WS nodes use device pairing.** Nodes present a device identity during `connect`; the Gateway
creates a device pairing request for `role: node`. Approve via the devices CLI (or UI).Quick CLI:

```
openclaw devices list
openclaw devices approve <requestId>
openclaw devices reject <requestId>
openclaw nodes status
openclaw nodes describe --node <idOrNameOrIp>
```

If a node retries with changed auth details (role/scopes/public key), the prior
pending request is superseded and a new `requestId` is created. Re-run
`openclaw devices list` before approving.Notes:

- `nodes status` marks a node as **paired** when its device pairing role includes `node`.
- The device pairing record is the durable approved-role contract. Token
rotation stays inside that contract; it cannot upgrade a paired node into a
different role that pairing approval never granted.
- `node.pair.*` (CLI: `openclaw nodes pending/approve/reject/remove/rename`) is a separate gateway-owned
node pairing store; it does **not** gate the WS `connect` handshake.
- `openclaw nodes remove --node <id|name|ip>` deletes stale entries from that
separate gateway-owned node pairing store.
- Approval scope follows the pending request’s declared commands:
  - commandless request: `operator.pairing`
  - non-exec node commands: `operator.pairing` \+ `operator.write`
  - `system.run` / `system.run.prepare` / `system.which`: `operator.pairing` \+ `operator.admin`

## [​](https://docs.openclaw.ai/nodes\#remote-node-host-system-run)  Remote node host (system.run)

Use a **node host** when your Gateway runs on one machine and you want commands
to execute on another. The model still talks to the **gateway**; the gateway
forwards `exec` calls to the **node host** when `host=node` is selected.

### [​](https://docs.openclaw.ai/nodes\#what-runs-where)  What runs where

- **Gateway host**: receives messages, runs the model, routes tool calls.
- **Node host**: executes `system.run`/`system.which` on the node machine.
- **Approvals**: enforced on the node host via `~/.openclaw/exec-approvals.json`.

Approval note:

- Approval-backed node runs bind exact request context.
- For direct shell/runtime file executions, OpenClaw also best-effort binds one concrete local
file operand and denies the run if that file changes before execution.
- If OpenClaw cannot identify exactly one concrete local file for an interpreter/runtime command,
approval-backed execution is denied instead of pretending full runtime coverage. Use sandboxing,
separate hosts, or an explicit trusted allowlist/full workflow for broader interpreter semantics.

### [​](https://docs.openclaw.ai/nodes\#start-a-node-host-foreground)  Start a node host (foreground)

On the node machine:

```
openclaw node run --host <gateway-host> --port 18789 --display-name "Build Node"
```

### [​](https://docs.openclaw.ai/nodes\#remote-gateway-via-ssh-tunnel-loopback-bind)  Remote gateway via SSH tunnel (loopback bind)

If the Gateway binds to loopback (`gateway.bind=loopback`, default in local mode),
remote node hosts cannot connect directly. Create an SSH tunnel and point the
node host at the local end of the tunnel.Example (node host -> gateway host):

```
# Terminal A (keep running): forward local 18790 -> gateway 127.0.0.1:18789
ssh -N -L 18790:127.0.0.1:18789 user@gateway-host

# Terminal B: export the gateway token and connect through the tunnel
export OPENCLAW_GATEWAY_TOKEN="<gateway-token>"
openclaw node run --host 127.0.0.1 --port 18790 --display-name "Build Node"
```

Notes:

- `openclaw node run` supports token or password auth.
- Env vars are preferred: `OPENCLAW_GATEWAY_TOKEN` / `OPENCLAW_GATEWAY_PASSWORD`.
- Config fallback is `gateway.auth.token` / `gateway.auth.password`.
- In local mode, node host intentionally ignores `gateway.remote.token` / `gateway.remote.password`.
- In remote mode, `gateway.remote.token` / `gateway.remote.password` are eligible per remote precedence rules.
- If active local `gateway.auth.*` SecretRefs are configured but unresolved, node-host auth fails closed.
- Node-host auth resolution only honors `OPENCLAW_GATEWAY_*` env vars.

### [​](https://docs.openclaw.ai/nodes\#start-a-node-host-service)  Start a node host (service)

```
openclaw node install --host <gateway-host> --port 18789 --display-name "Build Node"
openclaw node start
openclaw node restart
```

### [​](https://docs.openclaw.ai/nodes\#pair-+-name)  Pair + name

On the gateway host:

```
openclaw devices list
openclaw devices approve <requestId>
openclaw nodes status
```

If the node retries with changed auth details, re-run `openclaw devices list`
and approve the current `requestId`.Naming options:

- `--display-name` on `openclaw node run` / `openclaw node install` (persists in `~/.openclaw/node.json` on the node).
- `openclaw nodes rename --node <id|name|ip> --name "Build Node"` (gateway override).

### [​](https://docs.openclaw.ai/nodes\#allowlist-the-commands)  Allowlist the commands

Exec approvals are **per node host**. Add allowlist entries from the gateway:

```
openclaw approvals allowlist add --node <id|name|ip> "/usr/bin/uname"
openclaw approvals allowlist add --node <id|name|ip> "/usr/bin/sw_vers"
```

Approvals live on the node host at `~/.openclaw/exec-approvals.json`.

### [​](https://docs.openclaw.ai/nodes\#point-exec-at-the-node)  Point exec at the node

Configure defaults (gateway config):

```
openclaw config set tools.exec.host node
openclaw config set tools.exec.security allowlist
openclaw config set tools.exec.node "<id-or-name>"
```

Or per session:

```
/exec host=node security=allowlist node=<id-or-name>
```

Once set, any `exec` call with `host=node` runs on the node host (subject to the
node allowlist/approvals).`host=auto` will not implicitly choose the node on its own, but an explicit per-call `host=node` request is allowed from `auto`. If you want node exec to be the default for the session, set `tools.exec.host=node` or `/exec host=node ...` explicitly.Related:

- [Node host CLI](https://docs.openclaw.ai/cli/node)
- [Exec tool](https://docs.openclaw.ai/tools/exec)
- [Exec approvals](https://docs.openclaw.ai/tools/exec-approvals)

## [​](https://docs.openclaw.ai/nodes\#invoking-commands)  Invoking commands

Low-level (raw RPC):

```
openclaw nodes invoke --node <idOrNameOrIp> --command canvas.eval --params '{"javaScript":"location.href"}'
```

Higher-level helpers exist for the common “give the agent a MEDIA attachment” workflows.

## [​](https://docs.openclaw.ai/nodes\#command-policy)  Command policy

Node commands must pass two gates before they can be invoked:

1. The node must declare the command in its WebSocket `connect.commands` list.
2. The gateway’s platform policy must allow the declared command.

Windows and macOS companion nodes allow safe declared commands such as
`canvas.*`, `camera.list`, `location.get`, and `screen.snapshot` by default.
Dangerous or privacy-heavy commands such as `camera.snap`, `camera.clip`, and
`screen.record` still require explicit opt-in with
`gateway.nodes.allowCommands`. `gateway.nodes.denyCommands` always wins over
defaults and extra allowlist entries.Plugin-owned node commands can add a Gateway node-invoke policy. That policy
runs after the allowlist check and before forwarding to the node, so raw
`node.invoke`, CLI helpers, and dedicated agent tools share the same plugin
permission boundary. Dangerous plugin node commands still require explicit
`gateway.nodes.allowCommands` opt-in.After a node changes its declared command list, reject the old device pairing
and approve the new request so the gateway stores the updated command snapshot.

## [​](https://docs.openclaw.ai/nodes\#screenshots-canvas-snapshots)  Screenshots (canvas snapshots)

If the node is showing the Canvas (WebView), `canvas.snapshot` returns `{ format, base64 }`.CLI helper (writes to a temp file and prints `MEDIA:<path>`):

```
openclaw nodes canvas snapshot --node <idOrNameOrIp> --format png
openclaw nodes canvas snapshot --node <idOrNameOrIp> --format jpg --max-width 1200 --quality 0.9
```

### [​](https://docs.openclaw.ai/nodes\#canvas-controls)  Canvas controls

```
openclaw nodes canvas present --node <idOrNameOrIp> --target https://example.com
openclaw nodes canvas hide --node <idOrNameOrIp>
openclaw nodes canvas navigate https://example.com --node <idOrNameOrIp>
openclaw nodes canvas eval --node <idOrNameOrIp> --js "document.title"
```

Notes:

- `canvas present` accepts URLs or local file paths (`--target`), plus optional `--x/--y/--width/--height` for positioning.
- `canvas eval` accepts inline JS (`--js`) or a positional arg.

### [​](https://docs.openclaw.ai/nodes\#a2ui-canvas)  A2UI (Canvas)

```
openclaw nodes canvas a2ui push --node <idOrNameOrIp> --text "Hello"
openclaw nodes canvas a2ui push --node <idOrNameOrIp> --jsonl ./payload.jsonl
openclaw nodes canvas a2ui reset --node <idOrNameOrIp>
```

Notes:

- Only A2UI v0.8 JSONL is supported (v0.9/createSurface is rejected).

## [​](https://docs.openclaw.ai/nodes\#photos-+-videos-node-camera)  Photos + videos (node camera)

Photos (`jpg`):

```
openclaw nodes camera list --node <idOrNameOrIp>
openclaw nodes camera snap --node <idOrNameOrIp>            # default: both facings (2 MEDIA lines)
openclaw nodes camera snap --node <idOrNameOrIp> --facing front
```

Video clips (`mp4`):

```
openclaw nodes camera clip --node <idOrNameOrIp> --duration 10s
openclaw nodes camera clip --node <idOrNameOrIp> --duration 3000 --no-audio
```

Notes:

- The node must be **foregrounded** for `canvas.*` and `camera.*` (background calls return `NODE_BACKGROUND_UNAVAILABLE`).
- Clip duration is clamped (currently `<= 60s`) to avoid oversized base64 payloads.
- Android will prompt for `CAMERA`/`RECORD_AUDIO` permissions when possible; denied permissions fail with `*_PERMISSION_REQUIRED`.

## [​](https://docs.openclaw.ai/nodes\#screen-recordings-nodes)  Screen recordings (nodes)

Supported nodes expose `screen.record` (mp4). Example:

```
openclaw nodes screen record --node <idOrNameOrIp> --duration 10s --fps 10
openclaw nodes screen record --node <idOrNameOrIp> --duration 10s --fps 10 --no-audio
```

Notes:

- `screen.record` availability depends on node platform.
- Screen recordings are clamped to `<= 60s`.
- `--no-audio` disables microphone capture on supported platforms.
- Use `--screen <index>` to select a display when multiple screens are available.

## [​](https://docs.openclaw.ai/nodes\#location-nodes)  Location (nodes)

Nodes expose `location.get` when Location is enabled in settings.CLI helper:

```
openclaw nodes location get --node <idOrNameOrIp>
openclaw nodes location get --node <idOrNameOrIp> --accuracy precise --max-age 15000 --location-timeout 10000
```

Notes:

- Location is **off by default**.
- “Always” requires system permission; background fetch is best-effort.
- The response includes lat/lon, accuracy (meters), and timestamp.

## [​](https://docs.openclaw.ai/nodes\#sms-android-nodes)  SMS (Android nodes)

Android nodes can expose `sms.send` when the user grants **SMS** permission and the device supports telephony.Low-level invoke:

```
openclaw nodes invoke --node <idOrNameOrIp> --command sms.send --params '{"to":"+15555550123","message":"Hello from OpenClaw"}'
```

Notes:

- The permission prompt must be accepted on the Android device before the capability is advertised.
- Wi-Fi-only devices without telephony will not advertise `sms.send`.

## [​](https://docs.openclaw.ai/nodes\#android-device-+-personal-data-commands)  Android device + personal data commands

Android nodes can advertise additional command families when the corresponding capabilities are enabled.Available families:

- `device.status`, `device.info`, `device.permissions`, `device.health`
- `notifications.list`, `notifications.actions`
- `photos.latest`
- `contacts.search`, `contacts.add`
- `calendar.events`, `calendar.add`
- `callLog.search`
- `sms.search`
- `motion.activity`, `motion.pedometer`

Example invokes:

```
openclaw nodes invoke --node <idOrNameOrIp> --command device.status --params '{}'
openclaw nodes invoke --node <idOrNameOrIp> --command notifications.list --params '{}'
openclaw nodes invoke --node <idOrNameOrIp> --command photos.latest --params '{"limit":1}'
```

Notes:

- Motion commands are capability-gated by available sensors.

## [​](https://docs.openclaw.ai/nodes\#system-commands-node-host-/-mac-node)  System commands (node host / mac node)

The macOS node exposes `system.run`, `system.notify`, and `system.execApprovals.get/set`.
The headless node host exposes `system.run`, `system.which`, and `system.execApprovals.get/set`.Examples:

```
openclaw nodes notify --node <idOrNameOrIp> --title "Ping" --body "Gateway ready"
openclaw nodes invoke --node <idOrNameOrIp> --command system.which --params '{"name":"git"}'
```

Notes:

- `system.run` returns stdout/stderr/exit code in the payload.
- Shell execution now goes through the `exec` tool with `host=node`; `nodes` remains the direct-RPC surface for explicit node commands.
- `nodes invoke` does not expose `system.run` or `system.run.prepare`; those stay on the exec path only.
- The exec path prepares a canonical `systemRunPlan` before approval. Once an
approval is granted, the gateway forwards that stored plan, not any later
caller-edited command/cwd/session fields.
- `system.notify` respects notification permission state on the macOS app.
- Unrecognized node `platform` / `deviceFamily` metadata uses a conservative default allowlist that excludes `system.run` and `system.which`. If you intentionally need those commands for an unknown platform, add them explicitly via `gateway.nodes.allowCommands`.
- `system.run` supports `--cwd`, `--env KEY=VAL`, `--command-timeout`, and `--needs-screen-recording`.
- For shell wrappers (`bash|sh|zsh ... -c/-lc`), request-scoped `--env` values are reduced to an explicit allowlist (`TERM`, `LANG`, `LC_*`, `COLORTERM`, `NO_COLOR`, `FORCE_COLOR`).
- For allow-always decisions in allowlist mode, known dispatch wrappers (`env`, `nice`, `nohup`, `stdbuf`, `timeout`) persist inner executable paths instead of wrapper paths. If unwrapping is not safe, no allowlist entry is persisted automatically.
- On Windows node hosts in allowlist mode, shell-wrapper runs via `cmd.exe /c` require approval (allowlist entry alone does not auto-allow the wrapper form).
- `system.notify` supports `--priority <passive|active|timeSensitive>` and `--delivery <system|overlay|auto>`.
- Node hosts ignore `PATH` overrides and strip dangerous startup/shell keys (`DYLD_*`, `LD_*`, `NODE_OPTIONS`, `PYTHON*`, `PERL*`, `RUBYOPT`, `SHELLOPTS`, `PS4`). If you need extra PATH entries, configure the node host service environment (or install tools in standard locations) instead of passing `PATH` via `--env`.
- On macOS node mode, `system.run` is gated by exec approvals in the macOS app (Settings → Exec approvals).
Ask/allowlist/full behave the same as the headless node host; denied prompts return `SYSTEM_RUN_DENIED`.
- On headless node host, `system.run` is gated by exec approvals (`~/.openclaw/exec-approvals.json`).

## [​](https://docs.openclaw.ai/nodes\#exec-node-binding)  Exec node binding

When multiple nodes are available, you can bind exec to a specific node.
This sets the default node for `exec host=node` (and can be overridden per agent).Global default:

```
openclaw config set tools.exec.node "node-id-or-name"
```

Per-agent override:

```
openclaw config get agents.list
openclaw config set agents.list[0].tools.exec.node "node-id-or-name"
```

Unset to allow any node:

```
openclaw config unset tools.exec.node
openclaw config unset agents.list[0].tools.exec.node
```

## [​](https://docs.openclaw.ai/nodes\#permissions-map)  Permissions map

Nodes may include a `permissions` map in `node.list` / `node.describe`, keyed by permission name (e.g. `screenRecording`, `accessibility`) with boolean values (`true` = granted).

## [​](https://docs.openclaw.ai/nodes\#headless-node-host-cross-platform)  Headless node host (cross-platform)

OpenClaw can run a **headless node host** (no UI) that connects to the Gateway
WebSocket and exposes `system.run` / `system.which`. This is useful on Linux/Windows
or for running a minimal node alongside a server.Start it:

```
openclaw node run --host <gateway-host> --port 18789
```

Notes:

- Pairing is still required (the Gateway will show a device pairing prompt).
- The node host stores its node id, token, display name, and gateway connection info in `~/.openclaw/node.json`.
- Exec approvals are enforced locally via `~/.openclaw/exec-approvals.json`
(see [Exec approvals](https://docs.openclaw.ai/tools/exec-approvals)).
- On macOS, the headless node host executes `system.run` locally by default. Set
`OPENCLAW_NODE_EXEC_HOST=app` to route `system.run` through the companion app exec host; add
`OPENCLAW_NODE_EXEC_FALLBACK=0` to require the app host and fail closed if it is unavailable.
- Add `--tls` / `--tls-fingerprint` when the Gateway WS uses TLS.

## [​](https://docs.openclaw.ai/nodes\#mac-node-mode)  Mac node mode

- The macOS menubar app connects to the Gateway WS server as a node (so `openclaw nodes …` works against this Mac).
- In remote mode, the app opens an SSH tunnel for the Gateway port and connects to `localhost`.

[Contributing to the threat model](https://docs.openclaw.ai/security/CONTRIBUTING-THREAT-MODEL) [Node troubleshooting](https://docs.openclaw.ai/nodes/troubleshooting)

Ctrl+I