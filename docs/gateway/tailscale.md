---
source_url: https://docs.openclaw.ai/gateway/tailscale
title: "Tailscale - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/gateway/tailscale#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Remote access

Tailscale

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Modes](https://docs.openclaw.ai/gateway/tailscale#modes)
- [Auth](https://docs.openclaw.ai/gateway/tailscale#auth)
- [Config examples](https://docs.openclaw.ai/gateway/tailscale#config-examples)
- [Tailnet-only (Serve)](https://docs.openclaw.ai/gateway/tailscale#tailnet-only-serve)
- [Tailnet-only (bind to Tailnet IP)](https://docs.openclaw.ai/gateway/tailscale#tailnet-only-bind-to-tailnet-ip)
- [Public internet (Funnel + shared password)](https://docs.openclaw.ai/gateway/tailscale#public-internet-funnel-%2B-shared-password)
- [CLI examples](https://docs.openclaw.ai/gateway/tailscale#cli-examples)
- [Notes](https://docs.openclaw.ai/gateway/tailscale#notes)
- [Browser control (remote Gateway + local browser)](https://docs.openclaw.ai/gateway/tailscale#browser-control-remote-gateway-%2B-local-browser)
- [Tailscale prerequisites + limits](https://docs.openclaw.ai/gateway/tailscale#tailscale-prerequisites-%2B-limits)
- [Learn more](https://docs.openclaw.ai/gateway/tailscale#learn-more)
- [Related](https://docs.openclaw.ai/gateway/tailscale#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

OpenClaw can auto-configure Tailscale **Serve** (tailnet) or **Funnel** (public) for the
Gateway dashboard and WebSocket port. This keeps the Gateway bound to loopback while
Tailscale provides HTTPS, routing, and (for Serve) identity headers.

## [​](https://docs.openclaw.ai/gateway/tailscale\#modes)  Modes

- `serve`: Tailnet-only Serve via `tailscale serve`. The gateway stays on `127.0.0.1`.
- `funnel`: Public HTTPS via `tailscale funnel`. OpenClaw requires a shared password.
- `off`: Default (no Tailscale automation).

Status and audit output use **Tailscale exposure** for this OpenClaw Serve/Funnel
mode. `off` means OpenClaw is not managing Serve or Funnel; it does not mean the
local Tailscale daemon is stopped or logged out.

## [​](https://docs.openclaw.ai/gateway/tailscale\#auth)  Auth

Set `gateway.auth.mode` to control the handshake:

- `none` (private ingress only)
- `token` (default when `OPENCLAW_GATEWAY_TOKEN` is set)
- `password` (shared secret via `OPENCLAW_GATEWAY_PASSWORD` or config)
- `trusted-proxy` (identity-aware reverse proxy; see [Trusted Proxy Auth](https://docs.openclaw.ai/gateway/trusted-proxy-auth))

When `tailscale.mode = "serve"` and `gateway.auth.allowTailscale` is `true`,
Control UI/WebSocket auth can use Tailscale identity headers
(`tailscale-user-login`) without supplying a token/password. OpenClaw verifies
the identity by resolving the `x-forwarded-for` address via the local Tailscale
daemon (`tailscale whois`) and matching it to the header before accepting it.
OpenClaw only treats a request as Serve when it arrives from loopback with
Tailscale’s `x-forwarded-for`, `x-forwarded-proto`, and `x-forwarded-host`
headers.
For Control UI operator sessions that include browser device identity, this
verified Serve path also skips the device-pairing round trip. It does not bypass
browser device identity: device-less clients are still rejected, and node-role
or non-Control UI WebSocket connections still follow the normal pairing and
auth checks.
HTTP API endpoints (for example `/v1/*`, `/tools/invoke`, and `/api/channels/*`)
do **not** use Tailscale identity-header auth. They still follow the gateway’s
normal HTTP auth mode: shared-secret auth by default, or an intentionally
configured trusted-proxy / private-ingress `none` setup.
This tokenless flow assumes the gateway host is trusted. If untrusted local code
may run on the same host, disable `gateway.auth.allowTailscale` and require
token/password auth instead.
To require explicit shared-secret credentials, set `gateway.auth.allowTailscale: false`
and use `gateway.auth.mode: "token"` or `"password"`.

## [​](https://docs.openclaw.ai/gateway/tailscale\#config-examples)  Config examples

### [​](https://docs.openclaw.ai/gateway/tailscale\#tailnet-only-serve)  Tailnet-only (Serve)

```
{
  gateway: {
    bind: "loopback",
    tailscale: { mode: "serve" },
  },
}
```

Open: `https://<magicdns>/` (or your configured `gateway.controlUi.basePath`)

### [​](https://docs.openclaw.ai/gateway/tailscale\#tailnet-only-bind-to-tailnet-ip)  Tailnet-only (bind to Tailnet IP)

Use this when you want the Gateway to listen directly on the Tailnet IP (no Serve/Funnel).

```
{
  gateway: {
    bind: "tailnet",
    auth: { mode: "token", token: "your-token" },
  },
}
```

Connect from another Tailnet device:

- Control UI: `http://<tailscale-ip>:18789/`
- WebSocket: `ws://<tailscale-ip>:18789`

Loopback (`http://127.0.0.1:18789`) will **not** work in this mode.

### [​](https://docs.openclaw.ai/gateway/tailscale\#public-internet-funnel-+-shared-password)  Public internet (Funnel + shared password)

```
{
  gateway: {
    bind: "loopback",
    tailscale: { mode: "funnel" },
    auth: { mode: "password", password: "replace-me" },
  },
}
```

Prefer `OPENCLAW_GATEWAY_PASSWORD` over committing a password to disk.

## [​](https://docs.openclaw.ai/gateway/tailscale\#cli-examples)  CLI examples

```
openclaw gateway --tailscale serve
openclaw gateway --tailscale funnel --auth password
```

## [​](https://docs.openclaw.ai/gateway/tailscale\#notes)  Notes

- Tailscale Serve/Funnel requires the `tailscale` CLI to be installed and logged in.
- `tailscale.mode: "funnel"` refuses to start unless auth mode is `password` to avoid public exposure.
- Set `gateway.tailscale.resetOnExit` if you want OpenClaw to undo `tailscale serve`
or `tailscale funnel` configuration on shutdown.
- `gateway.bind: "tailnet"` is a direct Tailnet bind (no HTTPS, no Serve/Funnel).
- `gateway.bind: "auto"` prefers loopback; use `tailnet` if you want Tailnet-only.
- Serve/Funnel only expose the **Gateway control UI + WS**. Nodes connect over
the same Gateway WS endpoint, so Serve can work for node access.

## [​](https://docs.openclaw.ai/gateway/tailscale\#browser-control-remote-gateway-+-local-browser)  Browser control (remote Gateway + local browser)

If you run the Gateway on one machine but want to drive a browser on another machine,
run a **node host** on the browser machine and keep both on the same tailnet.
The Gateway will proxy browser actions to the node; no separate control server or Serve URL needed.Avoid Funnel for browser control; treat node pairing like operator access.

## [​](https://docs.openclaw.ai/gateway/tailscale\#tailscale-prerequisites-+-limits)  Tailscale prerequisites + limits

- Serve requires HTTPS enabled for your tailnet; the CLI prompts if it is missing.
- Serve injects Tailscale identity headers; Funnel does not.
- Funnel requires Tailscale v1.38.3+, MagicDNS, HTTPS enabled, and a funnel node attribute.
- Funnel only supports ports `443`, `8443`, and `10000` over TLS.
- Funnel on macOS requires the open-source Tailscale app variant.

## [​](https://docs.openclaw.ai/gateway/tailscale\#learn-more)  Learn more

- Tailscale Serve overview: [https://tailscale.com/kb/1312/serve](https://tailscale.com/kb/1312/serve)
- `tailscale serve` command: [https://tailscale.com/kb/1242/tailscale-serve](https://tailscale.com/kb/1242/tailscale-serve)
- Tailscale Funnel overview: [https://tailscale.com/kb/1223/tailscale-funnel](https://tailscale.com/kb/1223/tailscale-funnel)
- `tailscale funnel` command: [https://tailscale.com/kb/1311/tailscale-funnel](https://tailscale.com/kb/1311/tailscale-funnel)

## [​](https://docs.openclaw.ai/gateway/tailscale\#related)  Related

- [Remote access](https://docs.openclaw.ai/gateway/remote)
- [Discovery](https://docs.openclaw.ai/gateway/discovery)
- [Authentication](https://docs.openclaw.ai/gateway/authentication)

[Remote gateway setup](https://docs.openclaw.ai/gateway/remote-gateway-readme) [Network proxy](https://docs.openclaw.ai/security/network-proxy)

Ctrl+I