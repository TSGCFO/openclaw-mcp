---
source_url: https://docs.openclaw.ai/concepts/architecture
title: "Gateway architecture - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/concepts/architecture#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Fundamentals

Gateway architecture

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Overview](https://docs.openclaw.ai/concepts/architecture#overview)
- [Components and flows](https://docs.openclaw.ai/concepts/architecture#components-and-flows)
- [Gateway (daemon)](https://docs.openclaw.ai/concepts/architecture#gateway-daemon)
- [Clients (mac app / CLI / web admin)](https://docs.openclaw.ai/concepts/architecture#clients-mac-app-%2F-cli-%2F-web-admin)
- [Nodes (macOS / iOS / Android / headless)](https://docs.openclaw.ai/concepts/architecture#nodes-macos-%2F-ios-%2F-android-%2F-headless)
- [WebChat](https://docs.openclaw.ai/concepts/architecture#webchat)
- [Connection lifecycle (single client)](https://docs.openclaw.ai/concepts/architecture#connection-lifecycle-single-client)
- [Wire protocol (summary)](https://docs.openclaw.ai/concepts/architecture#wire-protocol-summary)
- [Pairing + local trust](https://docs.openclaw.ai/concepts/architecture#pairing-%2B-local-trust)
- [Protocol typing and codegen](https://docs.openclaw.ai/concepts/architecture#protocol-typing-and-codegen)
- [Remote access](https://docs.openclaw.ai/concepts/architecture#remote-access)
- [Operations snapshot](https://docs.openclaw.ai/concepts/architecture#operations-snapshot)
- [Invariants](https://docs.openclaw.ai/concepts/architecture#invariants)
- [Related](https://docs.openclaw.ai/concepts/architecture#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

## [​](https://docs.openclaw.ai/concepts/architecture\#overview)  Overview

- A single long‑lived **Gateway** owns all messaging surfaces (WhatsApp via
Baileys, Telegram via grammY, Slack, Discord, Signal, iMessage, WebChat).
- Control-plane clients (macOS app, CLI, web UI, automations) connect to the
Gateway over **WebSocket** on the configured bind host (default
`127.0.0.1:18789`).
- **Nodes** (macOS/iOS/Android/headless) also connect over **WebSocket**, but
declare `role: node` with explicit caps/commands.
- One Gateway per host; it is the only place that opens a WhatsApp session.
- The **canvas host** is served by the Gateway HTTP server under:

  - `/__openclaw__/canvas/` (agent-editable HTML/CSS/JS)
  - `/__openclaw__/a2ui/` (A2UI host)
    It uses the same port as the Gateway (default `18789`).

## [​](https://docs.openclaw.ai/concepts/architecture\#components-and-flows)  Components and flows

### [​](https://docs.openclaw.ai/concepts/architecture\#gateway-daemon)  Gateway (daemon)

- Maintains provider connections.
- Exposes a typed WS API (requests, responses, server‑push events).
- Validates inbound frames against JSON Schema.
- Emits events like `agent`, `chat`, `presence`, `health`, `heartbeat`, `cron`.

### [​](https://docs.openclaw.ai/concepts/architecture\#clients-mac-app-/-cli-/-web-admin)  Clients (mac app / CLI / web admin)

- One WS connection per client.
- Send requests (`health`, `status`, `send`, `agent`, `system-presence`).
- Subscribe to events (`tick`, `agent`, `presence`, `shutdown`).

### [​](https://docs.openclaw.ai/concepts/architecture\#nodes-macos-/-ios-/-android-/-headless)  Nodes (macOS / iOS / Android / headless)

- Connect to the **same WS server** with `role: node`.
- Provide a device identity in `connect`; pairing is **device‑based** (role `node`) and
approval lives in the device pairing store.
- Expose commands like `canvas.*`, `camera.*`, `screen.record`, `location.get`.

Protocol details:

- [Gateway protocol](https://docs.openclaw.ai/gateway/protocol)

### [​](https://docs.openclaw.ai/concepts/architecture\#webchat)  WebChat

- Static UI that uses the Gateway WS API for chat history and sends.
- In remote setups, connects through the same SSH/Tailscale tunnel as other
clients.

## [​](https://docs.openclaw.ai/concepts/architecture\#connection-lifecycle-single-client)  Connection lifecycle (single client)

GatewayClientGatewayClientor res error + closepayload=hello-oksnapshot: presence + healthreq:connectres (ok)event:presenceevent:tickreq:agentres:agentack {runId, status:"accepted"}event:agent(streaming)res:agentfinal {runId, status, summary}

## [​](https://docs.openclaw.ai/concepts/architecture\#wire-protocol-summary)  Wire protocol (summary)

- Transport: WebSocket, text frames with JSON payloads.
- First frame **must** be `connect`.
- After handshake:
  - Requests: `{type:"req", id, method, params}` → `{type:"res", id, ok, payload|error}`
  - Events: `{type:"event", event, payload, seq?, stateVersion?}`
- `hello-ok.features.methods` / `events` are discovery metadata, not a
generated dump of every callable helper route.
- Shared-secret auth uses `connect.params.auth.token` or
`connect.params.auth.password`, depending on the configured gateway auth mode.
- Identity-bearing modes such as Tailscale Serve
(`gateway.auth.allowTailscale: true`) or non-loopback
`gateway.auth.mode: "trusted-proxy"` satisfy auth from request headers
instead of `connect.params.auth.*`.
- Private-ingress `gateway.auth.mode: "none"` disables shared-secret auth
entirely; keep that mode off public/untrusted ingress.
- Idempotency keys are required for side‑effecting methods (`send`, `agent`) to
safely retry; the server keeps a short‑lived dedupe cache.
- Nodes must include `role: "node"` plus caps/commands/permissions in `connect`.

## [​](https://docs.openclaw.ai/concepts/architecture\#pairing-+-local-trust)  Pairing + local trust

- All WS clients (operators + nodes) include a **device identity** on `connect`.
- New device IDs require pairing approval; the Gateway issues a **device token**
for subsequent connects.
- Direct local loopback connects can be auto-approved to keep same-host UX
smooth.
- OpenClaw also has a narrow backend/container-local self-connect path for
trusted shared-secret helper flows.
- Tailnet and LAN connects, including same-host tailnet binds, still require
explicit pairing approval.
- All connects must sign the `connect.challenge` nonce.
- Signature payload `v3` also binds `platform` \+ `deviceFamily`; the gateway
pins paired metadata on reconnect and requires repair pairing for metadata
changes.
- **Non‑local** connects still require explicit approval.
- Gateway auth (`gateway.auth.*`) still applies to **all** connections, local or
remote.

Details: [Gateway protocol](https://docs.openclaw.ai/gateway/protocol), [Pairing](https://docs.openclaw.ai/channels/pairing),
[Security](https://docs.openclaw.ai/gateway/security).

## [​](https://docs.openclaw.ai/concepts/architecture\#protocol-typing-and-codegen)  Protocol typing and codegen

- TypeBox schemas define the protocol.
- JSON Schema is generated from those schemas.
- Swift models are generated from the JSON Schema.

## [​](https://docs.openclaw.ai/concepts/architecture\#remote-access)  Remote access

- Preferred: Tailscale or VPN.
- Alternative: SSH tunnel














```
ssh -N -L 18789:127.0.0.1:18789 user@host
```

- The same handshake + auth token apply over the tunnel.
- TLS + optional pinning can be enabled for WS in remote setups.

## [​](https://docs.openclaw.ai/concepts/architecture\#operations-snapshot)  Operations snapshot

- Start: `openclaw gateway` (foreground, logs to stdout).
- Health: `health` over WS (also included in `hello-ok`).
- Supervision: launchd/systemd for auto‑restart.

## [​](https://docs.openclaw.ai/concepts/architecture\#invariants)  Invariants

- Exactly one Gateway controls a single Baileys session per host.
- Handshake is mandatory; any non‑JSON or non‑connect first frame is a hard close.
- Events are not replayed; clients must refresh on gaps.

## [​](https://docs.openclaw.ai/concepts/architecture\#related)  Related

- [Agent Loop](https://docs.openclaw.ai/concepts/agent-loop) — detailed agent execution cycle
- [Gateway Protocol](https://docs.openclaw.ai/gateway/protocol) — WebSocket protocol contract
- [Queue](https://docs.openclaw.ai/concepts/queue) — command queue and concurrency
- [Security](https://docs.openclaw.ai/gateway/security) — trust model and hardening

[Agent runtime](https://docs.openclaw.ai/concepts/agent)

Ctrl+I