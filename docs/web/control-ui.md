---
source_url: https://docs.openclaw.ai/web/control-ui
title: "Control UI - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/web/control-ui#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Web interfaces

Control UI

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Quick open (local)](https://docs.openclaw.ai/web/control-ui#quick-open-local)
- [Device pairing (first connection)](https://docs.openclaw.ai/web/control-ui#device-pairing-first-connection)
- [Personal identity (browser-local)](https://docs.openclaw.ai/web/control-ui#personal-identity-browser-local)
- [Runtime config endpoint](https://docs.openclaw.ai/web/control-ui#runtime-config-endpoint)
- [Language support](https://docs.openclaw.ai/web/control-ui#language-support)
- [Appearance themes](https://docs.openclaw.ai/web/control-ui#appearance-themes)
- [What it can do (today)](https://docs.openclaw.ai/web/control-ui#what-it-can-do-today)
- [Chat behavior](https://docs.openclaw.ai/web/control-ui#chat-behavior)
- [PWA install and web push](https://docs.openclaw.ai/web/control-ui#pwa-install-and-web-push)
- [Hosted embeds](https://docs.openclaw.ai/web/control-ui#hosted-embeds)
- [Chat message width](https://docs.openclaw.ai/web/control-ui#chat-message-width)
- [Tailnet access (recommended)](https://docs.openclaw.ai/web/control-ui#tailnet-access-recommended)
- [Insecure HTTP](https://docs.openclaw.ai/web/control-ui#insecure-http)
- [Content security policy](https://docs.openclaw.ai/web/control-ui#content-security-policy)
- [Avatar route auth](https://docs.openclaw.ai/web/control-ui#avatar-route-auth)
- [Building the UI](https://docs.openclaw.ai/web/control-ui#building-the-ui)
- [Debugging/testing: dev server + remote Gateway](https://docs.openclaw.ai/web/control-ui#debugging%2Ftesting-dev-server-%2B-remote-gateway)
- [Related](https://docs.openclaw.ai/web/control-ui#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

The Control UI is a small **Vite + Lit** single-page app served by the Gateway:

- default: `http://<host>:18789/`
- optional prefix: set `gateway.controlUi.basePath` (e.g. `/openclaw`)

It speaks **directly to the Gateway WebSocket** on the same port.

## [​](https://docs.openclaw.ai/web/control-ui\#quick-open-local)  Quick open (local)

If the Gateway is running on the same computer, open:

- [http://127.0.0.1:18789/](http://127.0.0.1:18789/) (or [http://localhost:18789/](http://localhost:18789/))

If the page fails to load, start the Gateway first: `openclaw gateway`.Auth is supplied during the WebSocket handshake via:

- `connect.params.auth.token`
- `connect.params.auth.password`
- Tailscale Serve identity headers when `gateway.auth.allowTailscale: true`
- trusted-proxy identity headers when `gateway.auth.mode: "trusted-proxy"`

The dashboard settings panel keeps a token for the current browser tab session and selected gateway URL; passwords are not persisted. Onboarding usually generates a gateway token for shared-secret auth on first connect, but password auth works too when `gateway.auth.mode` is `"password"`.

## [​](https://docs.openclaw.ai/web/control-ui\#device-pairing-first-connection)  Device pairing (first connection)

When you connect to the Control UI from a new browser or device, the Gateway usually requires a **one-time pairing approval**. This is a security measure to prevent unauthorized access.**What you’ll see:** “disconnected (1008): pairing required”

1

[Navigate to header](https://docs.openclaw.ai/web/control-ui#)

List pending requests

```
openclaw devices list
```

2

[Navigate to header](https://docs.openclaw.ai/web/control-ui#)

Approve by request ID

```
openclaw devices approve <requestId>
```

If the browser retries pairing with changed auth details (role/scopes/public key), the previous pending request is superseded and a new `requestId` is created. Re-run `openclaw devices list` before approval.If the browser is already paired and you change it from read access to write/admin access, this is treated as an approval upgrade, not a silent reconnect. OpenClaw keeps the old approval active, blocks the broader reconnect, and asks you to approve the new scope set explicitly.Once approved, the device is remembered and won’t require re-approval unless you revoke it with `openclaw devices revoke --device <id> --role <role>`. See [Devices CLI](https://docs.openclaw.ai/cli/devices) for token rotation and revocation.

- Direct local loopback browser connections (`127.0.0.1` / `localhost`) are auto-approved.
- Tailscale Serve can skip the pairing round trip for Control UI operator sessions when `gateway.auth.allowTailscale: true`, Tailscale identity verifies, and the browser presents its device identity.
- Direct Tailnet binds, LAN browser connects, and browser profiles without device identity still require explicit approval.
- Each browser profile generates a unique device ID, so switching browsers or clearing browser data will require re-pairing.

## [​](https://docs.openclaw.ai/web/control-ui\#personal-identity-browser-local)  Personal identity (browser-local)

The Control UI supports a per-browser personal identity (display name and avatar) attached to outgoing messages for attribution in shared sessions. It lives in browser storage, is scoped to the current browser profile, and is not synced to other devices or persisted server-side beyond the normal transcript authorship metadata on messages you actually send. Clearing site data or switching browsers resets it to empty.The same browser-local pattern applies to the assistant avatar override. Uploaded assistant avatars overlay the gateway-resolved identity on the local browser only and never round-trip through `config.patch`. The shared `ui.assistant.avatar` config field is still available for non-UI clients writing the field directly (such as scripted gateways or custom dashboards).

## [​](https://docs.openclaw.ai/web/control-ui\#runtime-config-endpoint)  Runtime config endpoint

The Control UI fetches its runtime settings from `/__openclaw/control-ui-config.json`. That endpoint is gated by the same gateway auth as the rest of the HTTP surface: unauthenticated browsers cannot fetch it, and a successful fetch requires either an already valid gateway token/password, Tailscale Serve identity, or a trusted-proxy identity.

## [​](https://docs.openclaw.ai/web/control-ui\#language-support)  Language support

The Control UI can localize itself on first load based on your browser locale. To override it later, open **Overview -> Gateway Access -> Language**. The locale picker lives in the Gateway Access card, not under Appearance.

- Supported locales: `en`, `zh-CN`, `zh-TW`, `pt-BR`, `de`, `es`, `ja-JP`, `ko`, `fr`, `ar`, `it`, `tr`, `uk`, `id`, `pl`, `th`, `vi`, `nl`, `fa`
- Non-English translations are lazy-loaded in the browser.
- The selected locale is saved in browser storage and reused on future visits.
- Missing translation keys fall back to English.

Docs translations are generated for the same non-English locale set, but the docs site’s built-in Mintlify language picker is limited to the locale codes Mintlify accepts. Thai (`th`) and Persian (`fa`) docs are still generated in the publish repo; they may not appear in that picker until Mintlify supports those codes.

## [​](https://docs.openclaw.ai/web/control-ui\#appearance-themes)  Appearance themes

The Appearance panel keeps the built-in Claw, Knot, and Dash themes, plus one browser-local tweakcn import slot. To import a theme, open [tweakcn themes](https://tweakcn.com/themes), choose or create a theme, click **Share**, and paste the copied theme link into Appearance. The importer also accepts `https://tweakcn.com/r/themes/<id>` registry URLs, editor URLs like `https://tweakcn.com/editor/theme?theme=amethyst-haze`, relative `/themes/<id>` paths, raw theme IDs, and default theme names such as `amethyst-haze`.Imported themes are stored only in the current browser profile. They are not written to gateway config and do not sync across devices. Replacing the imported theme updates the one local slot; clearing it switches the active theme back to Claw if the imported theme was selected.

## [​](https://docs.openclaw.ai/web/control-ui\#what-it-can-do-today)  What it can do (today)

Chat and Talk

- Chat with the model via Gateway WS (`chat.history`, `chat.send`, `chat.abort`, `chat.inject`).
- Talk through browser realtime sessions. OpenAI uses direct WebRTC, Google Live uses a constrained one-use browser token over WebSocket, and backend-only realtime voice plugins use the Gateway relay transport. The relay keeps provider credentials on the Gateway while the browser streams microphone PCM through `talk.realtime.relay*` RPCs and sends `openclaw_agent_consult` tool calls back through `chat.send` for the larger configured OpenClaw model.
- Stream tool calls + live tool output cards in Chat (agent events).

Channels, instances, sessions, dreams

- Channels: built-in plus bundled/external plugin channels status, QR login, and per-channel config (`channels.status`, `web.login.*`, `config.patch`).
- Instances: presence list + refresh (`system-presence`).
- Sessions: list + per-session model/thinking/fast/verbose/trace/reasoning overrides (`sessions.list`, `sessions.patch`).
- Dreams: dreaming status, enable/disable toggle, and Dream Diary reader (`doctor.memory.status`, `doctor.memory.dreamDiary`, `config.patch`).

Cron, skills, nodes, exec approvals

- Cron jobs: list/add/edit/run/enable/disable + run history (`cron.*`).
- Skills: status, enable/disable, install, API key updates (`skills.*`).
- Nodes: list + caps (`node.list`).
- Exec approvals: edit gateway or node allowlists + ask policy for `exec host=gateway/node` (`exec.approvals.*`).

Config

- View/edit `~/.openclaw/openclaw.json` (`config.get`, `config.set`).
- Apply + restart with validation (`config.apply`) and wake the last active session.
- Writes include a base-hash guard to prevent clobbering concurrent edits.
- Writes (`config.set`/`config.apply`/`config.patch`) preflight active SecretRef resolution for refs in the submitted config payload; unresolved active submitted refs are rejected before write.
- Schema + form rendering (`config.schema` / `config.schema.lookup`, including field `title` / `description`, matched UI hints, immediate child summaries, docs metadata on nested object/wildcard/array/composition nodes, plus plugin + channel schemas when available); Raw JSON editor is available only when the snapshot has a safe raw round-trip.
- If a snapshot cannot safely round-trip raw text, Control UI forces Form mode and disables Raw mode for that snapshot.
- Raw JSON editor “Reset to saved” preserves the raw-authored shape (formatting, comments, `$include` layout) instead of re-rendering a flattened snapshot, so external edits survive a reset when the snapshot can safely round-trip.
- Structured SecretRef object values are rendered read-only in form text inputs to prevent accidental object-to-string corruption.

Debug, logs, update

- Debug: status/health/models snapshots + event log + manual RPC calls (`status`, `health`, `models.list`).
- Logs: live tail of gateway file logs with filter/export (`logs.tail`).
- Update: run a package/git update + restart (`update.run`) with a restart report, then poll `update.status` after reconnect to verify the running gateway version.

Cron jobs panel notes

- For isolated jobs, delivery defaults to announce summary. You can switch to none if you want internal-only runs.
- Channel/target fields appear when announce is selected.
- Webhook mode uses `delivery.mode = "webhook"` with `delivery.to` set to a valid HTTP(S) webhook URL.
- For main-session jobs, webhook and none delivery modes are available.
- Advanced edit controls include delete-after-run, clear agent override, cron exact/stagger options, agent model/thinking overrides, and best-effort delivery toggles.
- Form validation is inline with field-level errors; invalid values disable the save button until fixed.
- Set `cron.webhookToken` to send a dedicated bearer token, if omitted the webhook is sent without an auth header.
- Deprecated fallback: stored legacy jobs with `notify: true` can still use `cron.webhook` until migrated.

## [​](https://docs.openclaw.ai/web/control-ui\#chat-behavior)  Chat behavior

Send and history semantics

- `chat.send` is **non-blocking**: it acks immediately with `{ runId, status: "started" }` and the response streams via `chat` events.
- Chat uploads accept images plus non-video files. Images keep the native image path; other files are stored as managed media and shown in history as attachment links.
- Re-sending with the same `idempotencyKey` returns `{ status: "in_flight" }` while running, and `{ status: "ok" }` after completion.
- `chat.history` responses are size-bounded for UI safety. When transcript entries are too large, Gateway may truncate long text fields, omit heavy metadata blocks, and replace oversized messages with a placeholder (`[chat.history omitted: message too large]`).
- Assistant/generated images are persisted as managed media references and served back through authenticated Gateway media URLs, so reloads do not depend on raw base64 image payloads staying in the chat history response.
- `chat.history` also strips display-only inline directive tags from visible assistant text (for example `[[reply_to_*]]` and `[[audio_as_voice]]`), plain-text tool-call XML payloads (including `<tool_call>...</tool_call>`, `<function_call>...</function_call>`, `<tool_calls>...</tool_calls>`, `<function_calls>...</function_calls>`, and truncated tool-call blocks), and leaked ASCII/full-width model control tokens, and omits assistant entries whose whole visible text is only the exact silent token `NO_REPLY` / `no_reply`.
- During an active send and the final history refresh, the chat view keeps local optimistic user/assistant messages visible if `chat.history` briefly returns an older snapshot; the canonical transcript replaces those local messages once the Gateway history catches up.
- `chat.inject` appends an assistant note to the session transcript and broadcasts a `chat` event for UI-only updates (no agent run, no channel delivery).
- The chat header model and thinking pickers patch the active session immediately through `sessions.patch`; they are persistent session overrides, not one-turn-only send options.
- Typing `/new` in the Control UI creates and switches to the same fresh dashboard session as New Chat. Typing `/reset` keeps the Gateway’s explicit in-place reset for the current session.
- The chat model picker requests the Gateway’s configured model view. If `agents.defaults.models` is present, that allowlist drives the picker. Otherwise the picker shows explicit `models.providers.*.models` entries plus providers with usable auth. The full catalog stays available through the debug `models.list` RPC with `view: "all"`.
- When fresh Gateway session usage reports show high context pressure, the chat composer area shows a context notice and, at recommended compaction levels, a compact button that runs the normal session compaction path. Stale token snapshots are hidden until the Gateway reports fresh usage again.

Talk mode (browser realtime)

Talk mode uses a registered realtime voice provider. Configure OpenAI with `talk.provider: "openai"` plus `talk.providers.openai.apiKey`, or configure Google with `talk.provider: "google"` plus `talk.providers.google.apiKey`; Voice Call realtime provider config can still be reused as the fallback. The browser never receives a standard provider API key. OpenAI receives an ephemeral Realtime client secret for WebRTC. Google Live receives a one-use constrained Live API auth token for a browser WebSocket session, with instructions and tool declarations locked into the token by the Gateway. Providers that only expose a backend realtime bridge run through the Gateway relay transport, so credentials and vendor sockets stay server-side while browser audio moves through authenticated Gateway RPCs. The Realtime session prompt is assembled by the Gateway; `talk.realtime.session` does not accept caller-provided instruction overrides.In the Chat composer, the Talk control is the waves button next to the microphone dictation button. When Talk starts, the composer status row shows `Connecting Talk...`, then `Talk live` while audio is connected, or `Asking OpenClaw...` while a realtime tool call is consulting the configured larger model through `chat.send`.Maintainer live smoke: `OPENAI_API_KEY=... GEMINI_API_KEY=... node --import tsx scripts/dev/realtime-talk-live-smoke.ts` verifies the OpenAI browser WebRTC SDP exchange, Google Live constrained-token browser WebSocket setup, and the Gateway relay browser adapter with fake microphone media. The command prints provider status only and does not log secrets.

Stop and abort

- Click **Stop** (calls `chat.abort`).
- While a run is active, normal follow-ups queue. Click **Steer** on a queued message to inject that follow-up into the running turn.
- Type `/stop` (or standalone abort phrases like `stop`, `stop action`, `stop run`, `stop openclaw`, `please stop`) to abort out-of-band.
- `chat.abort` supports `{ sessionKey }` (no `runId`) to abort all active runs for that session.

Abort partial retention

- When a run is aborted, partial assistant text can still be shown in the UI.
- Gateway persists aborted partial assistant text into transcript history when buffered output exists.
- Persisted entries include abort metadata so transcript consumers can tell abort partials from normal completion output.

## [​](https://docs.openclaw.ai/web/control-ui\#pwa-install-and-web-push)  PWA install and web push

The Control UI ships a `manifest.webmanifest` and a service worker, so modern browsers can install it as a standalone PWA. Web Push lets the Gateway wake the installed PWA with notifications even when the tab or browser window is not open.

| Surface | What it does |
| --- | --- |
| `ui/public/manifest.webmanifest` | PWA manifest. Browsers offer “Install app” once it is reachable. |
| `ui/public/sw.js` | Service worker that handles `push` events and notification clicks. |
| `push/vapid-keys.json` (under the OpenClaw state dir) | Auto-generated VAPID keypair used to sign Web Push payloads. |
| `push/web-push-subscriptions.json` | Persisted browser subscription endpoints. |

Override the VAPID keypair through env vars on the Gateway process when you want to pin keys (for multi-host deployments, secrets rotation, or tests):

- `OPENCLAW_VAPID_PUBLIC_KEY`
- `OPENCLAW_VAPID_PRIVATE_KEY`
- `OPENCLAW_VAPID_SUBJECT` (defaults to `mailto:openclaw@localhost`)

The Control UI uses these scope-gated Gateway methods to register and test browser subscriptions:

- `push.web.vapidPublicKey` — fetches the active VAPID public key.
- `push.web.subscribe` — registers an `endpoint` plus `keys.p256dh`/`keys.auth`.
- `push.web.unsubscribe` — removes a registered endpoint.
- `push.web.test` — sends a test notification to the caller’s subscription.

Web Push is independent of the iOS APNS relay path (see [Configuration](https://docs.openclaw.ai/gateway/configuration) for relay-backed push) and the existing `push.test` method, which target native mobile pairing.

## [​](https://docs.openclaw.ai/web/control-ui\#hosted-embeds)  Hosted embeds

Assistant messages can render hosted web content inline with the `[embed ...]` shortcode. The iframe sandbox policy is controlled by `gateway.controlUi.embedSandbox`:

- strict

- scripts (default)

- trusted


Disables script execution inside hosted embeds.

Allows interactive embeds while keeping origin isolation; this is the default and is usually enough for self-contained browser games/widgets.

Adds `allow-same-origin` on top of `allow-scripts` for same-site documents that intentionally need stronger privileges.

Example:

```
{
  gateway: {
    controlUi: {
      embedSandbox: "scripts",
    },
  },
}
```

Use `trusted` only when the embedded document genuinely needs same-origin behavior. For most agent-generated games and interactive canvases, `scripts` is the safer choice.

Absolute external `http(s)` embed URLs stay blocked by default. If you intentionally want `[embed url="https://..."]` to load third-party pages, set `gateway.controlUi.allowExternalEmbedUrls: true`.

## [​](https://docs.openclaw.ai/web/control-ui\#chat-message-width)  Chat message width

Grouped chat messages use a readable default max-width. Wide-monitor deployments can override it without patching bundled CSS by setting `gateway.controlUi.chatMessageMaxWidth`:

```
{
  gateway: {
    controlUi: {
      chatMessageMaxWidth: "min(1280px, 82%)",
    },
  },
}
```

The value is validated before it reaches the browser. Supported values include plain lengths and percentages such as `960px` or `82%`, plus constrained `min(...)`, `max(...)`, `clamp(...)`, `calc(...)`, and `fit-content(...)` width expressions.

## [​](https://docs.openclaw.ai/web/control-ui\#tailnet-access-recommended)  Tailnet access (recommended)

- Integrated Tailscale Serve (preferred)

- Bind to tailnet + token


Keep the Gateway on loopback and let Tailscale Serve proxy it with HTTPS:

```
openclaw gateway --tailscale serve
```

Open:

- `https://<magicdns>/` (or your configured `gateway.controlUi.basePath`)

By default, Control UI/WebSocket Serve requests can authenticate via Tailscale identity headers (`tailscale-user-login`) when `gateway.auth.allowTailscale` is `true`. OpenClaw verifies the identity by resolving the `x-forwarded-for` address with `tailscale whois` and matching it to the header, and only accepts these when the request hits loopback with Tailscale’s `x-forwarded-*` headers. For Control UI operator sessions with browser device identity, this verified Serve path also skips the device-pairing round trip; device-less browsers and node-role connections still follow the normal device checks. Set `gateway.auth.allowTailscale: false` if you want to require explicit shared-secret credentials even for Serve traffic. Then use `gateway.auth.mode: "token"` or `"password"`.For that async Serve identity path, failed auth attempts for the same client IP and auth scope are serialized before rate-limit writes. Concurrent bad retries from the same browser can therefore show `retry later` on the second request instead of two plain mismatches racing in parallel.

Tokenless Serve auth assumes the gateway host is trusted. If untrusted local code may run on that host, require token/password auth.

```
openclaw gateway --bind tailnet --token "$(openssl rand -hex 32)"
```

Then open:

- `http://<tailscale-ip>:18789/` (or your configured `gateway.controlUi.basePath`)

Paste the matching shared secret into the UI settings (sent as `connect.params.auth.token` or `connect.params.auth.password`).

## [​](https://docs.openclaw.ai/web/control-ui\#insecure-http)  Insecure HTTP

If you open the dashboard over plain HTTP (`http://<lan-ip>` or `http://<tailscale-ip>`), the browser runs in a **non-secure context** and blocks WebCrypto. By default, OpenClaw **blocks** Control UI connections without device identity.Documented exceptions:

- localhost-only insecure HTTP compatibility with `gateway.controlUi.allowInsecureAuth=true`
- successful operator Control UI auth through `gateway.auth.mode: "trusted-proxy"`
- break-glass `gateway.controlUi.dangerouslyDisableDeviceAuth=true`

**Recommended fix:** use HTTPS (Tailscale Serve) or open the UI locally:

- `https://<magicdns>/` (Serve)
- `http://127.0.0.1:18789/` (on the gateway host)

Insecure-auth toggle behavior

```
{
  gateway: {
    controlUi: { allowInsecureAuth: true },
    bind: "tailnet",
    auth: { mode: "token", token: "replace-me" },
  },
}
```

`allowInsecureAuth` is a local compatibility toggle only:

- It allows localhost Control UI sessions to proceed without device identity in non-secure HTTP contexts.
- It does not bypass pairing checks.
- It does not relax remote (non-localhost) device identity requirements.

Break-glass only

```
{
  gateway: {
    controlUi: { dangerouslyDisableDeviceAuth: true },
    bind: "tailnet",
    auth: { mode: "token", token: "replace-me" },
  },
}
```

`dangerouslyDisableDeviceAuth` disables Control UI device identity checks and is a severe security downgrade. Revert quickly after emergency use.

Trusted-proxy note

- Successful trusted-proxy auth can admit **operator** Control UI sessions without device identity.
- This does **not** extend to node-role Control UI sessions.
- Same-host loopback reverse proxies still do not satisfy trusted-proxy auth; see [Trusted proxy auth](https://docs.openclaw.ai/gateway/trusted-proxy-auth).

See [Tailscale](https://docs.openclaw.ai/gateway/tailscale) for HTTPS setup guidance.

## [​](https://docs.openclaw.ai/web/control-ui\#content-security-policy)  Content security policy

The Control UI ships with a tight `img-src` policy: only **same-origin** assets, `data:` URLs, and locally generated `blob:` URLs are allowed. Remote `http(s)` and protocol-relative image URLs are rejected by the browser and do not issue network fetches.What this means in practice:

- Avatars and images served under relative paths (for example `/avatars/<id>`) still render, including authenticated avatar routes that the UI fetches and converts into local `blob:` URLs.
- Inline `data:image/...` URLs still render (useful for in-protocol payloads).
- Local `blob:` URLs created by the Control UI still render.
- Remote avatar URLs emitted by channel metadata are stripped at the Control UI’s avatar helpers and replaced with the built-in logo/badge, so a compromised or malicious channel cannot force arbitrary remote image fetches from an operator browser.

You do not need to change anything to get this behavior — it is always on and not configurable.

## [​](https://docs.openclaw.ai/web/control-ui\#avatar-route-auth)  Avatar route auth

When gateway auth is configured, the Control UI avatar endpoint requires the same gateway token as the rest of the API:

- `GET /avatar/<agentId>` returns the avatar image only to authenticated callers. `GET /avatar/<agentId>?meta=1` returns the avatar metadata under the same rule.
- Unauthenticated requests to either route are rejected (matching the sibling assistant-media route). This prevents the avatar route from leaking agent identity on hosts that are otherwise protected.
- The Control UI itself forwards the gateway token as a bearer header when fetching avatars, and uses authenticated blob URLs so the image still renders in dashboards.

If you disable gateway auth (not recommended on shared hosts), the avatar route also becomes unauthenticated, in line with the rest of the gateway.

## [​](https://docs.openclaw.ai/web/control-ui\#building-the-ui)  Building the UI

The Gateway serves static files from `dist/control-ui`. Build them with:

```
pnpm ui:build
```

Optional absolute base (when you want fixed asset URLs):

```
OPENCLAW_CONTROL_UI_BASE_PATH=/openclaw/ pnpm ui:build
```

For local development (separate dev server):

```
pnpm ui:dev
```

Then point the UI at your Gateway WS URL (e.g. `ws://127.0.0.1:18789`).

## [​](https://docs.openclaw.ai/web/control-ui\#debugging/testing-dev-server-+-remote-gateway)  Debugging/testing: dev server + remote Gateway

The Control UI is static files; the WebSocket target is configurable and can be different from the HTTP origin. This is handy when you want the Vite dev server locally but the Gateway runs elsewhere.

1

[Navigate to header](https://docs.openclaw.ai/web/control-ui#)

Start the UI dev server

```
pnpm ui:dev
```

2

[Navigate to header](https://docs.openclaw.ai/web/control-ui#)

Open with gatewayUrl

```
http://localhost:5173/?gatewayUrl=ws%3A%2F%2F<gateway-host>%3A18789
```

Optional one-time auth (if needed):

```
http://localhost:5173/?gatewayUrl=wss%3A%2F%2F<gateway-host>%3A18789#token=<gateway-token>
```

Notes

- `gatewayUrl` is stored in localStorage after load and removed from the URL.
- If you pass a full `ws://` or `wss://` endpoint via `gatewayUrl`, URL-encode the `gatewayUrl` value so the browser parses the query string correctly.
- `token` should be passed via the URL fragment (`#token=...`) whenever possible. Fragments are not sent to the server, which avoids request-log and Referer leakage. Legacy `?token=` query params are still imported once for compatibility, but only as a fallback, and are stripped immediately after bootstrap.
- `password` is kept in memory only.
- When `gatewayUrl` is set, the UI does not fall back to config or environment credentials. Provide `token` (or `password`) explicitly. Missing explicit credentials is an error.
- Use `wss://` when the Gateway is behind TLS (Tailscale Serve, HTTPS proxy, etc.).
- `gatewayUrl` is only accepted in a top-level window (not embedded) to prevent clickjacking.
- Non-loopback Control UI deployments must set `gateway.controlUi.allowedOrigins` explicitly (full origins). This includes remote dev setups.
- Gateway startup may seed local origins such as `http://localhost:<port>` and `http://127.0.0.1:<port>` from the effective runtime bind and port, but remote browser origins still need explicit entries.
- Do not use `gateway.controlUi.allowedOrigins: ["*"]` except for tightly controlled local testing. It means allow any browser origin, not “match whatever host I am using.”
- `gateway.controlUi.dangerouslyAllowHostHeaderOriginFallback=true` enables Host-header origin fallback mode, but it is a dangerous security mode.

Example:

```
{
  gateway: {
    controlUi: {
      allowedOrigins: ["http://localhost:5173"],
    },
  },
}
```

Remote access setup details: [Remote access](https://docs.openclaw.ai/gateway/remote).

## [​](https://docs.openclaw.ai/web/control-ui\#related)  Related

- [Dashboard](https://docs.openclaw.ai/web/dashboard) — gateway dashboard
- [Health Checks](https://docs.openclaw.ai/gateway/health) — gateway health monitoring
- [TUI](https://docs.openclaw.ai/web/tui) — terminal user interface
- [WebChat](https://docs.openclaw.ai/web/webchat) — browser-based chat interface

[Web](https://docs.openclaw.ai/web) [Dashboard](https://docs.openclaw.ai/web/dashboard)

Ctrl+I