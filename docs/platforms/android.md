---
source_url: https://docs.openclaw.ai/platforms/android
title: "Android app - OpenClaw"
---

[Skip to main content](https://docs.openclaw.ai/platforms/android#content-area)

[OpenClaw home page![light logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)![dark logo](https://mintcdn.com/clawdhub/dpADRo8IUoiDztzJ/assets/pixel-lobster.svg?fit=max&auto=format&n=dpADRo8IUoiDztzJ&q=85&s=8fdf719fb6d3eaad7c65231385bf28e5)](https://docs.openclaw.ai/)

![US](https://d3gk2c5xim1je2.cloudfront.net/flags/US.svg)

English

Search...

Ctrl K

Search...

Navigation

Platforms overview

Android app

[Get started](https://docs.openclaw.ai/) [Install](https://docs.openclaw.ai/install) [Channels](https://docs.openclaw.ai/channels) [Agents](https://docs.openclaw.ai/concepts/architecture) [Tools & Plugins](https://docs.openclaw.ai/tools) [Models](https://docs.openclaw.ai/providers) [Platforms](https://docs.openclaw.ai/platforms) [Gateway & Ops](https://docs.openclaw.ai/gateway) [Reference](https://docs.openclaw.ai/cli) [Help](https://docs.openclaw.ai/help)

On this page

- [Support snapshot](https://docs.openclaw.ai/platforms/android#support-snapshot)
- [System control](https://docs.openclaw.ai/platforms/android#system-control)
- [Connection runbook](https://docs.openclaw.ai/platforms/android#connection-runbook)
- [Prerequisites](https://docs.openclaw.ai/platforms/android#prerequisites)
- [1) Start the Gateway](https://docs.openclaw.ai/platforms/android#1-start-the-gateway)
- [2) Verify discovery (optional)](https://docs.openclaw.ai/platforms/android#2-verify-discovery-optional)
- [Tailnet (Vienna ⇄ London) discovery via unicast DNS-SD](https://docs.openclaw.ai/platforms/android#tailnet-vienna-%E2%87%84-london-discovery-via-unicast-dns-sd)
- [3) Connect from Android](https://docs.openclaw.ai/platforms/android#3-connect-from-android)
- [Presence alive beacons](https://docs.openclaw.ai/platforms/android#presence-alive-beacons)
- [4) Approve pairing (CLI)](https://docs.openclaw.ai/platforms/android#4-approve-pairing-cli)
- [5) Verify the node is connected](https://docs.openclaw.ai/platforms/android#5-verify-the-node-is-connected)
- [6) Chat + history](https://docs.openclaw.ai/platforms/android#6-chat-%2B-history)
- [7) Canvas + camera](https://docs.openclaw.ai/platforms/android#7-canvas-%2B-camera)
- [Gateway Canvas Host (recommended for web content)](https://docs.openclaw.ai/platforms/android#gateway-canvas-host-recommended-for-web-content)
- [8) Voice + expanded Android command surface](https://docs.openclaw.ai/platforms/android#8-voice-%2B-expanded-android-command-surface)
- [Assistant entrypoints](https://docs.openclaw.ai/platforms/android#assistant-entrypoints)
- [Notification forwarding](https://docs.openclaw.ai/platforms/android#notification-forwarding)
- [Related](https://docs.openclaw.ai/platforms/android#related)

> ## Documentation Index
>
> Fetch the complete documentation index at: [https://docs.openclaw.ai/llms.txt](https://docs.openclaw.ai/llms.txt)
>
> Use this file to discover all available pages before exploring further.

The Android app has not been publicly released yet. The source code is available in the [OpenClaw repository](https://github.com/openclaw/openclaw) under `apps/android`. You can build it yourself using Java 17 and the Android SDK (`./gradlew :app:assemblePlayDebug`). See [apps/android/README.md](https://github.com/openclaw/openclaw/blob/main/apps/android/README.md) for build instructions.

## [​](https://docs.openclaw.ai/platforms/android\#support-snapshot)  Support snapshot

- Role: companion node app (Android does not host the Gateway).
- Gateway required: yes (run it on macOS, Linux, or Windows via WSL2).
- Install: [Getting Started](https://docs.openclaw.ai/start/getting-started) \+ [Pairing](https://docs.openclaw.ai/channels/pairing).
- Gateway: [Runbook](https://docs.openclaw.ai/gateway) \+ [Configuration](https://docs.openclaw.ai/gateway/configuration).

  - Protocols: [Gateway protocol](https://docs.openclaw.ai/gateway/protocol) (nodes + control plane).

## [​](https://docs.openclaw.ai/platforms/android\#system-control)  System control

System control (launchd/systemd) lives on the Gateway host. See [Gateway](https://docs.openclaw.ai/gateway).

## [​](https://docs.openclaw.ai/platforms/android\#connection-runbook)  Connection runbook

Android node app ⇄ (mDNS/NSD + WebSocket) ⇄ **Gateway**Android connects directly to the Gateway WebSocket and uses device pairing (`role: node`).For Tailscale or public hosts, Android requires a secure endpoint:

- Preferred: Tailscale Serve / Funnel with `https://<magicdns>` / `wss://<magicdns>`
- Also supported: any other `wss://` Gateway URL with a real TLS endpoint
- Cleartext `ws://` remains supported on private LAN addresses / `.local` hosts, plus `localhost`, `127.0.0.1`, and the Android emulator bridge (`10.0.2.2`)

### [​](https://docs.openclaw.ai/platforms/android\#prerequisites)  Prerequisites

- You can run the Gateway on the “master” machine.
- Android device/emulator can reach the gateway WebSocket:
  - Same LAN with mDNS/NSD, **or**
  - Same Tailscale tailnet using Wide-Area Bonjour / unicast DNS-SD (see below), **or**
  - Manual gateway host/port (fallback)
- Tailnet/public mobile pairing does **not** use raw tailnet IP `ws://` endpoints. Use Tailscale Serve or another `wss://` URL instead.
- You can run the CLI (`openclaw`) on the gateway machine (or via SSH).

### [​](https://docs.openclaw.ai/platforms/android\#1-start-the-gateway)  1) Start the Gateway

```
openclaw gateway --port 18789 --verbose
```

Confirm in logs you see something like:

- `listening on ws://0.0.0.0:18789`

For remote Android access over Tailscale, prefer Serve/Funnel instead of a raw tailnet bind:

```
openclaw gateway --tailscale serve
```

This gives Android a secure `wss://` / `https://` endpoint. A plain `gateway.bind: "tailnet"` setup is not enough for first-time remote Android pairing unless you also terminate TLS separately.

### [​](https://docs.openclaw.ai/platforms/android\#2-verify-discovery-optional)  2) Verify discovery (optional)

From the gateway machine:

```
dns-sd -B _openclaw-gw._tcp local.
```

More debugging notes: [Bonjour](https://docs.openclaw.ai/gateway/bonjour).If you also configured a wide-area discovery domain, compare against:

```
openclaw gateway discover --json
```

That shows `local.` plus the configured wide-area domain in one pass and uses the resolved
service endpoint instead of TXT-only hints.

#### [​](https://docs.openclaw.ai/platforms/android\#tailnet-vienna-%E2%87%84-london-discovery-via-unicast-dns-sd)  Tailnet (Vienna ⇄ London) discovery via unicast DNS-SD

Android NSD/mDNS discovery won’t cross networks. If your Android node and the gateway are on different networks but connected via Tailscale, use Wide-Area Bonjour / unicast DNS-SD instead.Discovery alone is not sufficient for tailnet/public Android pairing. The discovered route still needs a secure endpoint (`wss://` or Tailscale Serve):

1. Set up a DNS-SD zone (example `openclaw.internal.`) on the gateway host and publish `_openclaw-gw._tcp` records.
2. Configure Tailscale split DNS for your chosen domain pointing at that DNS server.

Details and example CoreDNS config: [Bonjour](https://docs.openclaw.ai/gateway/bonjour).

### [​](https://docs.openclaw.ai/platforms/android\#3-connect-from-android)  3) Connect from Android

In the Android app:

- The app keeps its gateway connection alive via a **foreground service** (persistent notification).
- Open the **Connect** tab.
- Use **Setup Code** or **Manual** mode.
- If discovery is blocked, use manual host/port in **Advanced controls**. For private LAN hosts, `ws://` still works. For Tailscale/public hosts, turn on TLS and use a `wss://` / Tailscale Serve endpoint.

After the first successful pairing, Android auto-reconnects on launch:

- Manual endpoint (if enabled), otherwise
- The last discovered gateway (best-effort).

### [​](https://docs.openclaw.ai/platforms/android\#presence-alive-beacons)  Presence alive beacons

After the authenticated node session connects, and when the app moves to the background while the
foreground service is still connected, Android calls `node.event` with
`event: "node.presence.alive"`. The gateway records this as `lastSeenAtMs`/`lastSeenReason` on the
paired node/device metadata only after the authenticated node device identity is known.The app counts the beacon as successfully recorded only when the gateway response includes
`handled: true`. Older gateways may acknowledge `node.event` with `{ "ok": true }`; that response is
compatible but does not count as a durable last-seen update.

### [​](https://docs.openclaw.ai/platforms/android\#4-approve-pairing-cli)  4) Approve pairing (CLI)

On the gateway machine:

```
openclaw devices list
openclaw devices approve <requestId>
openclaw devices reject <requestId>
```

Pairing details: [Pairing](https://docs.openclaw.ai/channels/pairing).Optional: if the Android node always connects from a tightly controlled subnet,
you can opt in to first-time node auto-approval with explicit CIDRs or exact IPs:

```
{
  gateway: {
    nodes: {
      pairing: {
        autoApproveCidrs: ["192.168.1.0/24"],
      },
    },
  },
}
```

This is disabled by default. It applies only to fresh `role: node` pairing with
no requested scopes. Operator/browser pairing and any role, scope, metadata, or
public-key change still require manual approval.

### [​](https://docs.openclaw.ai/platforms/android\#5-verify-the-node-is-connected)  5) Verify the node is connected

- Via nodes status:














```
openclaw nodes status
```

- Via Gateway:














```
openclaw gateway call node.list --params "{}"
```


### [​](https://docs.openclaw.ai/platforms/android\#6-chat-+-history)  6) Chat + history

The Android Chat tab supports session selection (default `main`, plus other existing sessions):

- History: `chat.history` (display-normalized; inline directive tags are
stripped from visible text, plain-text tool-call XML payloads (including
`<tool_call>...</tool_call>`, `<function_call>...</function_call>`,
`<tool_calls>...</tool_calls>`, `<function_calls>...</function_calls>`, and
truncated tool-call blocks) and leaked ASCII/full-width model control tokens
are stripped, pure silent-token assistant rows such as exact `NO_REPLY` /
`no_reply` are omitted, and oversized rows can be replaced with placeholders)
- Send: `chat.send`
- Push updates (best-effort): `chat.subscribe` → `event:"chat"`

### [​](https://docs.openclaw.ai/platforms/android\#7-canvas-+-camera)  7) Canvas + camera

#### [​](https://docs.openclaw.ai/platforms/android\#gateway-canvas-host-recommended-for-web-content)  Gateway Canvas Host (recommended for web content)

If you want the node to show real HTML/CSS/JS that the agent can edit on disk, point the node at the Gateway canvas host.

Nodes load canvas from the Gateway HTTP server (same port as `gateway.port`, default `18789`).

1. Create `~/.openclaw/workspace/canvas/index.html` on the gateway host.
2. Navigate the node to it (LAN):

```
openclaw nodes invoke --node "<Android Node>" --command canvas.navigate --params '{"url":"http://<gateway-hostname>.local:18789/__openclaw__/canvas/"}'
```

Tailnet (optional): if both devices are on Tailscale, use a MagicDNS name or tailnet IP instead of `.local`, e.g. `http://<gateway-magicdns>:18789/__openclaw__/canvas/`.This server injects a live-reload client into HTML and reloads on file changes.
The A2UI host lives at `http://<gateway-host>:18789/__openclaw__/a2ui/`.Canvas commands (foreground only):

- `canvas.eval`, `canvas.snapshot`, `canvas.navigate` (use `{"url":""}` or `{"url":"/"}` to return to the default scaffold). `canvas.snapshot` returns `{ format, base64 }` (default `format="jpeg"`).
- A2UI: `canvas.a2ui.push`, `canvas.a2ui.reset` (`canvas.a2ui.pushJSONL` legacy alias)

Camera commands (foreground only; permission-gated):

- `camera.snap` (jpg)
- `camera.clip` (mp4)

See [Camera node](https://docs.openclaw.ai/nodes/camera) for parameters and CLI helpers.

### [​](https://docs.openclaw.ai/platforms/android\#8-voice-+-expanded-android-command-surface)  8) Voice + expanded Android command surface

- Voice tab: Android has two explicit capture modes. **Mic** is a manual Voice-tab session that sends each pause as a chat turn and stops when the app leaves the foreground or the user leaves the Voice tab. **Talk** is continuous Talk Mode and keeps listening until toggled off or the node disconnects.
- Talk Mode promotes the existing foreground service from `dataSync` to `dataSync|microphone` before capture starts, then demotes it when Talk Mode stops. Android 14+ requires the `FOREGROUND_SERVICE_MICROPHONE` declaration, the `RECORD_AUDIO` runtime grant, and the microphone service type at runtime.
- Spoken replies use `talk.speak` through the configured gateway Talk provider. Local system TTS is used only when `talk.speak` is unavailable.
- Voice wake remains disabled in the Android UX/runtime.
- Additional Android command families (availability depends on device + permissions):
  - `device.status`, `device.info`, `device.permissions`, `device.health`
  - `notifications.list`, `notifications.actions` (see [Notification forwarding](https://docs.openclaw.ai/platforms/android#notification-forwarding) below)
  - `photos.latest`
  - `contacts.search`, `contacts.add`
  - `calendar.events`, `calendar.add`
  - `callLog.search`
  - `sms.search`
  - `motion.activity`, `motion.pedometer`

## [​](https://docs.openclaw.ai/platforms/android\#assistant-entrypoints)  Assistant entrypoints

Android supports launching OpenClaw from the system assistant trigger (Google
Assistant). When configured, holding the home button or saying “Hey Google, ask
OpenClaw…” opens the app and hands the prompt into the chat composer.This uses Android **App Actions** metadata declared in the app manifest. No
extra configuration is needed on the gateway side — the assistant intent is
handled entirely by the Android app and forwarded as a normal chat message.

App Actions availability depends on the device, Google Play Services version,
and whether the user has set OpenClaw as the default assistant app.

## [​](https://docs.openclaw.ai/platforms/android\#notification-forwarding)  Notification forwarding

Android can forward device notifications to the gateway as events. Several controls let you scope which notifications are forwarded and when.

| Key | Type | Description |
| --- | --- | --- |
| `notifications.allowPackages` | string\[\] | Only forward notifications from these package names. If set, all other packages are ignored. |
| `notifications.denyPackages` | string\[\] | Never forward notifications from these package names. Applied after `allowPackages`. |
| `notifications.quietHours.start` | string (HH:mm) | Start of quiet hours window (local device time). Notifications are suppressed during this window. |
| `notifications.quietHours.end` | string (HH:mm) | End of quiet hours window. |
| `notifications.rateLimit` | number | Maximum forwarded notifications per package per minute. Excess notifications are dropped. |

The notification picker also uses safer behavior for forwarded notification events, preventing accidental forwarding of sensitive system notifications.Example configuration:

```
{
  notifications: {
    allowPackages: ["com.slack", "com.whatsapp"],
    denyPackages: ["com.android.systemui"],
    quietHours: {
      start: "22:00",
      end: "07:00",
    },
    rateLimit: 5,
  },
}
```

Notification forwarding requires the Android Notification Listener permission. The app prompts for this during setup.

## [​](https://docs.openclaw.ai/platforms/android\#related)  Related

- [iOS app](https://docs.openclaw.ai/platforms/ios)
- [Nodes](https://docs.openclaw.ai/nodes)
- [Android node troubleshooting](https://docs.openclaw.ai/nodes/troubleshooting)

[Windows](https://docs.openclaw.ai/platforms/windows) [iOS app](https://docs.openclaw.ai/platforms/ios)

Ctrl+I