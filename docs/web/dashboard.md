---
source_url: https://docs.openclaw.ai/web/dashboard
title: "Dashboard - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/web/dashboard#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Web interfaces

Dashboard

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Fast path (recommended)](https://docs.openclaw.ai/web/dashboard#fast-path-recommended)
- [Auth basics (local vs remote)](https://docs.openclaw.ai/web/dashboard#auth-basics-local-vs-remote)
- [If you see “unauthorized” / 1008](https://docs.openclaw.ai/web/dashboard#if-you-see-%E2%80%9Cunauthorized%E2%80%9D-%2F-1008)
- [Related](https://docs.openclaw.ai/web/dashboard#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

The Gateway dashboard is the browser Control UI served at `/` by default
(override with `gateway.controlUi.basePath`).Quick open (local Gateway):

- [http://127.0.0.1:18789/](http://127.0.0.1:18789/) (or [http://localhost:18789/](http://localhost:18789/))
- With `gateway.tls.enabled: true`, use `https://127.0.0.1:18789/` and
`wss://127.0.0.1:18789` for the WebSocket endpoint.

Key references:

- [Control UI](https://docs.openclaw.ai/web/control-ui) for usage and UI capabilities.
- [Tailscale](https://docs.openclaw.ai/gateway/tailscale) for Serve/Funnel automation.
- [Web surfaces](https://docs.openclaw.ai/web) for bind modes and security notes.

Authentication is enforced at the WebSocket handshake via the configured gateway
auth path:

- `connect.params.auth.token`
- `connect.params.auth.password`
- Tailscale Serve identity headers when `gateway.auth.allowTailscale: true`
- trusted-proxy identity headers when `gateway.auth.mode: "trusted-proxy"`

See `gateway.auth` in [Gateway configuration](https://docs.openclaw.ai/gateway/configuration).Security note: the Control UI is an **admin surface** (chat, config, exec approvals).
Do not expose it publicly. The UI keeps dashboard URL tokens in sessionStorage
for the current browser tab session and selected gateway URL, and strips them from the URL after load.
Prefer localhost, Tailscale Serve, or an SSH tunnel.

## [​](https://docs.openclaw.ai/web/dashboard\#fast-path-recommended)  Fast path (recommended)

- After onboarding, the CLI auto-opens the dashboard and prints a clean (non-tokenized) link.
- Re-open anytime: `openclaw dashboard` (copies link, opens browser if possible, shows SSH hint if headless).
- If the UI prompts for shared-secret auth, paste the configured token or
password into Control UI settings.

## [​](https://docs.openclaw.ai/web/dashboard\#auth-basics-local-vs-remote)  Auth basics (local vs remote)

- **Localhost**: open `http://127.0.0.1:18789/`.
- **Gateway TLS**: when `gateway.tls.enabled: true`, dashboard/status links use
`https://` and Control UI WebSocket links use `wss://`.
- **Shared-secret token source**: `gateway.auth.token` (or
`OPENCLAW_GATEWAY_TOKEN`); `openclaw dashboard` can pass it via URL fragment
for one-time bootstrap, and the Control UI keeps it in sessionStorage for the
current browser tab session and selected gateway URL instead of localStorage.
- If `gateway.auth.token` is SecretRef-managed, `openclaw dashboard`
prints/copies/opens a non-tokenized URL by design. This avoids exposing
externally managed tokens in shell logs, clipboard history, or browser-launch
arguments.
- If `gateway.auth.token` is configured as a SecretRef and is unresolved in your
current shell, `openclaw dashboard` still prints a non-tokenized URL plus
actionable auth setup guidance.
- **Shared-secret password**: use the configured `gateway.auth.password` (or
`OPENCLAW_GATEWAY_PASSWORD`). The dashboard does not persist passwords across
reloads.
- **Identity-bearing modes**: Tailscale Serve can satisfy Control UI/WebSocket
auth via identity headers when `gateway.auth.allowTailscale: true`, and a
non-loopback identity-aware reverse proxy can satisfy
`gateway.auth.mode: "trusted-proxy"`. In those modes the dashboard does not
need a pasted shared secret for the WebSocket.
- **Not localhost**: use Tailscale Serve, a non-loopback shared-secret bind, a
non-loopback identity-aware reverse proxy with
`gateway.auth.mode: "trusted-proxy"`, or an SSH tunnel. HTTP APIs still use
shared-secret auth unless you intentionally run private-ingress
`gateway.auth.mode: "none"` or trusted-proxy HTTP auth. See
[Web surfaces](https://docs.openclaw.ai/web).

## [​](https://docs.openclaw.ai/web/dashboard\#if-you-see-%E2%80%9Cunauthorized%E2%80%9D-/-1008)  If you see “unauthorized” / 1008

- Ensure the gateway is reachable (local: `openclaw status`; remote: SSH tunnel `ssh -N -L 18789:127.0.0.1:18789 user@host` then open `http://127.0.0.1:18789/`).
- For `AUTH_TOKEN_MISMATCH`, clients may do one trusted retry with a cached device token when the gateway returns retry hints. That cached-token retry reuses the token’s cached approved scopes; explicit `deviceToken` / explicit `scopes` callers keep their requested scope set. If auth still fails after that retry, resolve token drift manually.
- Outside that retry path, connect auth precedence is explicit shared token/password first, then explicit `deviceToken`, then stored device token, then bootstrap token.
- On the async Tailscale Serve Control UI path, failed attempts for the same
`{scope, ip}` are serialized before the failed-auth limiter records them, so
the second concurrent bad retry can already show `retry later`.
- For token drift repair steps, follow [Token drift recovery checklist](https://docs.openclaw.ai/cli/devices#token-drift-recovery-checklist).
- Retrieve or supply the shared secret from the gateway host:
  - Token: `openclaw config get gateway.auth.token`
  - Password: resolve the configured `gateway.auth.password` or
    `OPENCLAW_GATEWAY_PASSWORD`
  - SecretRef-managed token: resolve the external secret provider or export
    `OPENCLAW_GATEWAY_TOKEN` in this shell, then rerun `openclaw dashboard`
  - No shared secret configured: `openclaw doctor --generate-gateway-token`
- In the dashboard settings, paste the token or password into the auth field,
then connect.
- The UI language picker is in **Overview -> Gateway Access -> Language**.
It is part of the access card, not the Appearance section.

## [​](https://docs.openclaw.ai/web/dashboard\#related)  Related

- [Control UI](https://docs.openclaw.ai/web/control-ui)
- [WebChat](https://docs.openclaw.ai/web/webchat)

[Control UI](https://docs.openclaw.ai/web/control-ui) [WebChat](https://docs.openclaw.ai/web/webchat)

Ctrl+I