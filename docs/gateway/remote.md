---
source_url: https://docs.openclaw.ai/gateway/remote
title: "Remote access - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/gateway/remote#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Remote access

Remote access

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [The core idea](https://docs.openclaw.ai/gateway/remote#the-core-idea)
- [Common VPN and tailnet setups](https://docs.openclaw.ai/gateway/remote#common-vpn-and-tailnet-setups)
- [Always-on Gateway in your tailnet](https://docs.openclaw.ai/gateway/remote#always-on-gateway-in-your-tailnet)
- [Home desktop runs the Gateway](https://docs.openclaw.ai/gateway/remote#home-desktop-runs-the-gateway)
- [Laptop runs the Gateway](https://docs.openclaw.ai/gateway/remote#laptop-runs-the-gateway)
- [Command flow (what runs where)](https://docs.openclaw.ai/gateway/remote#command-flow-what-runs-where)
- [SSH tunnel (CLI + tools)](https://docs.openclaw.ai/gateway/remote#ssh-tunnel-cli-%2B-tools)
- [CLI remote defaults](https://docs.openclaw.ai/gateway/remote#cli-remote-defaults)
- [Credential precedence](https://docs.openclaw.ai/gateway/remote#credential-precedence)
- [Chat UI over SSH](https://docs.openclaw.ai/gateway/remote#chat-ui-over-ssh)
- [macOS app Remote over SSH](https://docs.openclaw.ai/gateway/remote#macos-app-remote-over-ssh)
- [Security rules (remote/VPN)](https://docs.openclaw.ai/gateway/remote#security-rules-remote%2Fvpn)
- [macOS: persistent SSH tunnel via LaunchAgent](https://docs.openclaw.ai/gateway/remote#macos-persistent-ssh-tunnel-via-launchagent)
- [Step 1: add SSH config](https://docs.openclaw.ai/gateway/remote#step-1-add-ssh-config)
- [Step 2: copy SSH key (one-time)](https://docs.openclaw.ai/gateway/remote#step-2-copy-ssh-key-one-time)
- [Step 3: configure the gateway token](https://docs.openclaw.ai/gateway/remote#step-3-configure-the-gateway-token)
- [Step 4: create the LaunchAgent](https://docs.openclaw.ai/gateway/remote#step-4-create-the-launchagent)
- [Step 5: load the LaunchAgent](https://docs.openclaw.ai/gateway/remote#step-5-load-the-launchagent)
- [Troubleshooting](https://docs.openclaw.ai/gateway/remote#troubleshooting)
- [Related](https://docs.openclaw.ai/gateway/remote#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

This repo supports “remote over SSH” by keeping a single Gateway (the master) running on a dedicated host (desktop/server) and connecting clients to it.

- For **operators (you / the macOS app)**: SSH tunneling is the universal fallback.
- For **nodes (iOS/Android and future devices)**: connect to the Gateway **WebSocket** (LAN/tailnet or SSH tunnel as needed).

## [​](https://docs.openclaw.ai/gateway/remote\#the-core-idea)  The core idea

- The Gateway WebSocket binds to **loopback** on your configured port (defaults to 18789).
- For remote use, you forward that loopback port over SSH (or use a tailnet/VPN and tunnel less).

## [​](https://docs.openclaw.ai/gateway/remote\#common-vpn-and-tailnet-setups)  Common VPN and tailnet setups

Think of the **Gateway host** as where the agent lives. It owns sessions, auth profiles, channels, and state. Your laptop, desktop, and nodes connect to that host.

### [​](https://docs.openclaw.ai/gateway/remote\#always-on-gateway-in-your-tailnet)  Always-on Gateway in your tailnet

Run the Gateway on a persistent host (VPS or home server) and reach it via **Tailscale** or SSH.

- **Best UX:** keep `gateway.bind: "loopback"` and use **Tailscale Serve** for the Control UI.
- **Fallback:** keep loopback plus SSH tunnel from any machine that needs access.
- **Examples:** [exe.dev](https://docs.openclaw.ai/install/exe-dev) (easy VM) or [Hetzner](https://docs.openclaw.ai/install/hetzner) (production VPS).

Ideal when your laptop sleeps often but you want the agent always-on.

### [​](https://docs.openclaw.ai/gateway/remote\#home-desktop-runs-the-gateway)  Home desktop runs the Gateway

The laptop does **not** run the agent. It connects remotely:

- Use the macOS app’s **Remote over SSH** mode (Settings → General → OpenClaw runs).
- The app opens and manages the tunnel, so WebChat and health checks just work.

Runbook: [macOS remote access](https://docs.openclaw.ai/platforms/mac/remote).

### [​](https://docs.openclaw.ai/gateway/remote\#laptop-runs-the-gateway)  Laptop runs the Gateway

Keep the Gateway local but expose it safely:

- SSH tunnel to the laptop from other machines, or
- Tailscale Serve the Control UI and keep the Gateway loopback-only.

Guides: [Tailscale](https://docs.openclaw.ai/gateway/tailscale) and [Web overview](https://docs.openclaw.ai/web).

## [​](https://docs.openclaw.ai/gateway/remote\#command-flow-what-runs-where)  Command flow (what runs where)

One gateway service owns state + channels. Nodes are peripherals.Flow example (Telegram → node):

- Telegram message arrives at the **Gateway**.
- Gateway runs the **agent** and decides whether to call a node tool.
- Gateway calls the **node** over the Gateway WebSocket (`node.*` RPC).
- Node returns the result; Gateway replies back out to Telegram.

Notes:

- **Nodes do not run the gateway service.** Only one gateway should run per host unless you intentionally run isolated profiles (see [Multiple gateways](https://docs.openclaw.ai/gateway/multiple-gateways)).
- macOS app “node mode” is just a node client over the Gateway WebSocket.

## [​](https://docs.openclaw.ai/gateway/remote\#ssh-tunnel-cli-+-tools)  SSH tunnel (CLI + tools)

Create a local tunnel to the remote Gateway WS:

```
ssh -N -L 18789:127.0.0.1:18789 user@host
```

With the tunnel up:

- `openclaw health` and `openclaw status --deep` now reach the remote gateway via `ws://127.0.0.1:18789`.
- `openclaw gateway status`, `openclaw gateway health`, `openclaw gateway probe`, and `openclaw gateway call` can also target the forwarded URL via `--url` when needed.

Replace `18789` with your configured `gateway.port` (or `--port` or `OPENCLAW_GATEWAY_PORT`).

When you pass `--url`, the CLI does not fall back to config or environment credentials. Include `--token` or `--password` explicitly. Missing explicit credentials is an error.

## [​](https://docs.openclaw.ai/gateway/remote\#cli-remote-defaults)  CLI remote defaults

You can persist a remote target so CLI commands use it by default:

```
{
  gateway: {
    mode: "remote",
    remote: {
      url: "ws://127.0.0.1:18789",
      token: "your-token",
    },
  },
}
```

When the gateway is loopback-only, keep the URL at `ws://127.0.0.1:18789` and open the SSH tunnel first.
In the macOS app’s SSH tunnel transport, discovered gateway hostnames belong in
`gateway.remote.sshTarget`; `gateway.remote.url` remains the local tunnel URL.

## [​](https://docs.openclaw.ai/gateway/remote\#credential-precedence)  Credential precedence

Gateway credential resolution follows one shared contract across call/probe/status paths and Discord exec-approval monitoring. Node-host uses the same base contract with one local-mode exception (it intentionally ignores `gateway.remote.*`):

- Explicit credentials (`--token`, `--password`, or tool `gatewayToken`) always win on call paths that accept explicit auth.
- URL override safety:
  - CLI URL overrides (`--url`) never reuse implicit config/env credentials.
  - Env URL overrides (`OPENCLAW_GATEWAY_URL`) may use env credentials only (`OPENCLAW_GATEWAY_TOKEN` / `OPENCLAW_GATEWAY_PASSWORD`).
- Local mode defaults:
  - token: `OPENCLAW_GATEWAY_TOKEN` -\> `gateway.auth.token` -\> `gateway.remote.token` (remote fallback applies only when local auth token input is unset)
  - password: `OPENCLAW_GATEWAY_PASSWORD` -\> `gateway.auth.password` -\> `gateway.remote.password` (remote fallback applies only when local auth password input is unset)
- Remote mode defaults:
  - token: `gateway.remote.token` -\> `OPENCLAW_GATEWAY_TOKEN` -\> `gateway.auth.token`
  - password: `OPENCLAW_GATEWAY_PASSWORD` -\> `gateway.remote.password` -\> `gateway.auth.password`
- Node-host local-mode exception: `gateway.remote.token` / `gateway.remote.password` are ignored.
- Remote probe/status token checks are strict by default: they use `gateway.remote.token` only (no local token fallback) when targeting remote mode.
- Gateway env overrides use `OPENCLAW_GATEWAY_*` only.

## [​](https://docs.openclaw.ai/gateway/remote\#chat-ui-over-ssh)  Chat UI over SSH

WebChat no longer uses a separate HTTP port. The SwiftUI chat UI connects directly to the Gateway WebSocket.

- Forward `18789` over SSH (see above), then connect clients to `ws://127.0.0.1:18789`.
- On macOS, prefer the app’s “Remote over SSH” mode, which manages the tunnel automatically.

## [​](https://docs.openclaw.ai/gateway/remote\#macos-app-remote-over-ssh)  macOS app Remote over SSH

The macOS menu bar app can drive the same setup end-to-end (remote status checks, WebChat, and Voice Wake forwarding).Runbook: [macOS remote access](https://docs.openclaw.ai/platforms/mac/remote).

## [​](https://docs.openclaw.ai/gateway/remote\#security-rules-remote/vpn)  Security rules (remote/VPN)

Short version: **keep the Gateway loopback-only** unless you’re sure you need a bind.

- **Loopback + SSH/Tailscale Serve** is the safest default (no public exposure).
- Plaintext `ws://` is loopback-only by default. For trusted private networks,
set `OPENCLAW_ALLOW_INSECURE_PRIVATE_WS=1` on the client process as
break-glass. There is no `openclaw.json` equivalent; this must be process
environment for the client making the WebSocket connection.
- **Non-loopback binds** (`lan`/`tailnet`/`custom`, or `auto` when loopback is unavailable) must use gateway auth: token, password, or an identity-aware reverse proxy with `gateway.auth.mode: "trusted-proxy"`.
- `gateway.remote.token` / `.password` are client credential sources. They do **not** configure server auth by themselves.
- Local call paths can use `gateway.remote.*` as fallback only when `gateway.auth.*` is unset.
- If `gateway.auth.token` / `gateway.auth.password` is explicitly configured via SecretRef and unresolved, resolution fails closed (no remote fallback masking).
- `gateway.remote.tlsFingerprint` pins the remote TLS cert when using `wss://`.
- **Tailscale Serve** can authenticate Control UI/WebSocket traffic via identity
headers when `gateway.auth.allowTailscale: true`; HTTP API endpoints do not
use that Tailscale header auth and instead follow the gateway’s normal HTTP
auth mode. This tokenless flow assumes the gateway host is trusted. Set it to
`false` if you want shared-secret auth everywhere.
- **Trusted-proxy** auth expects non-loopback identity-aware proxy setups by default.
Same-host loopback reverse proxies require explicit `gateway.auth.trustedProxy.allowLoopback = true`.
- Treat browser control like operator access: tailnet-only + deliberate node pairing.

Deep dive: [Security](https://docs.openclaw.ai/gateway/security).

### [​](https://docs.openclaw.ai/gateway/remote\#macos-persistent-ssh-tunnel-via-launchagent)  macOS: persistent SSH tunnel via LaunchAgent

For macOS clients connecting to a remote gateway, the easiest persistent setup uses an SSH `LocalForward` config entry plus a LaunchAgent to keep the tunnel alive across reboots and crashes.

#### [​](https://docs.openclaw.ai/gateway/remote\#step-1-add-ssh-config)  Step 1: add SSH config

Edit `~/.ssh/config`:

```
Host remote-gateway
    HostName <REMOTE_IP>
    User <REMOTE_USER>
    LocalForward 18789 127.0.0.1:18789
    IdentityFile ~/.ssh/id_rsa
```

Replace `<REMOTE_IP>` and `<REMOTE_USER>` with your values.

#### [​](https://docs.openclaw.ai/gateway/remote\#step-2-copy-ssh-key-one-time)  Step 2: copy SSH key (one-time)

```
ssh-copy-id -i ~/.ssh/id_rsa <REMOTE_USER>@<REMOTE_IP>
```

#### [​](https://docs.openclaw.ai/gateway/remote\#step-3-configure-the-gateway-token)  Step 3: configure the gateway token

Store the token in config so it persists across restarts:

```
openclaw config set gateway.remote.token "<your-token>"
```

#### [​](https://docs.openclaw.ai/gateway/remote\#step-4-create-the-launchagent)  Step 4: create the LaunchAgent

Save this as `~/Library/LaunchAgents/ai.openclaw.ssh-tunnel.plist`:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>ai.openclaw.ssh-tunnel</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/bin/ssh</string>
        <string>-N</string>
        <string>remote-gateway</string>
    </array>
    <key>KeepAlive</key>
    <true/>
    <key>RunAtLoad</key>
    <true/>
</dict>
</plist>
```

#### [​](https://docs.openclaw.ai/gateway/remote\#step-5-load-the-launchagent)  Step 5: load the LaunchAgent

```
launchctl bootstrap gui/$UID ~/Library/LaunchAgents/ai.openclaw.ssh-tunnel.plist
```

The tunnel will start automatically at login, restart on crash, and keep the forwarded port live.

If you have a leftover `com.openclaw.ssh-tunnel` LaunchAgent from an older setup, unload and delete it.

#### [​](https://docs.openclaw.ai/gateway/remote\#troubleshooting)  Troubleshooting

Check if the tunnel is running:

```
ps aux | grep "ssh -N remote-gateway" | grep -v grep
lsof -i :18789
```

Restart the tunnel:

```
launchctl kickstart -k gui/$UID/ai.openclaw.ssh-tunnel
```

Stop the tunnel:

```
launchctl bootout gui/$UID/ai.openclaw.ssh-tunnel
```

| Config entry | What it does |
| --- | --- |
| `LocalForward 18789 127.0.0.1:18789` | Forwards local port 18789 to remote port 18789 |
| `ssh -N` | SSH without executing remote commands (port-forwarding only) |
| `KeepAlive` | Automatically restarts the tunnel if it crashes |
| `RunAtLoad` | Starts the tunnel when the LaunchAgent loads at login |

## [​](https://docs.openclaw.ai/gateway/remote\#related)  Related

- [Tailscale](https://docs.openclaw.ai/gateway/tailscale)
- [Authentication](https://docs.openclaw.ai/gateway/authentication)
- [Remote gateway setup](https://docs.openclaw.ai/gateway/remote-gateway-readme)

[Bonjour discovery](https://docs.openclaw.ai/gateway/bonjour) [Remote gateway setup](https://docs.openclaw.ai/gateway/remote-gateway-readme)

Ctrl+I