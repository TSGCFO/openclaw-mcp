---
source_url: https://docs.openclaw.ai/gateway/trusted-proxy-auth
title: "Trusted proxy auth - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/gateway/trusted-proxy-auth#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Authentication and secrets

Trusted proxy auth

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [When to use](https://docs.openclaw.ai/gateway/trusted-proxy-auth#when-to-use)
- [When NOT to use](https://docs.openclaw.ai/gateway/trusted-proxy-auth#when-not-to-use)
- [How it works](https://docs.openclaw.ai/gateway/trusted-proxy-auth#how-it-works)
- [Control UI pairing behavior](https://docs.openclaw.ai/gateway/trusted-proxy-auth#control-ui-pairing-behavior)
- [Configuration](https://docs.openclaw.ai/gateway/trusted-proxy-auth#configuration)
- [Configuration reference](https://docs.openclaw.ai/gateway/trusted-proxy-auth#configuration-reference)
- [TLS termination and HSTS](https://docs.openclaw.ai/gateway/trusted-proxy-auth#tls-termination-and-hsts)
- [Rollout guidance](https://docs.openclaw.ai/gateway/trusted-proxy-auth#rollout-guidance)
- [Proxy setup examples](https://docs.openclaw.ai/gateway/trusted-proxy-auth#proxy-setup-examples)
- [Mixed token configuration](https://docs.openclaw.ai/gateway/trusted-proxy-auth#mixed-token-configuration)
- [Operator scopes header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#operator-scopes-header)
- [Security checklist](https://docs.openclaw.ai/gateway/trusted-proxy-auth#security-checklist)
- [Security audit](https://docs.openclaw.ai/gateway/trusted-proxy-auth#security-audit)
- [Troubleshooting](https://docs.openclaw.ai/gateway/trusted-proxy-auth#troubleshooting)
- [Migration from token auth](https://docs.openclaw.ai/gateway/trusted-proxy-auth#migration-from-token-auth)
- [Related](https://docs.openclaw.ai/gateway/trusted-proxy-auth#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

**Security-sensitive feature.** This mode delegates authentication entirely to your reverse proxy. Misconfiguration can expose your Gateway to unauthorized access. Read this page carefully before enabling.

## [​](https://docs.openclaw.ai/gateway/trusted-proxy-auth\#when-to-use)  When to use

Use `trusted-proxy` auth mode when:

- You run OpenClaw behind an **identity-aware proxy** (Pomerium, Caddy + OAuth, nginx + oauth2-proxy, Traefik + forward auth).
- Your proxy handles all authentication and passes user identity via headers.
- You’re in a Kubernetes or container environment where the proxy is the only path to the Gateway.
- You’re hitting WebSocket `1008 unauthorized` errors because browsers can’t pass tokens in WS payloads.

## [​](https://docs.openclaw.ai/gateway/trusted-proxy-auth\#when-not-to-use)  When NOT to use

- If your proxy doesn’t authenticate users (just a TLS terminator or load balancer).
- If there’s any path to the Gateway that bypasses the proxy (firewall holes, internal network access).
- If you’re unsure whether your proxy correctly strips/overwrites forwarded headers.
- If you only need personal single-user access (consider Tailscale Serve + loopback for simpler setup).

## [​](https://docs.openclaw.ai/gateway/trusted-proxy-auth\#how-it-works)  How it works

1

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Proxy authenticates the user

Your reverse proxy authenticates users (OAuth, OIDC, SAML, etc.).

2

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Proxy adds an identity header

Proxy adds a header with the authenticated user identity (e.g., `x-forwarded-user: nick@example.com`).

3

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Gateway verifies trusted source

OpenClaw checks that the request came from a **trusted proxy IP** (configured in `gateway.trustedProxies`).

4

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Gateway extracts identity

OpenClaw extracts the user identity from the configured header.

5

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Authorize

If everything checks out, the request is authorized.

## [​](https://docs.openclaw.ai/gateway/trusted-proxy-auth\#control-ui-pairing-behavior)  Control UI pairing behavior

When `gateway.auth.mode = "trusted-proxy"` is active and the request passes trusted-proxy checks, Control UI WebSocket sessions can connect without device pairing identity.Implications:

- Pairing is no longer the primary gate for Control UI access in this mode.
- Your reverse proxy auth policy and `allowUsers` become the effective access control.
- Keep gateway ingress locked to trusted proxy IPs only (`gateway.trustedProxies` \+ firewall).

## [​](https://docs.openclaw.ai/gateway/trusted-proxy-auth\#configuration)  Configuration

```
{
  gateway: {
    // Trusted-proxy auth expects requests from a non-loopback trusted proxy source by default
    bind: "lan",

    // CRITICAL: Only add your proxy's IP(s) here
    trustedProxies: ["10.0.0.1", "172.17.0.1"],

    auth: {
      mode: "trusted-proxy",
      trustedProxy: {
        // Header containing authenticated user identity (required)
        userHeader: "x-forwarded-user",

        // Optional: headers that MUST be present (proxy verification)
        requiredHeaders: ["x-forwarded-proto", "x-forwarded-host"],

        // Optional: restrict to specific users (empty = allow all)
        allowUsers: ["nick@example.com", "admin@company.org"],

        // Optional: allow a same-host loopback proxy after explicit opt-in
        allowLoopback: false,
      },
    },
  },
}
```

**Important runtime rules**

- Trusted-proxy auth rejects loopback-source requests (`127.0.0.1`, `::1`, loopback CIDRs) by default.
- Same-host loopback reverse proxies do **not** satisfy trusted-proxy auth unless you explicitly set `gateway.auth.trustedProxy.allowLoopback = true` and include the loopback address in `gateway.trustedProxies`.
- `allowLoopback` trusts local processes on the Gateway host to the same degree as the reverse proxy. Enable it only when the Gateway is still firewalled from direct remote access and the local proxy strips or overwrites client-supplied identity headers.
- Internal Gateway clients that do not travel through the reverse proxy should use `gateway.auth.password` / `OPENCLAW_GATEWAY_PASSWORD`, not trusted-proxy identity headers.
- Non-loopback Control UI deployments still need explicit `gateway.controlUi.allowedOrigins`.
- **Forwarded-header evidence overrides loopback locality for local direct fallback.** If a request arrives on loopback but carries `X-Forwarded-For` / `X-Forwarded-Host` / `X-Forwarded-Proto` headers pointing at a non-local origin, that evidence disqualifies local-direct password fallback and device-identity gating. With `allowLoopback: true`, trusted-proxy auth can still accept the request as a same-host proxy request, while `requiredHeaders` and `allowUsers` continue to apply.

### [​](https://docs.openclaw.ai/gateway/trusted-proxy-auth\#configuration-reference)  Configuration reference

[​](https://docs.openclaw.ai/gateway/trusted-proxy-auth#param-gateway-trusted-proxies)

gateway.trustedProxies

string\[\]

required

Array of proxy IP addresses to trust. Requests from other IPs are rejected.

[​](https://docs.openclaw.ai/gateway/trusted-proxy-auth#param-gateway-auth-mode)

gateway.auth.mode

string

required

Must be `"trusted-proxy"`.

[​](https://docs.openclaw.ai/gateway/trusted-proxy-auth#param-gateway-auth-trusted-proxy-user-header)

gateway.auth.trustedProxy.userHeader

string

required

Header name containing the authenticated user identity.

[​](https://docs.openclaw.ai/gateway/trusted-proxy-auth#param-gateway-auth-trusted-proxy-required-headers)

gateway.auth.trustedProxy.requiredHeaders

string\[\]

Additional headers that must be present for the request to be trusted.

[​](https://docs.openclaw.ai/gateway/trusted-proxy-auth#param-gateway-auth-trusted-proxy-allow-users)

gateway.auth.trustedProxy.allowUsers

string\[\]

Allowlist of user identities. Empty means allow all authenticated users.

[​](https://docs.openclaw.ai/gateway/trusted-proxy-auth#param-gateway-auth-trusted-proxy-allow-loopback)

gateway.auth.trustedProxy.allowLoopback

boolean

Opt-in support for same-host loopback reverse proxies. Defaults to `false`.

Only enable `allowLoopback` when the local reverse proxy is the intended trust boundary. Any local process that can connect to the Gateway can try to send proxy identity headers, so keep direct Gateway access private to the host and require proxy-owned headers such as `x-forwarded-proto` or a signed assertion header where your proxy supports one.

## [​](https://docs.openclaw.ai/gateway/trusted-proxy-auth\#tls-termination-and-hsts)  TLS termination and HSTS

Use one TLS termination point and apply HSTS there.

- Proxy TLS termination (recommended)

- Gateway TLS termination


When your reverse proxy handles HTTPS for `https://control.example.com`, set `Strict-Transport-Security` at the proxy for that domain.

- Good fit for internet-facing deployments.
- Keeps certificate + HTTP hardening policy in one place.
- OpenClaw can stay on loopback HTTP behind the proxy.

Example header value:

```
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

If OpenClaw itself serves HTTPS directly (no TLS-terminating proxy), set:

```
{
  gateway: {
    tls: { enabled: true },
    http: {
      securityHeaders: {
        strictTransportSecurity: "max-age=31536000; includeSubDomains",
      },
    },
  },
}
```

`strictTransportSecurity` accepts a string header value, or `false` to disable explicitly.

### [​](https://docs.openclaw.ai/gateway/trusted-proxy-auth\#rollout-guidance)  Rollout guidance

- Start with a short max age first (for example `max-age=300`) while validating traffic.
- Increase to long-lived values (for example `max-age=31536000`) only after confidence is high.
- Add `includeSubDomains` only if every subdomain is HTTPS-ready.
- Use preload only if you intentionally meet preload requirements for your full domain set.
- Loopback-only local development does not benefit from HSTS.

## [​](https://docs.openclaw.ai/gateway/trusted-proxy-auth\#proxy-setup-examples)  Proxy setup examples

Pomerium

Pomerium passes identity in `x-pomerium-claim-email` (or other claim headers) and a JWT in `x-pomerium-jwt-assertion`.

```
{
  gateway: {
    bind: "lan",
    trustedProxies: ["10.0.0.1"], // Pomerium's IP
    auth: {
      mode: "trusted-proxy",
      trustedProxy: {
        userHeader: "x-pomerium-claim-email",
        requiredHeaders: ["x-pomerium-jwt-assertion"],
      },
    },
  },
}
```

Pomerium config snippet:

```
routes:
  - from: https://openclaw.example.com
    to: http://openclaw-gateway:18789
    policy:
      - allow:
          or:
            - email:
                is: nick@example.com
    pass_identity_headers: true
```

Caddy with OAuth

Caddy with the `caddy-security` plugin can authenticate users and pass identity headers.

```
{
  gateway: {
    bind: "lan",
    trustedProxies: ["10.0.0.1"], // Caddy/sidecar proxy IP
    auth: {
      mode: "trusted-proxy",
      trustedProxy: {
        userHeader: "x-forwarded-user",
      },
    },
  },
}
```

Caddyfile snippet:

```
openclaw.example.com {
    authenticate with oauth2_provider
    authorize with policy1

    reverse_proxy openclaw:18789 {
        header_up X-Forwarded-User {http.auth.user.email}
    }
}
```

nginx + oauth2-proxy

oauth2-proxy authenticates users and passes identity in `x-auth-request-email`.

```
{
  gateway: {
    bind: "lan",
    trustedProxies: ["10.0.0.1"], // nginx/oauth2-proxy IP
    auth: {
      mode: "trusted-proxy",
      trustedProxy: {
        userHeader: "x-auth-request-email",
      },
    },
  },
}
```

nginx config snippet:

```
location / {
    auth_request /oauth2/auth;
    auth_request_set $user $upstream_http_x_auth_request_email;

    proxy_pass http://openclaw:18789;
    proxy_set_header X-Auth-Request-Email $user;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
}
```

Traefik with forward auth

```
{
  gateway: {
    bind: "lan",
    trustedProxies: ["172.17.0.1"], // Traefik container IP
    auth: {
      mode: "trusted-proxy",
      trustedProxy: {
        userHeader: "x-forwarded-user",
      },
    },
  },
}
```

## [​](https://docs.openclaw.ai/gateway/trusted-proxy-auth\#mixed-token-configuration)  Mixed token configuration

OpenClaw rejects ambiguous configurations where both a `gateway.auth.token` (or `OPENCLAW_GATEWAY_TOKEN`) and `trusted-proxy` mode are active at the same time. Mixed token configs can cause loopback requests to silently authenticate on the wrong auth path.If you see a `mixed_trusted_proxy_token` error on startup:

- Remove the shared token when using trusted-proxy mode, or
- Switch `gateway.auth.mode` to `"token"` if you intend token-based auth.

Loopback trusted-proxy identity headers still fail closed: same-host callers are not silently authenticated as proxy users. Internal OpenClaw callers that bypass the proxy may authenticate with `gateway.auth.password` / `OPENCLAW_GATEWAY_PASSWORD` instead. Token fallback remains intentionally unsupported in trusted-proxy mode.

## [​](https://docs.openclaw.ai/gateway/trusted-proxy-auth\#operator-scopes-header)  Operator scopes header

Trusted-proxy auth is an **identity-bearing** HTTP mode, so callers may optionally declare operator scopes with `x-openclaw-scopes`.Examples:

- `x-openclaw-scopes: operator.read`
- `x-openclaw-scopes: operator.read,operator.write`
- `x-openclaw-scopes: operator.admin,operator.write`

Behavior:

- When the header is present, OpenClaw honors the declared scope set.
- When the header is present but empty, the request declares **no** operator scopes.
- When the header is absent, normal identity-bearing HTTP APIs fall back to the standard operator default scope set.
- Gateway-auth **plugin HTTP routes** are narrower by default: when `x-openclaw-scopes` is absent, their runtime scope falls back to `operator.write`.
- Browser-origin HTTP requests still have to pass `gateway.controlUi.allowedOrigins` (or deliberate Host-header fallback mode) even after trusted-proxy auth succeeds.

Practical rule: send `x-openclaw-scopes` explicitly when you want a trusted-proxy request to be narrower than the defaults, or when a gateway-auth plugin route needs something stronger than write scope.

## [​](https://docs.openclaw.ai/gateway/trusted-proxy-auth\#security-checklist)  Security checklist

Before enabling trusted-proxy auth, verify:

- [ ] **Proxy is the only path**: The Gateway port is firewalled from everything except your proxy.
- [ ] **trustedProxies is minimal**: Only your actual proxy IPs, not entire subnets.
- [ ] **Loopback proxy source is deliberate**: trusted-proxy auth fails closed for loopback-source requests unless `gateway.auth.trustedProxy.allowLoopback` is explicitly enabled for a same-host proxy.
- [ ] **Proxy strips headers**: Your proxy overwrites (not appends) `x-forwarded-*` headers from clients.
- [ ] **TLS termination**: Your proxy handles TLS; users connect via HTTPS.
- [ ] **allowedOrigins is explicit**: Non-loopback Control UI uses explicit `gateway.controlUi.allowedOrigins`.
- [ ] **allowUsers is set** (recommended): Restrict to known users rather than allowing anyone authenticated.
- [ ] **No mixed token config**: Do not set both `gateway.auth.token` and `gateway.auth.mode: "trusted-proxy"`.
- [ ] **Local password fallback is private**: If you configure `gateway.auth.password` for internal direct callers, keep the Gateway port firewalled so non-proxy remote clients cannot reach it directly.

## [​](https://docs.openclaw.ai/gateway/trusted-proxy-auth\#security-audit)  Security audit

`openclaw security audit` will flag trusted-proxy auth with a **critical** severity finding. This is intentional — it’s a reminder that you’re delegating security to your proxy setup.The audit checks for:

- Base `gateway.trusted_proxy_auth` warning/critical reminder
- Missing `trustedProxies` configuration
- Missing `userHeader` configuration
- Empty `allowUsers` (allows any authenticated user)
- Enabled `allowLoopback` for same-host proxy sources
- Wildcard or missing browser-origin policy on exposed Control UI surfaces

## [​](https://docs.openclaw.ai/gateway/trusted-proxy-auth\#troubleshooting)  Troubleshooting

trusted\_proxy\_untrusted\_source

The request didn’t come from an IP in `gateway.trustedProxies`. Check:

- Is the proxy IP correct? (Docker container IPs can change.)
- Is there a load balancer in front of your proxy?
- Use `docker inspect` or `kubectl get pods -o wide` to find actual IPs.

trusted\_proxy\_loopback\_source

OpenClaw rejected a loopback-source trusted-proxy request.Check:

- Is the proxy connecting from `127.0.0.1` / `::1`?
- Are you trying to use trusted-proxy auth with a same-host loopback reverse proxy?

Fix:

- Prefer token/password auth for internal same-host clients that do not go through the proxy, or
- Route through a non-loopback trusted proxy address and keep that IP in `gateway.trustedProxies`, or
- For a deliberate same-host reverse proxy, set `gateway.auth.trustedProxy.allowLoopback = true`, keep the loopback address in `gateway.trustedProxies`, and make sure the proxy strips or overwrites identity headers.

trusted\_proxy\_user\_missing

The user header was empty or missing. Check:

- Is your proxy configured to pass identity headers?
- Is the header name correct? (case-insensitive, but spelling matters)
- Is the user actually authenticated at the proxy?

trusted\_proxy\_missing\_header\_\*

A required header wasn’t present. Check:

- Your proxy configuration for those specific headers.
- Whether headers are being stripped somewhere in the chain.

trusted\_proxy\_user\_not\_allowed

The user is authenticated but not in `allowUsers`. Either add them or remove the allowlist.

trusted\_proxy\_origin\_not\_allowed

Trusted-proxy auth succeeded, but the browser `Origin` header did not pass Control UI origin checks.Check:

- `gateway.controlUi.allowedOrigins` includes the exact browser origin.
- You are not relying on wildcard origins unless you intentionally want allow-all behavior.
- If you intentionally use Host-header fallback mode, `gateway.controlUi.dangerouslyAllowHostHeaderOriginFallback=true` is set deliberately.

WebSocket still failing

Make sure your proxy:

- Supports WebSocket upgrades (`Upgrade: websocket`, `Connection: upgrade`).
- Passes the identity headers on WebSocket upgrade requests (not just HTTP).
- Doesn’t have a separate auth path for WebSocket connections.

## [​](https://docs.openclaw.ai/gateway/trusted-proxy-auth\#migration-from-token-auth)  Migration from token auth

If you’re moving from token auth to trusted-proxy:

1

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Configure the proxy

Configure your proxy to authenticate users and pass headers.

2

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Test the proxy independently

Test the proxy setup independently (curl with headers).

3

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Update OpenClaw config

Update OpenClaw config with trusted-proxy auth.

4

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Restart the Gateway

Restart the Gateway.

5

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Test WebSocket

Test WebSocket connections from the Control UI.

6

[Navigate to header](https://docs.openclaw.ai/gateway/trusted-proxy-auth#)

Audit

Run `openclaw security audit` and review findings.

## [​](https://docs.openclaw.ai/gateway/trusted-proxy-auth\#related)  Related

- [Configuration](https://docs.openclaw.ai/gateway/configuration) — config reference
- [Remote access](https://docs.openclaw.ai/gateway/remote) — other remote access patterns
- [Security](https://docs.openclaw.ai/gateway/security) — full security guide
- [Tailscale](https://docs.openclaw.ai/gateway/tailscale) — simpler alternative for tailnet-only access

[Secrets apply plan contract](https://docs.openclaw.ai/gateway/secrets-plan-contract) [Health checks](https://docs.openclaw.ai/gateway/health)

Ctrl+I