---
source_url: https://docs.openclaw.ai/web
title: "Web - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/web#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Web interfaces

Web

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Webhooks](https://docs.openclaw.ai/web#webhooks)
- [Config (default-on)](https://docs.openclaw.ai/web#config-default-on)
- [Tailscale access](https://docs.openclaw.ai/web#tailscale-access)
- [Integrated Serve (recommended)](https://docs.openclaw.ai/web#integrated-serve-recommended)
- [Tailnet bind + token](https://docs.openclaw.ai/web#tailnet-bind-%2B-token)
- [Public internet (Funnel)](https://docs.openclaw.ai/web#public-internet-funnel)
- [Security notes](https://docs.openclaw.ai/web#security-notes)
- [Building the UI](https://docs.openclaw.ai/web#building-the-ui)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

The Gateway serves a small **browser Control UI** (Vite + Lit) from the same port as the Gateway WebSocket:

- default: `http://<host>:18789/`
- with `gateway.tls.enabled: true`: `https://<host>:18789/`
- optional prefix: set `gateway.controlUi.basePath` (e.g. `/openclaw`)

Capabilities live in [Control UI](https://docs.openclaw.ai/web/control-ui). The rest of this page focuses on bind modes, security, and web-facing surfaces.

## [​](https://docs.openclaw.ai/web\#webhooks)  Webhooks

When `hooks.enabled=true`, the Gateway also exposes a small webhook endpoint on the same HTTP server.
See [Gateway configuration](https://docs.openclaw.ai/gateway/configuration) → `hooks` for auth + payloads.

## [​](https://docs.openclaw.ai/web\#config-default-on)  Config (default-on)

The Control UI is **enabled by default** when assets are present (`dist/control-ui`).
You can control it via config:

```
{
  gateway: {
    controlUi: { enabled: true, basePath: "/openclaw" }, // basePath optional
  },
}
```

## [​](https://docs.openclaw.ai/web\#tailscale-access)  Tailscale access

### [​](https://docs.openclaw.ai/web\#integrated-serve-recommended)  Integrated Serve (recommended)

Keep the Gateway on loopback and let Tailscale Serve proxy it:

```
{
  gateway: {
    bind: "loopback",
    tailscale: { mode: "serve" },
  },
}
```

Then start the gateway:

```
openclaw gateway
```

Open:

- `https://<magicdns>/` (or your configured `gateway.controlUi.basePath`)

### [​](https://docs.openclaw.ai/web\#tailnet-bind-+-token)  Tailnet bind + token

```
{
  gateway: {
    bind: "tailnet",
    controlUi: { enabled: true },
    auth: { mode: "token", token: "your-token" },
  },
}
```

Then start the gateway (this non-loopback example uses shared-secret token
auth):

```
openclaw gateway
```

Open:

- `http://<tailscale-ip>:18789/` (or your configured `gateway.controlUi.basePath`)

### [​](https://docs.openclaw.ai/web\#public-internet-funnel)  Public internet (Funnel)

```
{
  gateway: {
    bind: "loopback",
    tailscale: { mode: "funnel" },
    auth: { mode: "password" }, // or OPENCLAW_GATEWAY_PASSWORD
  },
}
```

## [​](https://docs.openclaw.ai/web\#security-notes)  Security notes

- Gateway auth is required by default (token, password, trusted-proxy, or Tailscale Serve identity headers when enabled).
- Non-loopback binds still **require** gateway auth. In practice that means token/password auth or an identity-aware reverse proxy with `gateway.auth.mode: "trusted-proxy"`.
- The wizard creates shared-secret auth by default and usually generates a
gateway token (even on loopback).
- In shared-secret mode, the UI sends `connect.params.auth.token` or
`connect.params.auth.password`.
- When `gateway.tls.enabled: true`, local dashboard and status helpers render
`https://` dashboard URLs and `wss://` WebSocket URLs.
- In identity-bearing modes such as Tailscale Serve or `trusted-proxy`, the
WebSocket auth check is satisfied from request headers instead.
- For non-loopback Control UI deployments, set `gateway.controlUi.allowedOrigins`
explicitly (full origins). Without it, gateway startup is refused by default.
- `gateway.controlUi.dangerouslyAllowHostHeaderOriginFallback=true` enables
Host-header origin fallback mode, but is a dangerous security downgrade.
- With Serve, Tailscale identity headers can satisfy Control UI/WebSocket auth
when `gateway.auth.allowTailscale` is `true` (no token/password required).
HTTP API endpoints do not use those Tailscale identity headers; they follow
the gateway’s normal HTTP auth mode instead. Set
`gateway.auth.allowTailscale: false` to require explicit credentials. See
[Tailscale](https://docs.openclaw.ai/gateway/tailscale) and [Security](https://docs.openclaw.ai/gateway/security). This
tokenless flow assumes the gateway host is trusted.
- `gateway.tailscale.mode: "funnel"` requires `gateway.auth.mode: "password"` (shared password).

## [​](https://docs.openclaw.ai/web\#building-the-ui)  Building the UI

The Gateway serves static files from `dist/control-ui`. Build them with:

```
pnpm ui:build
```

[Location command](https://docs.openclaw.ai/nodes/location-command) [Control UI](https://docs.openclaw.ai/web/control-ui)

Ctrl+I