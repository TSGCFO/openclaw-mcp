---
source_url: https://docs.openclaw.ai/web/webchat
title: "WebChat - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/web/webchat#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Web interfaces

WebChat

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [What it is](https://docs.openclaw.ai/web/webchat#what-it-is)
- [Quick start](https://docs.openclaw.ai/web/webchat#quick-start)
- [How it works (behavior)](https://docs.openclaw.ai/web/webchat#how-it-works-behavior)
- [Control UI agents tools panel](https://docs.openclaw.ai/web/webchat#control-ui-agents-tools-panel)
- [Remote use](https://docs.openclaw.ai/web/webchat#remote-use)
- [Configuration reference (WebChat)](https://docs.openclaw.ai/web/webchat#configuration-reference-webchat)
- [Related](https://docs.openclaw.ai/web/webchat#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

Status: the macOS/iOS SwiftUI chat UI talks directly to the Gateway WebSocket.

## [​](https://docs.openclaw.ai/web/webchat\#what-it-is)  What it is

- A native chat UI for the gateway (no embedded browser and no local static server).
- Uses the same sessions and routing rules as other channels.
- Deterministic routing: replies always go back to WebChat.

## [​](https://docs.openclaw.ai/web/webchat\#quick-start)  Quick start

1. Start the gateway.
2. Open the WebChat UI (macOS/iOS app) or the Control UI chat tab.
3. Ensure a valid gateway auth path is configured (shared-secret by default,
even on loopback).

## [​](https://docs.openclaw.ai/web/webchat\#how-it-works-behavior)  How it works (behavior)

- The UI connects to the Gateway WebSocket and uses `chat.history`, `chat.send`, and `chat.inject`.
- `chat.history` is bounded for stability: Gateway may truncate long text fields, omit heavy metadata, and replace oversized entries with `[chat.history omitted: message too large]`.
- `chat.history` follows the active transcript branch for modern append-only session files, so abandoned rewrite branches and superseded prompt copies are not rendered in WebChat.
- Control UI remembers the backing Gateway `sessionId` returned by `chat.history` and includes it on follow-up `chat.send` calls, so reconnects and page refreshes continue the same stored conversation unless the user starts or resets a session.
- Control UI coalesces duplicate in-flight submits for the same session, message, and attachments before generating a new `chat.send` run id; the Gateway still dedupes repeated requests that reuse the same idempotency key.
- `chat.history` is also display-normalized: runtime-only OpenClaw context,
inbound envelope wrappers, inline delivery directive tags
such as `[[reply_to_*]]` and `[[audio_as_voice]]`, plain-text tool-call XML
payloads (including `<tool_call>...</tool_call>`,
`<function_call>...</function_call>`, `<tool_calls>...</tool_calls>`,
`<function_calls>...</function_calls>`, and truncated tool-call blocks), and
leaked ASCII/full-width model control tokens are stripped from visible text,
and assistant entries whose whole visible text is only the exact silent
token `NO_REPLY` / `no_reply` are omitted.
- Reasoning-flagged reply payloads (`isReasoning: true`) are excluded from WebChat assistant content, transcript replay text, and audio content blocks, so thinking-only payloads do not surface as visible assistant messages or playable audio.
- `chat.inject` appends an assistant note directly to the transcript and broadcasts it to the UI (no agent run).
- Aborted runs can keep partial assistant output visible in the UI.
- Gateway persists aborted partial assistant text into transcript history when buffered output exists, and marks those entries with abort metadata.
- History is always fetched from the gateway (no local file watching).
- If the gateway is unreachable, WebChat is read-only.

## [​](https://docs.openclaw.ai/web/webchat\#control-ui-agents-tools-panel)  Control UI agents tools panel

- The Control UI `/agents` Tools panel has two separate views:

  - **Available Right Now** uses `tools.effective(sessionKey=...)` and shows what the current
    session can actually use at runtime, including core, plugin, and channel-owned tools.
  - **Tool Configuration** uses `tools.catalog` and stays focused on profiles, overrides, and
    catalog semantics.
- Runtime availability is session-scoped. Switching sessions on the same agent can change the
**Available Right Now** list.
- The config editor does not imply runtime availability; effective access still follows policy
precedence (`allow`/`deny`, per-agent and provider/channel overrides).

## [​](https://docs.openclaw.ai/web/webchat\#remote-use)  Remote use

- Remote mode tunnels the gateway WebSocket over SSH/Tailscale.
- You do not need to run a separate WebChat server.

## [​](https://docs.openclaw.ai/web/webchat\#configuration-reference-webchat)  Configuration reference (WebChat)

Full configuration: [Configuration](https://docs.openclaw.ai/gateway/configuration)WebChat options:

- `gateway.webchat.chatHistoryMaxChars`: maximum character count for text fields in `chat.history` responses. When a transcript entry exceeds this limit, Gateway truncates long text fields and may replace oversized messages with a placeholder. Per-request `maxChars` can also be sent by the client to override this default for a single `chat.history` call.

Related global options:

- `gateway.port`, `gateway.bind`: WebSocket host/port.
- `gateway.auth.mode`, `gateway.auth.token`, `gateway.auth.password`:
shared-secret WebSocket auth.
- `gateway.auth.allowTailscale`: browser Control UI chat tab can use Tailscale
Serve identity headers when enabled.
- `gateway.auth.mode: "trusted-proxy"`: reverse-proxy auth for browser clients behind an identity-aware **non-loopback** proxy source (see [Trusted Proxy Auth](https://docs.openclaw.ai/gateway/trusted-proxy-auth)).
- `gateway.remote.url`, `gateway.remote.token`, `gateway.remote.password`: remote gateway target.
- `session.*`: session storage and main key defaults.

## [​](https://docs.openclaw.ai/web/webchat\#related)  Related

- [Control UI](https://docs.openclaw.ai/web/control-ui)
- [Dashboard](https://docs.openclaw.ai/web/dashboard)

[Dashboard](https://docs.openclaw.ai/web/dashboard) [TUI](https://docs.openclaw.ai/web/tui)

Ctrl+I